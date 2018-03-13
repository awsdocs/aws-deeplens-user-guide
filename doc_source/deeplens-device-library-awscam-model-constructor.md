# Constructor<a name="deeplens-device-library-awscam-model-constructor"></a>

The constructor for a `awscam.Model`

**Request Syntax**

```
import awscam
model = awscam.Model(model_topology_file, loading_config)
```

**Parameters**

+ `model_topology_file`—Required\. A neural network topology file \(\.xml\) from the [Intel model optimizer](https://software.intel.com/en-us/model-optimizer-devguide)\.

  Models supported: Classification and Single Shot MultiBox Detector \(SSD\)

+ `loading_config` \(dict\)—Required\. Specifies whether the model should be loaded into the GPU or CPU\. The format of this parameter is a dictionary\.

**Permitted values:**

  + `{"GPU":1}`—Loads the model into the GPU

  + `{"GPU":0}`—Loads the model into the CPU