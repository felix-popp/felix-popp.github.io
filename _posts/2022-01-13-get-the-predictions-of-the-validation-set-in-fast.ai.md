---
layout: post
title: Get the predictions of the validation set in fast.ai
author: Felix
comments: false
tags: fast.ai
excerpt_separator: "<!--more-->"
sticky: false
hidden: false

---
Getting the predictions of a neural network in fast.ai helps assess a model's performance. For example, if 90% of all labels are identical and the model always predicts this label, the accuracy is 90%. However, that doesn't mean you have a good model. <!--more--> The following approach was taken from [here](https://forums.fast.ai/t/doing-predictions-and-showing-results-with-v2-questions-best-practice-thread/62915 "Doing predictions and showing results") with modifications.
```
    print("label, prediction, confidence, image_name")
    for index,item in enumerate(preds):
    prediction = dls.categorize.decode(np.argmax(item)).upper()
    confidence = max(item)
    percent = float(confidence)
    label = labels[index]
    print(f"{label}, {prediction}, {percent*100:.2f}, {(dls.items.iloc[[index]].filename).to_string(index=False)}")
 ```
