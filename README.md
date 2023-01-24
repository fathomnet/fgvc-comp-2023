# FathomNet out-of-sample detection 2023
Ocean going camera systems having given scientists access to an amazing data product that allows them to monitor populations and discover new organisims. Many groups have had success training and deloying machine learning models to help sort incoming visual data. But these models typically do not generalize well to new situations -- different cameras or illumination, new organisms appearing, changes in the appearence of the seafloor -- an especially vexing problem in the dynamic ocean. Improving the robustness of these tools will allow ecologists to better leverage existing data and provide engineers with the ability to deploy instruments in ever more remote parts of the sea. 

For this competition, we have selected data from the broader [FathomNet](https://fathomnet.org/fathomnet/#/) annotated image set that represents a challenging use-case: the training is collected in the upper ocean (< 800 m) and target data are from the deep sea. The species distributions are overlapping but not identical and diverage as the vertical distance between the samples increases. The images in this dataset were collected by a single camera deployed by the [Monterey Bay Aquarium Research Institute](https://www.mbari.org/) on several of its [Remotely Operated Vehicles](https://www.mbari.org/our-work/technologies/vehicles/?visc-qf-vehicle_tax_filter_technology_type%5B%5D=remotely-operated-vehicle-rov).

<img src="https://fathomnet.org/static/m3/framegrabs/Doc%20Ricketts/images/0754/05_22_47_01.png" alt="drawing" width="800"/>

## Competition info

This competition is presented as part of the [10th Fine-Grained Visual Categorization](https://sites.google.com/view/fgvc10/home) (FGVC10) workshp at [CVPR 2023](https://cvpr.thecvf.com/Conferences/2023) in Vancouver, Canada.  

The leaderboard is hosted on Kaggle [INSERT LINK].

Please feel free to open an issue on this repo if you have a question, comment, or problem. 

## Dates
|||
|------|---------------:|
Start Date | March, 2023 |
Submission Deadline | May, 2023 | 

## Dataset details
The training and validation data provided for the competition come from above and below the [XXX m] depth horizion respectively. The above [XXX m] training set contains [XXX images] with [XXX bounding boxes]. The below [XXX m] validation set contains [XXX images] with [XXX bounding boxes]. The test set conatins images collected in the same region both above and below the depth threshold. 

All three sets of data share the same list of 290 classes. The distribution of classes in the training, validaiton, and test sets are different. Not all classes are garunteed to be in each dataset. 

## Evaluation
Submissions will be evaluated with [mean average precision](https://kaggle-metrics.readthedocs.io/en/latest/api.html#kaggle_metrics.mean_average_precision) on multilable classification of the test images. Entries should be submitted as csv file with the following format:

```
id, categories, osd
8119e2ac-ca3a-4c3b-9e1c-c7a079a705c8, "[1, 146]", 0
11e2891-93a3-4532-a4ea-6e22e335ae54, "[17, 82, 251]", 1
```

`id` is the image's `file_name` as indicated in the COCO-formatted json document. The `categories` should be an ordered list of integers denoting all detected categories. Please note that the list is blocked off in brackets *and* quotation marks. The `osd`, or out-of-sample, column is a binary prediction of whether or not the image is drawn from a different distribution relative to the training set. 

## Guidelines
Competitors are expected to only use the training images during model training; validation images are only to be used to assess model peformance. Pretrained models can be used for initalizing training (e.g. ImageNet or COCO classification or detection models provided by many deep learning packages). We do encourage participants to leveage outside data sources as they see fit. Teams should plan on specifying additional data sources and/or pretrained models used when uploading results.

## Annotation format
The annotations are formatted to adhere to the [COCO Object Detection](https://cocodataset.org/#format-data) standard. We provide a utility for converting the annotation files to YOLO format. 

## Terms of Use

By downloading and using this dataset you are agreeing to FathomNet's data use policy. In particular:

- The annotations are licensed under a Creative Commons Attribution-No Derivatives 4.0 International License. 
- The images are licensed under a Creative Commons Attribution-Non Commercial-No Derivatives 4.0 International.
- Images and annotations are provided by the copyright holders and contributors "as is" and any express or implied warranties, including, but not limited to, the implied warranties of merchantability and fitness for a particular purpose are disclaimed.
- Please acknowledge FathomNet and MBARI when using images for publication or communication purposes.

For more details please see the full [FathomNet Use Policy](https://fathomnet.org/fathomnet/#/license). 

## Download

Images are made available via a list of urls. Please access the annotation files at [URL FOR TRAIN] and [URL FOR VAL]. The download script can be run from the command line: 

```
$ python download_images.py [PATH/TO/DATASET.json] --output [PATH/TO/IMAGE/DIRECTORY]
```

If no `--output` directory is specified, the script by default downloads to the directory the command is executed from. 
