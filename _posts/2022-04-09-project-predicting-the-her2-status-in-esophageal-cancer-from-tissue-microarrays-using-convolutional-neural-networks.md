---
layout: post
title: 'Project: Predicting the HER2 status in esophageal cancer from tissue microarrays
  using convolutional neural networks'
author: Felix
comments: false
tags:
- Projects
excerpt_separator: "<!--more-->"
sticky: true
hidden: true
featureImage: "/assets/figure-attention-vs-avg_brown-3.jpg"

---
We aim to develop a deep-learning method to screen microscopic images of esophageal cancer for the presence of HER2 overexpression. <!--more--> Our solution enables direct automated detection of HER2 in immunohistochemically stained tissue sections without the need for manual assessment and additional costly in situ hybridization (ISH) assays.

We trained neural networks (CNNs) on a large dataset of 1,602 patient samples and tested them on an independent set of 307 patient samples. We incorporated an attention mechanism into the CNN architecture to identify the tissue regions that the network identifies as important for the prediction.

Our method has an accuracy of 0.94, a precision of 0.97, and a recognition of 0.95. Importantly, our approach provides accurate predictions in cases that pathologists can not resolve without additional FISH testing.