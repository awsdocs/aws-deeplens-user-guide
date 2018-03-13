# model\.doInference\(video\_frame\)<a name="deeplens-device-library-awscam-model-doinference"></a>

Runs inference on a video frame \(image file\) by applying the loaded model\. The method returns the result of the inference\.

**Request Syntax**

```
import awscam
model = awscam.Model(model_topology_file, loading_config)
result = model.doInference(video_frame)
```

**Parameters**

+ `video_frame`—Required\. The model runs its inference on a video frame \(image file\) and returns the result of the model inference, which is a dictionary\.

**Return Type**

+ `dict list`

**Returns**  
Returns a `dict` with a single key\-value pair\. The key is the name of the model's output layer, which is defined by the model you use\. The value is a list of `dicts` in which each element is an object that the model identified and its associated probability\. The user who applies the model should know how to parse the result\.

**Example**  
Sample output:  

```
{
   'SoftMax_67': array(
       [  
          2.41881448e-08, 
          3.57339691e-09, 
          1.00263861e-07, 
          5.40415579e-09, 
          4.37702547e-04, 
          6.16787545e-08
       ], 
   dtype=float32)
}
```