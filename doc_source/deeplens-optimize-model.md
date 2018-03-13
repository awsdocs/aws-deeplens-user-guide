# Optimizing a Custom Model<a name="deeplens-optimize-model"></a>

To access the GPU for inference, AWS DeepLens uses the clDNN, Compute Library for Deep Neural Networks\. To run your own models on AWS DeepLens, you have to convert them into clDNN format\. The model optimizer converts the format with the following code:

```
error, model_path = mo.optimize(model_name,input_width,input_height)
```

You include the model optimizer code in an inference Lambda function, which is required to allow AWS DeepLens to access deployed models\.

For information on how to create an inference Lambda function that includes the model optimizer, see [Creating an AWS DeepLens Inference Lambda Function](deeplens-inference-lambda-create.md)\.

For more information about the model optimizer, see [AWS DeepLens Model Optimzer API](deeplens-model-optimizer-api.md)\.