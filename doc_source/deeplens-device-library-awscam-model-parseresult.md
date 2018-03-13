# model\.parseResult\(model\_type, raw\_infer\_result\)<a name="deeplens-device-library-awscam-model-parseresult"></a>

Parses the results of some commonly used models, such as classification, SSD, and segmentation models\. For customized models, you need to write your own parse functions\.

**Request Syntax**

```
import awscam
model = awscam.Model(model_topology_file, loading_config)
raw_infer_result = model.doInference(video_frame)
result = model.parseResult(model_type, raw_infer_result)
```

**Parameters**

+ `model_type`—String that identifies the model type to use to generate the inference\. Required\. 

  Valid values: `classification`, `ssd`, and `segmentation`

+ `raw_infer_result`—The output of the function `model.doInference(video_frame)`\. Required\.

**Return Type**

+ `dict`

**Returns**  
Returns a `dict` with a single key\-value pair\. The key is the model type\. The value is a list of `dicts`, in which each element is an object label and probability calculated by the model\.

**Example**  
The output of a classification model might look like the following:  

```
{"output":[
    {"label":"318","prob":0.5},
    {"label":"277","prob":0.3",
    {"label":"433","prob":0.001",
    ]
}
```
The output of an SDD model contains bounding box information, similar to the following:  

```
{"output": [
    {"label": "318", "xmin": 124, "xmax": 245, "ymin": 10, "ymax": 142", "prob": 0.5},
    {"label": "277", "xmin": 89, "xmax": 166, "ymin": 233, "ymax": 376", "prob": 0.3},
    ...     …
    {"label": "433", "xmin": 355, "xmax": 468, "ymin": 210, "ymax": 266", "prob": 0.001}
    ]
}
```