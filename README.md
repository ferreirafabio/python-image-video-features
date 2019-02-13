[![Downloads](http://pepy.tech/badge/videofeatures)](http://pepy.tech/count/videofeatures)

# Description
This package implements the feature extraction from video or image (currently supported: **ResNet**, **VGG**, **SIFT** and **SURF**) and based on these features, the training of a Fisher Vector GMM and computation of (improved) Fisher Vectors. A perhaps useful application of this implementation is to set-up a feature extractor baseline and compare one's approach to both FV and standard computer vision features.
In particular, the package covers the following features:
1) **extraction**, exporting and restoring **of several features from videos** or image data
2) **training a GMM for a Fisher Vector (FVGMM)** encoding based on the features, then exporting the FVGMM model parameters
3) **computation of (improved) Fisher Vectors** from the FVGMM and features

For representation of datasets, we use the convenient and straightforward GulpIO storage format. The above mentioned feature extractors are ready-to-use with this package. 

# Installation and import
```
$ pip install videofeatures
```
then import the package with
```
from videofeatures import Pipeline, ActivityNetDataset, ResNetFeatures
```

# First steps
### 1. Setting up the dataset
It is very straightforward to bring your dataset into the right 'gulp' format. The GulpIO documentation gets you started quickly [1]. If you're using one of the prevalent datasets, e.g. ActivityNet, Kinetics or TwentyBN-something-something, it's even simpler to get you started - simply use the available adapter [2] to gulp your local files. 

### 2. Initialization
First, we instantiate both dataset (here ActivityNet) and extractor (ResNet)
```
activitynet = ActivityNetDataset(batch_size=20, train_dir=path_train, valid_dir=path_train).getDataLoader(train=True)
resnet = ResNetFeatures()
pipeline = Pipeline(dataset=activitynet, extractor=resnet, base_dir="./output")
```

### 3. Feature extraction, GMM training and FV computation
Having initialized the pipeline, we can do
```
features, labels = pipeline.extractFeatures()
fisher_vector_gmm = pipeline.trainFisherVectorGMM(features)
fisher_vectors, labels = pipeline.computeFisherVectors(features=features, labels=labels, fv_gmm=fisher_vector_gmm)
```

### Full example
A full example can be viewed in the following gist:
https://gist.github.com/ferreirafabio/60323a87ba80c052ab272ff769149577

# Available imports
if you want to use 
* VGG, SIFT and SURF features: `from videofeatures import VGGFeatures, SIFTFeatures, SURFFeatures` 
* TwentyBNDataset: `from videofeatures import TwentyBNDataset`

Contributors:
* Jonas Rothfuss (https://github.com/jonasrothfuss/)
* Fabio Ferreira (https://github.com/ferreirafabio/)

[1] https://github.com/TwentyBN/GulpIO
[2] https://github.com/TwentyBN/GulpIO/blob/master/src/main/python/gulpio/adapters.py
