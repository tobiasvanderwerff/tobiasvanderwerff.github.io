---
layout: post
title:  "Using Albumentations in Detectron2"
date:   2024-04-13 21:00:00 +0100
---


I have recently been using [Detectron2](https://github.com/facebookresearch/detectron2) to train deep learning models for object detection and instance segmentation. This works great, but I found that there is one area in which Detectron2 is lacking: data augmentation. Although it has a decent [API](https://detectron2.readthedocs.io/en/latest/modules/data_transforms.html) for data transforms, it has a limited selection of useful data augmentations. Normally, a dedicated data augmentation library such as [Albumentations](https://github.com/albumentations-team/albumentations) would be my first choice for this. Unfortunately, Detectron2 does not have any integration with Albumentations, unlike similar libraries such as [mmdetection](https://github.com/open-mmlab/mmdetection/pull/1354). 

So instead, I started looking for custom ways to integrate Albumentations into Detectron2. Since Detectron2 already has its own data transforms API, I expected integration with Albumentations to be relatively straightforward. However, it turned out to be a little more complicated than I expected. Therefore, I'm sharing my findings here in case they are useful to anyone else.

## The goal: Albumentations wrapper in Detectron2

Because Detectron2 also has some useful data transformations, I would ideally like the option to use both the Detectron2 transforms API as well as the Albumentations API. It should be possible to integrate this into a standard Detectron2 pipeline. Here's what it should look like:

```python
import detectron2.data.transforms as T
import albumentations as A

augmentations = [
    T.Albumentations(A.HorizontalFlip(p=0.5)),
    T.Albumentations(A.RandomBrightnessContrast(p=0.5)),
    T.FixedSizeCrop(crop_size=(256, 256)),
]
```

Let's say you're using the standard `DatasetMapper` class from Detectron2. The augmentations could then be passed to it like this:

```python
from detectron2.data import DatasetMapper

mapper = DatasetMapper(cfg, augmentations=augmentations)
```

Or, here's an example of a custom training dataloader using the `DefaultTrainer` class:

```python
import detectron2.data.transforms as T
from detectron2.engine import DefaultTrainer
from detectron2.data import build_detection_train_loader, DatasetMapper
import albumentations as A

def get_augmentations():
    return [
        T.Albumentations(A.HorizontalFlip(p=0.5)),
        T.Albumentations(A.RandomBrightnessContrast(p=0.5)),
        T.FixedSizeCrop(crop_size=(256, 256)),
    ]

class Trainer(DefaultTrainer):
    @classmethod
    def build_train_loader(cls, cfg):
        mapper = DatasetMapper(
            cfg, is_train=True, augmentations=get_augmentations(),
        )
        return build_detection_train_loader(cfg, mapper=mapper)
```

In the next sections, I will go through an explanation of how this can be achieved. If you just want the code, skip to the [last section](#conclusion), while making sure to read the [limitations section](#limitations).


## How does Detectron2 use data transforms?

Let's start by looking at the `detectron2.data.transforms` API. Detectron2 uses two primary classes for data augmentation: `Augmentation` and `Transform` (defined [here](https://github.com/facebookresearch/detectron2/blob/b7c7f4ba82192ff06f2bbb162b9f67b00ea55867/detectron2/data/transforms/augmentation.py#L80) and [here](https://github.com/facebookresearch/fvcore/blob/1856b63f7067a987470999876d7d38ee241a6a32/fvcore/transforms/transform.py#L28)). The `Augmentation` class acts as a wrapper around the `Transform` class. The primary difference between the two classes is that calling `Transform` is expected to be deterministic, whereas `Augmentation` can be non-deterministic.

The reason for this design choice (or at least one of the reasons) appears to be that in a standard Detectron2 pipeline, data transformations are applied not in one single place, but in several. This can be seen in the `DatasetMapper` class, which takes metadata for a single image as input and maps it into a format used by the model. Here are the relevant code snippets:


```python
# From detectron2/data/dataset_mapper.py
def __call__(self, dataset_dict):
    # Note that `dataset_dict` represents Metadata of a single image

    # ...

    # Line 163
    aug_input = T.AugInput(image, sem_seg=sem_seg_gt)
    transforms = self.augmentations(aug_input)
    image, sem_seg_gt = aug_input.image, aug_input.sem_seg

    # ...

    # Line 188
    if "annotations" in dataset_dict:
        self._transform_annotations(dataset_dict, transforms, image_shape)
```

At a high level, this code shows that data transformations are applied in two places:

- Line [163-165](https://github.com/facebookresearch/detectron2/blob/b7c7f4ba82192ff06f2bbb162b9f67b00ea55867/detectron2/data/dataset_mapper.py#L163): image-level annotations (image + semantic segmentation mask)
- Line [188-189](https://github.com/facebookresearch/detectron2/blob/b7c7f4ba82192ff06f2bbb162b9f67b00ea55867/detectron2/data/dataset_mapper.py#L188): instance annotations (bounding boxes + instance segmentations)

In both these places, the same `transforms` variable is used to transform the data. Now consider the following: The data transforms need to be applied in the same way (i.e. deterministically) in order for the image and its annotations to stay consistent with each other. For example, when using a random rotation transform, if the image was rotated by 15 degrees, but the bounding boxes were rotated by 25 degrees, then clearly this would lead to inconsistency between the image and the bounding boxes. Therefore, `transforms` needs to lead to the same result each time it is applied.

Determinism also comes with drawbacks. Clearly, it is not ideal to use only deterministic transforms in a deep learning pipeline. As any deep learning practitioner knows, data augmentation is useful because it can transform data in _random_ ways, to increase the diversity of the dataset. For example, for a single image, you can randomly vary the brightness level, to make your model better at handling various brightness ranges. 

Detectron2 addresses this by defining the `Augmentation` class. Basically, this class constructs a `Transform` class instance, in a non-deterministic way. Specifically, by calling `Augmentation.get_transform()`, a new `Transform` instance is created with randomly sampled parameters. If we look back at the `DatasetMapper.__call__` definition shown above, where augmentations are applied in two separate places, the following procedure is applied under the hood: First, call `Augmentation.get_transform()` once at the beginning of the function to randomly sample a new `Transform` instance. Then, apply this transform deterministically to all input data.

## Albumentations: Slightly different

On the other hand, the Albumentations API generally applies transforms in a fully non-deterministic way. Unlike Detectron2, there is no intermediate abstraction which is used for applying transforms in a deterministic way. For example, every time you call `A.RandomCrop`, you will get a different crop. This works because the transformations are all applied in a single call. Consider the following standard Albumentations workflow:

```python
import albumentations as A

transform = A.Compose([
    A.RandomBrightnessContrast(),
    A.RandomCrop(256, 256),
], bbox_params=A.BboxParams(format='coco'))

image = ...   # image here
bboxes = ...  # bounding boxes here

transformed = transform(image=image, bboxes=bboxes)
```

Even though calling `transform` here is non-deterministic (you get a different result every time you call it), this still works correctly because all data is passed to the transform at the same time. This means all data (in this case, the image and bounding boxes) will be transformed in a way that is consistent with each other. Note that this means it is expected of the user to call `transform` only once for a single image and its annotations. For example, this would not work:

```python
# This does not work: The transform is applied with different random parameters each time it is called.
transformed1 = transform(image=image)
transformed2 = transform(bboxes=bboxes)
```

## Putting the pieces together

The non-determinism of the Albumentations API makes the integration with Detectron2 non-trivial, because it expects augmentations that can be applied several times in a deterministic way. However, when inspecting the Albumentations code more closely, it can be seen that there is actually a way to apply transformations in a deterministic way. Concretely, the Albumentations `BasicTransform` class defines a `get_params()` method ([here](https://github.com/albumentations-team/albumentations/blob/0dd39463aad7eaa42eb924121f81466b068c894b/albumentations/core/transforms_interface.py#L136)), which randomly samples the parameters of the transform it is called on. What this means is that after `get_params()` is called, the returned parameters can be passed to the transform, which leads to execution of the transform in a deterministic way. For example, here is the implementation of `get_params()` for `A.RandomCrop` ([source](https://github.com/albumentations-team/albumentations/blob/0dd39463aad7eaa42eb924121f81466b068c894b/albumentations/augmentations/crops/transforms.py#L87C4-L88C72)), which samples the `h_start` and `w_start` parameters used to define the size of the random crop:

        
```python
    def get_params(self) -> Dict[str, float]:
        return {"h_start": random.random(), "w_start": random.random()}
```

Putting all of these pieces together, we can come up with the following Albumentations wrapper in Detectron2 (thanks to [KUASWoodyLIN](https://github.com/KUASWoodyLIN) for his code contribution):

```python
class Albumentations(T.Augmentation):
    """
    Wrap an augmentation from the albumentations library:
    https://github.com/albumentations-team/albumentations/.
    Image, Bounding Box and Segmentation are supported.
    """

    def __init__(self, aug):
        """
        Args:
            aug (albumentations.BasicTransform): albumentations transform
        """
        self._aug = aug

    def get_transform(self, image):
        do = self._rand_range() < self._aug.p
        if do:
            params = self.prepare_params(image)
            h, w = image.shape[:2]
            return AlbumentationsTransform(self._aug, params, h, w)
        else:
            return NoOpTransform()

    def prepare_params(self, image):
        params = self._aug.get_params()
        if self._aug.targets_as_params:
            targets_as_params = {"image": image}
            # Get additional parameters dependent on the input image
            params_dependent_on_targets = self._aug.get_params_dependent_on_targets(
                targets_as_params
            )
            params.update(params_dependent_on_targets)
        params = self._aug.update_params(params, **{"image": image})
        return params
```

Note that `get_transform()` returns a deterministic transform, using randomly sampled parameters by calling `prepare_params`. The `AlbumentationsTransform` class is omitted here for brevity. In short, it takes the Albumentations transform along with the parameters passed to it and applies the transform in a deterministic way. The source can be found [here](https://github.com/tobiasvanderwerff/detectron2/blob/db82d1ae8dbe8fe1d47e851ea1730af6eded849b/detectron2/data/transforms/augmentation_impl.py#L740).

By using this wrapper class, we can now define Albumentations transforms as part of our regular Detectron2 pipeline, as shown in the [beginning of this post](#the-goal-albumentations-wrapper-in-detectron2).


<!-- A point of attention is to use the input format expected by `albumentations`. However, `detectron2` will convert bounding boxes to XYXY_ABS format before augmenting them. -->

## Limitations

There are some limitations to this implementation to be aware of:

- It does not work for keypoints yet. Keypoint augmentations could be added relatively easily, by implementing the `AlbumentationsTransform.apply_coords` method. The main reason I did not implement it yet is because I did not have any keypoint data to test on.
- It does not work for composite transforms, e.g. `A.Compose` or `A.OneOf`.


## Conclusion

If you want to use this implementation: I forked Detectron2 and added my changes there, which can be found [here](https://github.com/tobiasvanderwerff/detectron2). If you just want a pip install command, use the following:

```shell
pip install 'git+https://github.com/tobiasvanderwerff/detectron2.git'
```

Alternatively, you could also copy the `Albumentations` and `AlbumentationsTransform` classes as defined [here](https://github.com/tobiasvanderwerff/detectron2/blob/db82d1ae8dbe8fe1d47e851ea1730af6eded849b/detectron2/data/transforms/augmentation_impl.py#L740) and [here](https://github.com/tobiasvanderwerff/detectron2/blob/eae825324d2beeb6ed546fde04b1bbe9a17d40ca/detectron2/data/transforms/transform.py#L308) to your own project.

I've made a [pull
request](https://github.com/facebookresearch/detectron2/pull/5253#issue-2234148912) for the changes to be integrated into Detectron2, but given the recent lack of activity by maintainers, I am not sure if it will get merged. 

