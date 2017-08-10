---
layout: post
title:  "Why Core ML will not work for your app (most likely)"
date:   2017-06-09 23:42:00 +0200
categories: ML
excerpt: While the buzz around newly released Apple framework is loud, I want to explain several things, that may not be obvious for those who are new to machine learning (ML).
comments: true
---

> **Update: August 5, 2017**
> 
> After integrating Core ML into several apps I've decided to update the original post with some new thoughts and observations. Apple had recently introduced a workaround for some of the issues, mentioned further by adding new API for compiling models on device.

While the buzz around newly released Apple framework is loud, I want to explain several things, that may not be obvious for those who are new to machine learning (ML).

## What is Core ML




## Core ML is not a machine learning framework

Yes, in opposite to what have been proclaimed from the WWDC scene many times, Core ML is not a "machine learning framework." Why? If you ask any ML practitioner to name several ML frameworks, most likely they will recall [scikit-learn](http://scikit-learn.org/), [TensorFlow](https://www.tensorflow.org/), [Theano](http://deeplearning.net/software/theano/), [Spark MLlib](https://spark.apache.org/docs/latest/ml-guide.html), maybe [Weka](http://www.cs.waikato.ac.nz/ml/weka/) if they are old enough. All those software have one thing in common - they can train models using data. Then, some models (not all of them) can do inference, or simply speaking, predict things.

**Core ML can't train models.** This sounds funny, but the "New machine *learning* framework" was shipped *without any learning* in it. At its current stage, Core ML is a bunch of conversion scripts + new model format + Xcode integration. It is not even an ML acceleration framework like CuDNN, because those part is on Metal performance shaders and BNNS. To call Core ML an ML framework is almost the same as to call Caffe to Keras conversion script an ML framework.

These are actual *machine learning frameworks* mentioned in the WWDC keynote:

![](/images/to_coreml_or_not/frameworks.png)

<font size="-1"> Except for Turi, perhaps. To be honest, I have no idea, what it is and there are no references to them in Core ML documentation. According to news sites, Turi was an ML startup until Apple aquaried them. Maybe they have some plans to release their own ML framework or something.</font> 

Apple released the Python package called [coremltools](https://pypi.python.org/pypi/coremltools) which is essentially a [bunch of scripts to convert models](https://apple.github.io/coremltools/index.html) from framework-specific formats into Core ML format \(`*.mlmodel`\). Coremltools can take some types of pretrained models, convert them to its own format and then one can use those converted models to make predictions.

## Core ML supports only two types of ML

Namely, [regression and classification](http://pythonhosted.org/coremltools/index.html#conversion-support). While classification is arguably the most popular ML task, there are many others: clustering, ranking, dimensionality reduction, structure prediction, anomaly and novelty detection, rule mining, denoising, data compression, representation learning, reinforcement learning and so on. All this is *mostly* behind the scope of Core ML. 
<br>![](/images/to_coreml_or_not/coreml.png)

## You can not dynamically update the model in the runtime

Model lives in your app bundle and Xcode generates a Swift class for it. So it's almost like .xcdatamodel file of CoreData. You can't change model based on user's input. You can't have personalized model.

## <s>You can not replace the model without releasing the new version of your app to the AppStore.</s> 
 
<s>You can't download the latest version of your model from your server.</s> 

Hacky workaround:

0. (optional) Switch to Xcode beta `sudo xcode-select --switch /Applications/Xcode-beta.app/Contents/Developer`.
1. Compile your model: `xcrun coremlcompiler compile /path/to/MyModel.mlmodel /path/to/output/folder`.
2. Put compiled model folder `MyModel.mlmodelc` into your app bundle.
3. Add auto-generated swift model class (`MyModel.swift`) to your project manually and annotate it with `@available(iOS 11.0, *)`.
[![How to find model class][1]][1]
4. Load and initialize your model:

    let path = Bundle.main.path(forResource: "MyModel", ofType: "mlmodelc")

    let url = URL(fileURLWithPath: path!)

    let model = try! MyModel(contentsOf: url)

**Warning:** I haven't tried to upload such app to the AppStore.
I’ve tried it on my test device, and it works, I’m just not sure if it continues working after releasing to the Appstore.

**Upd. 07/25/17:** Apple just have introduced [new API](https://developer.apple.com/documentation/coreml/mlmodel/2921516-compilemodel) for compiling models on device. This means, that you can avoid steps 1-4 now.

## If your models are proprietary or contain sensitive information, you can't use CoreML

After the compilation, the model file is not encrypted or in other way secured. For instance, Caffe neural network gets converted to several JSON files with layers description and binaries with weights data. Anyone with an archiver can open the .ipa file and inspect the structure of your model. Again, you can use a workaround like in 4.

![](/images/to_coreml_or_not/reverse.png)

## Core ML doesn't solve the privacy concerns

To train the model user's data have to be collected and uploaded to the servers. Core ML doesn't address this in any way. Even worse, [Core ML allows to mine user's private info](https://medium.com/@privacy.no.name/coreml-allows-stealing-your-private-info-6x-faster-than-before-b4f180031c03) from his/her photo albums (including "hidden"). Looks like a real problem.

## Core ML doesn't compress models

Modern neural networks can easily be hundreds of MB and even GBs. Model compression is something that you will have to sort out on your own. By "model compression" I understand techniques like learning distillation, network pruning, quantization etc, not just archiving.
Actually, we don't know what coremltools does under the hood. Maybe it does compression for some cases. For example, my Keras model was 3.9 Mb before conversion and 1.3 Mb after conversion. Since now Core ML allows dynamically compile models, you can compress and uncompress models yourself.

## Core ML is not the only way to do machine learning on iOS

There are [tens of libraries and frameworks](http://alexsosn.github.io/ml/2015/11/05/iOS-ML.html) that are free of these limitations and compatible with iOS.

## Core ML is a black box

After using Core ML for deep learning neural networks in commercial project, I can say, that perhaps the biggest problem of Core ML is its opacity. You can't check what's happening inside your model. You dont know how layers got translated into code. If the output after conversion is different from the results before conversion, you can't do anything, axcept complaining at the forums.

## Core ML is not easy to integrate with new libraries


## Conclusion 

So, where can Core ML be useful? In any app, that does pattern recognition using open source models and are okay with the above limitations. I'm pretty sure there are a lot of use cases and ... looking forward to seeing a lot of new dog-breed-recognizing apps.😄

Finally, friendly advice to all my fellow iOS developers, who got excited about Core ML presentation:

**Learn Machine Learning, not Core ML.**


P.S. If you want to add machine learning capabilities to your iOS app, I can help. [Contact me](), I'm doing this for at least 3 years now.


--

P.S. Feel free to point out if I've missed something.