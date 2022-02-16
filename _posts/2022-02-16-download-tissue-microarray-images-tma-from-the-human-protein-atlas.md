---
layout: post
title: Download tissue microarray images (TMA) from the human protein atlas
author: Felix
comments: false
tags: ''
excerpt_separator: ''
sticky: false
hidden: false

---
The aim is to download microarray tissue images of immunohistochemical staining for HER2 (ERBB2, ENSG00000141736) from the human protein atlas. Images should be from patients with gastric and esophageal cancer. <!--more-->

Enter ERBB2 in the search box on the website [proteinatlas.org](https://www.proteinatlas.org/ "https://www.proteinatlas.org/") and then click on "PATHOLOGY". Since we are only interested in images of gastric carcinoma, click on "Stomach" in the PROTEIN EXPRESSION SUMMARY view. 90 TMA images will be displayed. When viewing a specific staining intensity, such as "Low", 2 images of a patient are often displayed. It is also possible to display 4 images of a patient, e.g. for the patient with ID #664 there are 2 images with staining intensity "Low" and 2 with staining intensity "Medium".

![](/assets/tma-image-freom-the-human-protein-atlas.JPG)

We use the highly recommended R-package [HPAnalyze ](https://github.com/anhtr/HPAanalyze "HPAnalyze")by Anh N Tran to download the images. We follow the instructions from [here](https://bioc.ism.ac.jp/packages/3.10/bioc/vignettes/HPAanalyze/inst/doc/f_HPAanalyze_case_images.html "Download histology images from the Human Protein Atlas") and the original [paper](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-3059-z).

First, we download all cases with HER2 analysis and discover that 6 different antibodies were used.

    ERBB2xml <- hpaXmlGet("ENSG00000141736") 	
    
    ERBB2_ab <- hpaXmlAntibody(ERBB2xml)
    ERBB2_ab

Then we merge all cases.

    ERBB2_all <- bind_rows(ERBB2_expr)

Now, we select all gastric cancer and esophageal cancer cases.

    ERBB2_stomach <-  ERBB2_all %>% filter(tissueDescription1 == "Adenocarcinoma, NOS" &  (str_detect(tissueDescription2, "^Stomach") | tissueDescription2 == "Esophagus"))

Then we download the images

    dir.create("img")
    
    
    for (i in 1:nrow(ERBB2_stomach)) {
        download.file(ERBB2_stomach$imageUrl[i],
                      destfile = paste0("img/", ERBB2_ab$id[1], "_",
                                        ERBB2_stomach$patientId[i], "_", 
                                        ERBB2_stomach$tissueDescription1[i], "_", 
                                        ERBB2_stomach$staining[i],
                                        ".jpg"),
                      mode = "wb")
    }

This snippet downloads 37 images. We get 2 images for the patient with ID #664.

The first image with low staining intensity corresponds to the first image from the human protein atlas search.

![](/assets/cab000043_664_adenocarcinoma-nos_low.jpg)

The second image is not loaded. How to find it? Why are 37 images loaded as opposed to 90 on [proteinatlas.org](https://www.proteinatlas.org/ "https://www.proteinatlas.org/")?