# Fairness-testing-of-Autonomous-Driving-Systems

Welcome to the online appendix for the paper entitled "Unveiling the Blind Spots: A Critical Examination of Fairness in  Autonomous Driving Systems". Here, we provide supplementary materials including raw results,  datasets and sensitive attributes labels, Python code for the analysis process, and scripts to replicate our experiments.

## Datasets

In this directory, we present the datasets used in our experiments. The "Datasets" directory is organized into two aspects: the four testing datasets we used in RQ1 (overall) and the partitioned datasets in each scenario (partitioned) in RQ2. The structure of the directory is as follows (the numbers in parentheses represent the number of images in each folder):

```
Datasets
|
|-- overall
|   |-- citypersons(#1,525)
|   |-- ecp_day (#2,427)
|   |-- ecp_night (#2,126)
|   |-- bdd100k (#2,233)
|-- partitioned
    |-- brightness
    |   |-- day and night
    |		|-- day(#4,409), night(#1,524)
    |-- contrast
    |   |-- five contrast levels
    |    	|-- level1(#1,163), level2(#3,933), level3(#837)
    |-- weather
        |-- rainy(#277)
        |-- non-rainy(#3,800)
```

All the datasets can be downloaded from the google drive link with aforementioned structure:
https://drive.google.com/drive/folders/17jwRiQ-Oum60ZUN_l2tlw2adce0Ej3gX?usp=sharing

The four benchmark testing datasets used in our experiments, as described in <u>Section 3.3.1</u>. The datasets are detailed in "*Table 2: Benchmark Datasets*" in our academic paper, providing information on the number of images and the time of capture for each dataset. 

In addition,  we also provide the partitioned datasets from the scenario labeling discussed in <u>Section 3.3.3</u>. The scripts employed for partitioning these datasets can be located in the "Scripts" directory.  RQ2 focuses on real-world key scenarios, each having its respective sub-folder: brightness, contrast, and weather. Detailed information is presented in "*Table 5: Number of images in different brightness levels, contrast levels, and weather conditions*."


## Labels

In the "Labels" directory, we provide the labels used in our experiments. This folder is divided into two main parts. The first part includes the labels for RQ1: Overall Fairness ("RQ1_overall" sub-directory), containing ground-truth labels for the four testing datasets. These labels are organized based on four testing datasets. The second part encompasses the labels for RQ2: Fairness in Different Scenarios ("RQ2_partitioned" sub-directory).  These labels are categorized based on fine-grained scenarios we discussed.

This directory can also be downloaded in the provided google drive link:
https://drive.google.com/file/d/1ufFX3E8E7ncKn0ab53A9HUb3by5_MOxV/view?usp=drive_link

### 1. RQ1 Overall Fairness (RQ1_overall)

The "Labels" folder is structured as follows. The "GT" subdirectory contains ground-truth labels for each of the four testing datasets: "bdd100k," "citypersons," "ecp_day," and "ecp_night." Within this directory, the labels are further categorized based on their attributes and stored in corresponding sub-folders.

Similarly, the "DT" subdirectory is also organized according to the four testing datasets: "bdd100k," "citypersons," "ecp_day," and "ecp_night." It contains predicted labels from eight pedestrian detectors for each dataset. The predicted labels are stored in sub-folders named after the respective detectors, such as "yolox," "retinanet," "faster rcnn," "cascade rcnn," "alfnet," "prnet," "csp," and "mgan."

```
RQ1_overall
|
|-- GT
|   |-- bdd100k
|   |   |-- gender, age, skin
|   |-- citypersons
|   |   |-- gender, age
|   |-- ecp_day
|   |   |-- gender, age
|   |-- ecp_night
|       |-- gender, age
|-- DT
    |-- bdd100k
    |   |-- yolox, retinanet, faster rcnn, cascade rcnn, alfnet, prnet, csp, mgan
    |-- citypersons
    |   |-- yolox, retinanet, faster rcnn, cascade rcnn, alfnet, prnet, csp, mgan
    |-- ecp_day
    |   |-- yolox, retinanet, faster rcnn, cascade rcnn, alfnet, prnet, csp, mgan
    |-- ecp_night
        |-- yolox, retinanet, faster rcnn, cascade rcnn, alfnet, prnet, csp, mgan
```

The GT and DT format is explained as follows:

**(1) GT  (Ground-Truth Labels)**

The "GT" directory contains ground-truth labels for each dataset, as detailed in "*Table 4: Number of labeled pedestrian instances per dataset.*" in our academic paper. We provide formulated skin tone, age and gender labels on all four testing datasets. 

Each image's ground truth label is stored in a separate TXT file within the corresponding attribute's folder. For example, in the "cp" dataset, the gender label for the image `berlin_000003_000019_leftImg8bit.png` can be found in the file `berlin_000003_000019_leftImg8bit.txt` within the `./Labels/RQ1_overall/GT/citypersons/gender/` directory. Each TXT file can contain labels for multiple individuals present in the image.

The manual labeling process was performed using LabelImg, with the original format being in YOLO format. We have formulated each label's format in a more precise way, the formulated GT label for each person is represented using a *five-digit format*, taking gender as an example:

`gender X_min Y_min X_max Y_max`

Here, "gender" denotes the gender classification, where 0 represents male and 1 represents female. For other attributes, such as skin tone (0 for light-skin, 1 for dark-skin), and age (0 for adult, 1 for child), similar conventions apply.

The values X_min, Y_min, X_max, and Y_max are used to describe the *bounding box* of the target object:

- `X_min` and `Y_min`: The coordinates of the **top-left corner** of the bounding box.

- `X_max` and `Y_max`: The coordinates of the **bottom-right corner** of the bounding box.

Our bounding boxes start at the point (`X_min`, `Y_min`) and end at the point (`X_max`, `Y_max`). Let's consider one of the sample images as an example:

![image-20230727173236222](chart/image-20230727173236222.png)

The final label for the middle person is: 

`0 (male) 787 257 (top-left corner) 1068 768 (bottom-right corner)`

**(2) DT (Detected Labels/Predicted Labels)**

The detected labels refer to the prediction information made by the 8 pedestrian detectors on the locations of "predicted person" in the four testing datasets. For each image's inference result, there is a corresponding TXT file. Within each TXT file, the organization format for the prediction result of a person is represented in a five-digit format, as follows:

`confidence_score X_min Y_min X_max Y_max`

Confidence_score represents the probability assigned by the pedestrian detector to the prediction, indicating the level of certainty that the detector has in its prediction. To ensure the reliability of the predictions, we retain only those predictions with a confidence score higher than 50%. 

`X_min`, `Y_min`, `X_max`, and `Y_max` carry the same meaning as in the GT. They denote the coordinates of the bounding box enclosing the predicted person, where `X_min` and `Y_min` correspond to the top-left corner, and `X_max` and `Y_max` correspond to the bottom-right corner of the bounding box.

### 2. RQ2 Fairness in Different Scenarios (RQ2_partitioned)

In this section, we present the classified and structured label information for RQ2 - Fairness in Different Scenarios ("RQ2_partitioned" directory). The labels has been categorized based on the classified dataset in "Dataset" part explained in the previous dataset introduction. The directory structure is as follows:

```
RQ2_partitioned
|
|-- brightness
|   |-- day_night
|       |-- GT
|        	|-- day,night
|        	 	|-- gender, age, skin
|       |-- DT
|        	|-- day,night
|        	 	|-- yolox, retinanet, faster rcnn, cascade rcnn, alfnet, prnet, csp, mgan
|-- contrast
|   |-- five_contrast_levels
|       |-- GT
|        	|-- level1,level2, level3, level4, level5
|        	 	|-- gender, age, skin
|       |-- DT
|       	|-- level1,level2, level3, level4, level5
|       	 	|-- yolox, retinanet, faster rcnn, cascade rcnn, alfnet, prnet, csp, mgan
|       |-- rms-info
|       	|-- contrast-level.txt
|-- weather
    |-- GT
        |-- rainy, non-rainy
         	|-- gender, age, skin
    |-- DT
        |-- rainy, non-rainy
         	|-- yolox, retinanet, faster rcnn, cascade rcnn, alfnet, prnet, csp, mgan
```

Within each scenario (brightness, contrast, and weather), there are corresponding "GT" and "DT" directories. The structure of the "GT" directories' meaning is identical to that in RQ1_overall, containing ground-truth labels for each attribute. The "DT" directories also store the predicted labels from the eight pedestrian detectors.  At the same time,  as mentioned in Section 3.3.3, we provide the contrast values for all images containing labeled pedestrians with a total of 5,917 images. These values can be found in the `contrast-level.txt`.

## Pedestrian Detection Models

In this section, we provide a detailed description of the experimental settings for eight pedestrian detection models as discussed in <u>Section 3.2</u> with "*Table 1: Representative pedestrian detectors.*". The experiments configuration are implemented with their corresponding open-source framework.

- MMDetection (YOLOX, RetinaNet, Faster RCNN, and Cascade RCNN): [Benchmark and Model Zoo — MMDetection 3.1.0 documentation](https://mmdetection.readthedocs.io/en/stable/model_zoo.html)

- Pedestron (CSP, MGAN): [GitHub - hasanirtiza/Pedestron: [Pedestron\] Generalizable Pedestrian Detection: The Elephant In The Room. @ CVPR2021](https://github.com/hasanirtiza/Pedestron)
- ALFNet: [GitHub - liuwei16/ALFNet: Code for 'Learning Efficient Single-stage Pedestrian Detectors by Asymptotic Localization Fitting' in ECCV2018](https://github.com/liuwei16/ALFNet)
- PRNet: [GitHub - sxlpris/PRNet: Code for "Progressive Refinement Network for Occluded Pedestrian Detection" in ECCV2020.](https://github.com/sxlpris/PRNet)

In particular, all experiments were performed on a system equipped with 64GB RAM, 2.5GHz Intel Xeon (R) v3 Dual CPUs, and one NVIDIA GeForce RTX 2080 Ti GPU. YOLOX, RetinaNet, Faster RCNN, and Cascade RCNN were implemented using PyTorch 1.8.1 and Python 3.7.10 on Ubuntu 18.04 LTS, following the MMdetection configuration. CSP and MGAN utilized PyTorch 1.10.0 and Python 3.8.10 on Ubuntu 18.04 LTS, adhering to the Pedestron configuration. Finally, ALFNet and PRNet  were implemented using Keras 2.0.6, Tensorflow 1.4.0, and Python 2.7.18 on Ubuntu 16.04 LTS following the instruction and configuration in the public framework they released.

For accessibility, all the public models we used can be downloaded from the google drive link:
https://drive.google.com/file/d/1Cys9n8_nrBsszEOt4W3RhgIkimmtlQ5q/view?usp=drive_link

The predicted results from each model can be found in the "DT" directory as previously mentioned.


## Scripts
In this section, we provide the computational code for <u>Section 4</u> of the paper.  The "evaluation" folder contains the essential code for generating Table 6, Figure 1, Figure 2, Figure 3, Table 7, Figure 4, and Table 8. Additionally, it includes a demonstration example for computing evaluation metrics and performing statistical analysis. The "scenario_split" folder offers the code used in Section 3.3.3 of the paper, specifically for partitioning datasets based on five brightness and contrast levels.  

For obtaining our results, you will only need to use the organized "Labels" folder and some of the scripts provided in these directories. The "evaluation" folder will be instrumental in generating the evaluation results and figures, while the "scenario_split" folder will help in investigating fairness in different scenarios. 

### 1. Installation

Here are the required packages.

```
pip install numpy
pip install pandas
pip install statsmodels
pip install cv2
```
You have to move this directory, and unzip the `Labels.zip` parallel to the Scripts folder. 

```
cd /Fairness-Testing-of-Autonomous-Driving-Systems/Scripts/
```

### 2. Evaluation

The script `evaluation.py` offers three selectable parameters for customization:

- `--attribute`: This parameter allows you to choose the sensitive attribute that you want to investigate. You can specify the attribute such as "gender," "age," or "skin" to focus on a particular aspect of the evaluation.
- `--gt-path`: This parameter is used to specify the input path for the ground-truth labels (GT). You need to provide the path to the GT directory that contains the labels for the chosen attribute in the "Labels" directory.
- `--dt-path`: This parameter is used to specify the paths for the eight pedestrian detectors' predicted labels (DT). You need to provide the paths to the corresponding DT directories containing eight pedestrian detector's predictions for the chosen attribute "Labels" .

By customizing these parameters, you can perform evaluations and analysis for different sensitive attributes, experiment with various ground-truth and predicted labels, and explore fairness in pedestrian detection across different scenarios.

Here is an presentation demo.

**Demo**

Suppose you want to calculate the statistics for the `skin` attribute in both the "day-time" and "night-time" scenarios, as presented in Table 7, you can execute the following commands in the terminal:

Step 1: Obtain data for the "day time" scenario:

```
python .\evaluation\evaluation.py --attribute skin --gt_path '..\Labels\RQ2_Partitioned\brightness\day_night\GT\day\skin' --dt_path '..\Labels\RQ2_Partitioned\brightness\day_night\DT\day'
```

Output:

```
========== Evaluation Results ==========

Current Testing Attribute: Skin

========== DT Type: yolox ==========

Total Samples:  2990

Light-Skin Results:
        Total: 2319
        Successful Detections: 2149
        MR: 0.0733

Dark-Skin Results:
        Total: 671
        Successful Detections: 433
        MR: 0.3547

Equal Opportunity Difference (EOD):
        -0.2814

Two Proportions Z-test (P-value):
        4.944517072987645e-78

========== DT Type: retinanet ==========

Total Samples:  2990

Light-Skin Results:
        Total: 2319
        Successful Detections: 2093
        MR: 0.0975

Dark-Skin Results:
        Total: 671
        Successful Detections: 435
        MR: 0.3517

Equal Opportunity Difference (EOD):
        -0.2543

Two Proportions Z-test (P-value):
        5.933476284573287e-58

========== DT Type: faster_rcnn ==========

Total Samples:  2990

Light-Skin Results:
        Total: 2319
        Successful Detections: 2187
        MR: 0.0569

Dark-Skin Results:
        Total: 671
        Successful Detections: 647
        MR: 0.0358

Equal Opportunity Difference (EOD):
        0.0212

Two Proportions Z-test (P-value):
        0.030003417646018847

========== DT Type: cascade_rcnn ==========

Total Samples:  2990

Light-Skin Results:
        Total: 2319
        Successful Detections: 2182
        MR: 0.0591

Dark-Skin Results:
        Total: 671
        Successful Detections: 645
        MR: 0.0387

Equal Opportunity Difference (EOD):
        0.0203

Two Proportions Z-test (P-value):
        0.04108072584895096

========== DT Type: alfnet ==========

Total Samples:  2990

Light-Skin Results:
        Total: 2319
        Successful Detections: 1452
        MR: 0.3739

Dark-Skin Results:
        Total: 671
        Successful Detections: 411
        MR: 0.3875

Equal Opportunity Difference (EOD):
        -0.0136

Two Proportions Z-test (P-value):
        0.5216329420986184

========== DT Type: csp ==========

Total Samples:  2990

Light-Skin Results:
        Total: 2319
        Successful Detections: 1016
        MR: 0.5619

Dark-Skin Results:
        Total: 671
        Successful Detections: 269
        MR: 0.5991

Equal Opportunity Difference (EOD):
        -0.0372

Two Proportions Z-test (P-value):
        0.08626407024410886

========== DT Type: mgan ==========

Total Samples:  2990

Light-Skin Results:
        Total: 2319
        Successful Detections: 1221
        MR: 0.4735

Dark-Skin Results:
        Total: 671
        Successful Detections: 337
        MR: 0.4978

Equal Opportunity Difference (EOD):
        -0.0243

Two Proportions Z-test (P-value):
        0.2674400031255866

========== DT Type: prnet ==========

Total Samples:  2990

Light-Skin Results:
        Total: 2319
        Successful Detections: 1056
        MR: 0.5446

Dark-Skin Results:
        Total: 671
        Successful Detections: 304
        MR: 0.5469

Equal Opportunity Difference (EOD):
        -0.0023

Two Proportions Z-test (P-value):
        0.9155906131938283

```

Step 2: Obtain data for the "night-time" scenario:

Run the following command in the terminal:

```
python .\evaluation\evaluation.py --attribute skin --gt_path '..\Labels\RQ2_Partitioned\brightness\day_night\GT\night\skin' --dt_path '..\Labels\RQ2_Partitioned\brightness\day_night\DT\night'
```

Output:

```
========== Evaluation Results ==========

Current Testing Attribute: Skin

========== DT Type: yolox ==========

Total Samples:  511

Light-Skin Results:
        Total: 395
        Successful Detections: 349
        MR: 0.1165

Dark-Skin Results:
        Total: 116
        Successful Detections: 49
        MR: 0.5776

Equal Opportunity Difference (EOD):
        -0.4611

Two Proportions Z-test (P-value):
        6.870922523647809e-26

========== DT Type: retinanet ==========

Total Samples:  511

Light-Skin Results:
        Total: 395
        Successful Detections: 332
        MR: 0.1595

Dark-Skin Results:
        Total: 116
        Successful Detections: 47
        MR: 0.5948

Equal Opportunity Difference (EOD):
        -0.4353

Two Proportions Z-test (P-value):
        4.602139385259241e-21

========== DT Type: faster_rcnn ==========

Total Samples:  511

Light-Skin Results:
        Total: 395
        Successful Detections: 357
        MR: 0.0962

Dark-Skin Results:
        Total: 116
        Successful Detections: 111
        MR: 0.0431

Equal Opportunity Difference (EOD):
        0.0531

Two Proportions Z-test (P-value):
        0.07010896072821972

========== DT Type: cascade_rcnn ==========

Total Samples:  511

Light-Skin Results:
        Total: 395
        Successful Detections: 352
        MR: 0.1089

Dark-Skin Results:
        Total: 116
        Successful Detections: 108
        MR: 0.0690

Equal Opportunity Difference (EOD):
        0.0399

Two Proportions Z-test (P-value):
        0.20753901838654143

========== DT Type: alfnet ==========

Total Samples:  511

Light-Skin Results:
        Total: 395
        Successful Detections: 109
        MR: 0.7241

Dark-Skin Results:
        Total: 116
        Successful Detections: 31
        MR: 0.7328

Equal Opportunity Difference (EOD):
        -0.0087

Two Proportions Z-test (P-value):
        0.8533181835733008

========== DT Type: csp ==========

Total Samples:  511

Light-Skin Results:
        Total: 395
        Successful Detections: 35
        MR: 0.9114

Dark-Skin Results:
        Total: 116
        Successful Detections: 9
        MR: 0.9224

Equal Opportunity Difference (EOD):
        -0.0110

Two Proportions Z-test (P-value):
        0.7098631497510621

========== DT Type: mgan ==========

Total Samples:  511

Light-Skin Results:
        Total: 395
        Successful Detections: 59
        MR: 0.8506

Dark-Skin Results:
        Total: 116
        Successful Detections: 19
        MR: 0.8362

Equal Opportunity Difference (EOD):
        0.0144

Two Proportions Z-test (P-value):
        0.7040691092353053

========== DT Type: prnet ==========

Total Samples:  511

Light-Skin Results:
        Total: 395
        Successful Detections: 41
        MR: 0.8962

Dark-Skin Results:
        Total: 116
        Successful Detections: 16
        MR: 0.8621

Equal Opportunity Difference (EOD):
        0.0341

Two Proportions Z-test (P-value):
        0.30454978991693227

```

Similarly, to generate the results for Table 6, Figure 1, and Figure 2 in RQ1, as well as Table 7, Figure 4, and Table 8 in RQ2, you can repeat the execution and change the paths accordingly, then record the output results. To ensure the reproducibility of the results presented in <u>Section 4</u>, we have provided a "scripts input guideline" in the "generate_scripts" folder. This folder contains various script files, such as `fig1_fig2_table6_scripts.txt`, `fig4_scripts.txt`, `fig5_scripts.txt`, `table7_scripts.txt`, and `table8_scripts.txt`, which will aid in generating and recording the corresponding results accurately.

All the recorded results, including those for RQ1 and RQ2, will be conveniently presented in the "Results" directory. 

Additionally,  we also include the necessary scripts in "fig 3" directory to generate Figure 3 from our paper. These scripts are available in the "fig 3" folder.

### 3. Scenario Split

The scripts `contrast-rms.py` serve the purpose of partitioning images into five distinct contrast levels, respectively. These scripts encompass the computation methods such as RMS contrast used for dividing the datasets. As part of our efforts, we have already organized all the images into their respective categories and made them accessible in the "Dataset" part, which already provided the source link.

## Results

Through the above steps, each run will record all the results into a spreadsheet. The final computation results for each research question are saved in their corresponding Excel files. For RQ1, all the computation details and raw data are available in the `RQ1-overall fairness.xlsx` file, which includes Table 6, Figure 1, and Figure 2. As for RQ2, the raw data for all computations are stored in separate files named `RQ2.1-brightness.xlsx` , `RQ2.2-contrast.xlsx` ,   and "`RQ2.3-weather.xlsx` , which includes Table 7, Figure 4, Table 8.

