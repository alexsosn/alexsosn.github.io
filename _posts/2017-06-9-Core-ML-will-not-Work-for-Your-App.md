---
layout: post
title:  "Why Core ML will not work for your app (most likely)"
date:   2017-06-09 23:42:00 +0200
categories: ML
excerpt: While the buzz around newly released Apple framework is loud, I want to explain several things, that may not be obvious for those who are new to machine learning (ML).
comments: true
---

While the buzz around newly released Apple framework is loud, I want to explain several things, that may not be obvious for those who are new to machine learning (ML).

1. **Core ML is not a machine learning framework.**
Yes, in opposite to what have been proclaimed from the WWDC scene many times, Core ML is not a "machine learning framework." Why? If you ask any ML practitioner to name several ML frameworks, most likely they will recall ScikitLearn, TensorFlow, Theano, SparkML, maybe Weka if they are old enough. All those frameworks can train models on data. Then, some models can do inference, or simply speaking, predict things.
Core ML can't take data and can't train models. It can take some types of trained models, convert them to its own format and then make predictions. These are *machine learning frameworks* mentioned in the keynote:
<br>![Except Turi, perhaps. To be honest, I have no idea, who they are.](/images/to_coreml_or_not/frameworks.png)

2. **Core ML supports only two types of ML.**
Namely, [regression and classification](http://pythonhosted.org/coremltools/index.html#conversion-support). While classification is arguably the most popular ML task, there are many others: clustering, ranking, dimensionality reduction, density estimation, structure prediction, anomaly and novelty detection, rule mining, denoising, data compression, representation learning, reinforcement learning and so on. All this is mostly behind the scope of Core ML. 
<br>![](/images/to_coreml_or_not/coreml.png)

3. **You can not update the model in the runtime.**
Model lives in your app bundle and Xcode generates a Swift class for it. So it's almost like .xcdatamodel file of CoreData. You can't change model based on user's input. You can't have personalized model.

4. <s><b>You can not replace the model without releasing the new version of your app to the AppStore.</b> You can't download the latest version of your model from your server.</s> Looks like there is a workaround: (1) put the model's swift file into the target, (2) compile new model from .mlmodel to .mlmodelc without changing its interface, (3) put those sources to the server, (4) download them from inside of your app, (5) initialize new model using `YourModelClass.init(contentsOf: URL)` method. I've tried it on my test device, and it works, I'm just not sure if it continues working after releasing to the Appstore. And this is definetely not the way Core ML is designed to be used.

5. **If your models are proprietary or contain sensitive information, you can't use CoreML.**
After the compilation, the model file is not encrypted or in other way secured. For instance, Caffe neural network gets converted to several JSON files with layers description and binaries with weights data. Anyone with an archiver can open the .ipa file and inspect the structure of your model. Again, you can use a workaround like in 4.
<br>![](/images/to_coreml_or_not/reverse.png)

6. **Core ML doesn't solve the privacy concerns.**
To train the model user's data have to be collected and uploaded to the servers. Core ML doesn't address this in any way.

7. **Core ML doesn't compress models.**
Modern neural networks can easily be hundreds of MB and even GBs. Model compression is something that you will have to sort out on your own. By "model compression" I understand techniques like learning distillation, network pruning, quantization etc, not just archiving.

8. **Core ML is not the only way to do machine learning on iOS.** There are [tens of libraries and frameworks](http://alexsosn.github.io/ml/2015/11/05/iOS-ML.html) that are free of these limitations and compatible with iOS.

So, where can Core ML be useful? In any app, that does pattern recognition using open source models and are okay with the above limitations. I'm pretty sure there are a lot of use cases and ... looking forward to seeing a lot of new dog-breed-recognizing apps.😄

Finally, friendly advice to all my fellow iOS developers, who got excited about Core ML presentation:

**Learn Machine Learning, not Core ML.**

--

P.S. Feel free to point out if I've missed something.