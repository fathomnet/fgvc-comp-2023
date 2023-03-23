<img src="assets/Paragorgiidae004_trimmed_title.jpg" alt="drawing" width="1200"/>

Ocean-going camera systems have given scientists access to amazing data products that allow them to monitor populations and discover new animals. Many groups have had success training and deploying machine learning models to help sort incoming visual data. But these models typically do not generalize well to new situations -- different cameras or illumination, new organisms appearing, changes in the appearance  of the seafloor -- an especially vexing problem in the dynamic ocean. Improving the robustness of these tools will allow marine ecologists to better leverage existing data and provide engineers with the ability to deploy instruments in ever more remote parts of the ocean. 

For this competition, we have selected data from the broader [FathomNet](https://fathomnet.org/fathomnet/#/) annotated image set that represents a challenging use-case: the training set is collected in the upper ocean (< 800 m) while the target data is collected from deeper waters. This is a common scenario in ocean research: deeper waters are more difficult to access and typically more annotated data is available closer to the surface. The species distributions are often overlapping, but not identical, and diverge as the vertical distance between the samples increases. The challenge is both to identify animals in a target image and assess if the image is from a different distribution relative to the training data. Such out-of-sample detection could help scientists discover new animals and improve ecosystem management practices.  

<img src="https://fathomnet.org/static/m3/framegrabs/Doc%20Ricketts/images/0754/05_22_47_01.png" alt="drawing" width="800"/>

The images for both the training and target data were collected by a single camera system deployed by the [Monterey Bay Aquarium Research Institute](https://www.mbari.org/) (MBARI) on several of its [Remotely Operated Vehicles](https://www.mbari.org/our-work/technologies/vehicles/?visc-qf-vehicle_tax_filter_technology_type%5B%5D=remotely-operated-vehicle-rov) off the coast of Central California. We encourage participants to leverage outside data sources as they see fit. Teams should plan on specifying additional data sources and/or pretrained models used when uploading results. Pretrained models can be used for initialization of training (e.g. ImageNet or COCO classification or detection models provided by many deep learning packages).

## Competition info
This competition is part of the Fine-Grained Visual Categorization [FGVC10](https://sites.google.com/view/fgvc10) workshop at the Computer Vision and Pattern Recognition Conference [CVPR 2023](http://cvpr2023.thecvf.com/). A panel will review the top submissions for the competition based on the description of the methods provided. From this, a subset may be invited to present their results at the workshop. Attending the workshop is not required to participate in the competition; however, only teams that are attending the workshop will be considered to present their work.

There is no cash prize for this competition. PLEASE NOTE: CVPR frequently sells out early, we cannot guarantee CVPR registration after the competition's end. If you are interested in attending, please plan ahead.

You can see a list of all of the FGVC10 competitions [here](https://sites.google.com/view/fgvc10/competitions?authuser=0).

### Kaggle
The leaderboard and dataset download is hosted on [Kaggle](http://www.kaggle.com/competitions/fathomnet-out-of-sample-detection).

Please feel free to open a disscussion on the Kaggle page or an issue on this repo if you have a question, comment, or problem. 

## Dates
|||
|------|---------------:|
Start Date | March 23, 2023 |
Submission Deadline | May 23, 2023 | 

## Dataset details
The training and test images for the competition were all collected in the Monterey Bay Area between the surface and 1300 meters depth. The images contain bounding box annotations of 290 categories of bottom dwelling animals. The training and validation data are split across an 800 meter depth threshold: all training data is collected from 0-800 meters, evaluation data comes from the whole 0-1300 meter range. Since an organisms' habitat range is partially a function of depth, the species distributions in the two regions are overlapping but not identical. Test images are drawn from the same region but may come from above or below the depth horizon. 

The competition goal is to label the animals present in a given image (i.e. multi-label classification) and determine whether the image is out-of-sample.

### Data format
The training dataset are provide in two different formats: multi-label classification and object detection. The different formats live in the corresponding named directories. The datasets in these directories are identical aside from how they are organized. 

#### Multi-label classification
Each line of the csv files indicates an image by its `id` and a list of corresponding `categories` present in the frame. 
```
id, categories
4a7f2199-772d-486d-b8e2-b651246316b5, [1.0]
3bddedf6-4ff8-4e81-876a-564d2b03b364, "[1.0, 9.0, 11.0, 88.0]"
3f735021-f5de-4168-b139-74bf2859d12a, "[1.0, 37.0, 51.0, 119.0]"
130e185f-09c5-490c-8d08-641c4cbf6e54, "[1.0, 51.0, 119.0]"
```
The `ids` correspond those in the object detection files. The `categories` are the set of all unique annotations found in the associated image.

#### Object detection
The datasets are formatted to adhere to the [COCO Object Detection](https://cocodataset.org/#format-data) standard. Every training image contains at least one annotation corresponding to a `category_id` ranging from 1 to 290 and a `supercategory` from 1 to 20. The fine-grained annotations are taxonomic thought not always at the same level of the taxonomic tree. 

#### Supercategories
Each category also belongs to one of 20 semantic supercategories as indicated in `category_key.csv`:
```
['Worm', 'Feather star', 'Sea cucumber', 'Squat lobster', 'Fish', 'Soft coral', 'Urchin', 'Sea star', 'Sea fan', 'Sea pen', 'Barnacle', 'Eel', 'Glass sponge', 'Shrimp', 'Black coral', 'Anemone', 'Sea spider', 'Gastropod', 'Crab', 'Stony coral']
```
These supercategories might be useful for certain training procedures. The supercategories *are* represented in both the training and validation set. But please note that submissions must be made identifying the 290 fine grained categories. 

We are not able to provide images as a single downloadable archive due to [FathomNet's Terms of Use](https://fathomnet.org/fathomnet/#/license). Images should be downloaded using the indicated `coco_url` field in each of the annotation files. Participants can either write their own script or use the provided `download_images.py` python script. 

## Evaluation
For each image in the test set, you will predict a list of *categories* belonging to the image and classify whether the image is *out-of-sample*.

The out-of-sample predictions are evaluated by the [AUCROC](https://developers.google.com/machine-learning/crash-course/classification/roc-and-auc) score, rescaled as follows:

$$sAUC = 2 AUC - 1$$

Predictions of categories are evaluated according to the Mean Average Precision at 20 (MAP@20):

$$MAP@20 = \frac{1}{U} \sum\_{u=1}^{U} \sum\_{k=1}^{min(n,20)} P(k) \times rel(k)$$

where \\( U \\)  is the number of images, \\( P(k) \\) is the precision at cutoff \\( k \\), \\( n \\) is the number predictions per image, and \\( rel(k) \\) is an indicator function equaling 1 if the item at rank \\( k \\) is a relevant (correct) label, zero otherwise.

Once a correct label has been scored for *an observation*, that label is no longer considered relevant for that observation, and additional predictions of that label are skipped in the calculation. For example, if the correct label is `1` for an observation, the following predictions all score an average precision of `1.0`.

    [1, 2, 3, 4, 5]
    [1, 1, 1, 1, 1]
    [1, 2, 1, 3, 1]

The final score is the simple average:

$$\frac{1}{2} \left( sAUC + MAP@20 \right)$$ 

### Submission Format
Entries should be submitted as a csv file with each line representing a single image. The image uuid should be followed by all detected `categories` and an `osd` score:

```
id,categories,osd
8119e2ac-ca3a-4c3b-9e1c-c7a079a705c8,1 146 10 12 44 210,0.1
11e2891-93a3-4532-a4ea-6e22e335ae54,17 82 251,0.9
...
```

The `id` is the image's `file_name` as indicated in the annotation files. The `categories` should be a space-delimited list of integer codes ordered by the probability the category is present in the image; this list will be automatically truncated after 20 entries. The `osd` field should be a probability or confidence score of whether the image is drawn from a different distribution relative to the training set.

## Guidelines
Pretrained models can be used for initalizing training (e.g. ImageNet or COCO classification or detection models provided by many deep learning packages). We do encourage participants to leveage outside data sources as they see fit. Teams should plan on specifying additional data sources and/or pretrained models used when uploading results.

## Terms of Use
By downloading and using this dataset you are agreeing to FathomNet's data use policy. In particular:

- The annotations are licensed under a Creative Commons Attribution-No Derivatives 4.0 International License. 
- The images are licensed under a Creative Commons Attribution-Non Commercial-No Derivatives 4.0 International License. Notwithstanding the foregoing, all of the images may be used for training and development of machine learning algorithms for commercial, academic, and government purposes.
- Images and annotations are provided by the copyright holders and contributors "as is" and any express or implied warranties, including, but not limited to, the implied warranties of merchantability and fitness for a particular purpose are disclaimed.
- Please acknowledge FathomNet and MBARI when using images for publication or communication purposes regarding this competition. For all other uses of the images, users should contact the original copyright holder.

For more details please see the full [FathomNet Use Policy](https://fathomnet.org/fathomnet/#/license). 

### FathomNet citation
[FathomNet](https://doi.org/10.1038/s41598-022-19939-2)
```
@article{katija2022fathomnet,
  title={FathomNet: A global image database for enabling artificial intelligence in the ocean},
  author={Katija, Kakani and Orenstein, Eric and Schlining, Brian and Lundsten, Lonny and Barnard, Kevin and Sainz, Giovanna and Boulais, Oceane and Cromwell, Megan and Butler, Erin and Woodward, Benjamin and others},
  journal={Scientific reports},
  volume={12},
  number={1},
  pages={15914},
  year={2022},
  publisher={Nature Publishing Group UK London}
}
```

## Download
Images are made available for download by the unique URLs in the COCO-formatted object detection annotation files. The download script can be run from the command line: 

```
$ python download_images.py [PATH/TO/DATASET.json] --output [PATH/TO/IMAGE/DIRECTORY]
```

If no `--output` directory is specified, the script by default downloads images to the directory the command is executed from. 

## Acknowledgements
The images for this competition has been generously provided by [MBARI](https://www.mbari.org/). Annotations were produced by experts in the [MBARI Video Lab](https://doi.org/10.1109/OCEANS.2006.306879).
