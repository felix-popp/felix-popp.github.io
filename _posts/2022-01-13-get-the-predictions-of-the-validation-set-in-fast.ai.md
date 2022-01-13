---
layout: post
title: Get the predictions of the validation set in fast.ai
author: Felix
comments: false
tags: fast.ai
excerpt_separator: ''
sticky: false
hidden: false

---
Getting the predictions after running a model in fast.ai helps to assess the plausibility of the model. For example, if 90% of all labels are the same, the accuracy is likely better than 90%. However, that doesn't mean you have a good model. The following approach was taken from [here](https://forums.fast.ai/t/doing-predictions-and-showing-results-with-v2-questions-best-practice-thread/62915 "Doing predictions and showing results") and slightly modiefied.

    preds, labels = learn.get_preds(ds_idx=1)
    for index,item in enumerate(preds):
        prediction = dls.categorize.decode(np.argmax(item)).upper()
        confidence = max(item)
        percent = float(confidence)
        label = labels[index]
        print(f"{label}   {prediction}   {percent*100:.2f}% confidence.   Image = {dl.items[index].name}")