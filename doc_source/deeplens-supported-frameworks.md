# Machine Learning Frameworks Supported by AWS DeepLens<a name="deeplens-supported-frameworks"></a>

Currently, AWS DeepLens supports only models created with the Apache MXNet framework\.


+ [Apache MXNet Models](#deeplens-supported-frameworks-mxnet)
+ [Gluon Models](#deeplens-supported-frameworks-gluon)

## Apache MXNet Models<a name="deeplens-supported-frameworks-mxnet"></a>

AWS DeepLens supports the following MXNet layers\.


| Supported MXNet Layers | 
| --- | 
|   Activation   | 
|   BatchNorm   | 
|   Concat   | 
|   \_contrib\_MultiBoxDetection   | 
|   \_contrib\_MultiBoxPrior   | 
|   Convolution   | 
|   Deconvolution   | 
|   elemwise\_add   | 
|   Flatten   | 
|   FullyConnected   | 
|   InputLayer   | 
|   L2Norm   | 
|  LRN \( Local Response Normalization \)  | 
|   Pooling   | 
|   Reshape   | 
|   ScaleShift   | 
|   SoftmaxActivation   | 
|   SoftmaxOutput   | 
|   transpose   | 
|   UpSampling   | 
|   \_mul   | 
|   \_Plus   | 

 For more information about MXNet, see [Apache MXNet](https://mxnet.apache.org/)\.

## Gluon Models<a name="deeplens-supported-frameworks-gluon"></a>

Gluon is an API within MXNet that you use to access MXNet models\.

**Example**  
The following example shows how to export your Squeezenet version 1 model using the Gluon API\. The output is a symbol and params file with 'squeezenet' as the filename prefix\.  

```
import mxnet as mx
from mxnet.gluon.model_zoo import vision
squeezenet = vision.squeezenet_v1(pretrained=True, ctx=mx.cpu())

# to export you need to hybridize your gluon model
squeezenet.hybridize()

# squeezenet’s input pattern is images 224x224, so prep a fake image
fake_image = mx.nd.random.uniform(shape=(1,3,224,224), ctx=mx.cpu())

# run the model once
result = squeezenet(fake_image)

# now you can export it; you can use a path if desired ‘models/squeezenet’
squeezenet.export(‘squeezenet')
```

The following table lists the AWS DeepLens supported models from the Gluon model zoo\.


| Supported Gluon Models |  Model | Description | 
| --- | --- | --- | 
|   AlexNet  |  Image classification model trained on the ImageNet dataset from ONNX  | 
|   MobileNet  |  Image classification model trained in TensorFlow using RMSprop  | 
|   ResNet  |  Image classification model trained on the ImageNet dataset from MXNet  | 
|   SqueezeNet  |  Image classification model trained on the ImageNet dataset from ONNX  | 
|   VGG  |  Image classification model trained on ImageNet dataset imported from MXNet or ONNX  | 

\* Currently, AWS DeepLens doesn't support the Gluon DenseNet or Inception models\.

For a complete list of models and more information, see the following:

+ [Gluon](https://mxnet.incubator.apache.org/api/python/gluon/gluon.html)

+ [Gluon Model Zoo](https://mxnet.incubator.apache.org/api/python/gluon/model_zoo.html)