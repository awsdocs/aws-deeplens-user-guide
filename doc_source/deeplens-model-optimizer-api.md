# AWS DeepLens Model Optimzer API<a name="deeplens-model-optimizer-api"></a>

Custom models run inefficiently on the CPU\. To run a custom model efficiently on the GPU, use the AWS DeepLens model optimizer API\. The model optimizer class has a single method, `mo.optimize`, which optimizes your model to Cl\-DNN format so it can run on the GPU\.

```
class mo
```

Represents an AWS DeepLens model optimizer\.


+ [Method mo\.optimize\(\)](deeplens-model-optimizer-api-methods.md)