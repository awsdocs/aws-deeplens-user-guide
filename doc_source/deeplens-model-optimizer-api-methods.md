# Method mo\.optimize\(model\_name, input\_width, input\_height, \[platform\], \[aux\_inputs\]\)<a name="deeplens-model-optimizer-api-methods"></a>

Optimizes the AWS DeepLens model specified by `model_name` so that it runs efficiently on the AWS DeepLens GPU, instead of the CPU\.

**Request Syntax**

```
import mo
mo.optimize(model_name, input_width, input_height, platform, aux_inputs)
```

**Parameters**

+ `model_name`—Required\. The name of the model to optimize\. Type: string\.

+ `input_width`—Required\. The width of the input image in pixels\. The value must be a non\-negative integer less than or equal to 1024\. Type: integer\.

+ `input_height`—Required\. The height of the input image in pixels\. The value must be a non\-negative integer less than or equal to 1024\. Type: integer\.

+ `platform`—Optional\. The platform for this optimization\. The default value is `mxNet`, which is currently the only supported platform\. Type: string\.

+ `aux_inputs`—Optional\. A Python dictionary that contains auxiliary inputs that are not common to all platforms\. Type: Dict\.  
**mxNet Platform aux\_inputs**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/deeplens/latest/dg/deeplens-model-optimizer-api-methods.html)

**Response**

+ `error`—The error code\. For error codes, their cause, and corrective actions, see the following table\. Type: integer\.

+ `model_path`—For convenience, this method returns the path and the artifact name so that you don't need to know the name or the path of the artifact to load the model\. To load the model, call the model optimizer API and specify the `model_path`\. Type: string\. 


**Error Codes**  

| Error Code | Cause | Action to take | 
| --- | --- | --- | 
|  0  |  Model optimizer ran successfully\.  |  No action needed\.  | 
|  1  |  The requested platform is not supported\. Currently, `mxNet` is the only supported platform\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/deeplens/latest/dg/deeplens-model-optimizer-api-methods.html)  | 
|  2  |  Model optimizer failed\.  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/deeplens/latest/dg/deeplens-model-optimizer-api-methods.html)  | 