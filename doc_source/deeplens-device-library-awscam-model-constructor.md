# Model Class Constructor<a name="deeplens-device-library-awscam-model-constructor"></a>

Creates an `awscam.Model` object to run inference and to parse the result\.

**Request Syntax**

```
import awscam
model = awscam.Model(model_topology_file, loading_config)
```

**Parameters**
+ `model_topology_file`—Required\. A path to an optimized model \(\.xml\) file output by the [`mo.optimize`](#deeplens-device-library-awscam-model-constructor) method\. 

  Models supported: Classification, Segmentation and Single Shot MultiBox Detector \(SSD\)
+ `loading_config` \(dict\)—Required\. Specifies whether the model should be loaded into the GPU or CPU\. The format of this parameter is a dictionary\.

**Permitted values:**
  + `{"GPU":1}`—Loads the model into the GPU
  + `{"GPU":0}`—Loads the model into the CPU