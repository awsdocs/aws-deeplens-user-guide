# AWS DeepLens Developer Guide

-----
*****Copyright &copy; 2018 Amazon Web Services, Inc. and/or its affiliates. All rights reserved.*****

-----
Amazon's trademarks and trade dress may not be used in 
     connection with any product or service that is not Amazon's, 
     in any manner that is likely to cause confusion among customers, 
     or in any manner that disparages or discredits Amazon. All other 
     trademarks not owned by Amazon are the property of their respective
     owners, who may or may not be affiliated with, connected to, or 
     sponsored by Amazon.

-----
## Contents
+ [What Is AWS DeepLens?](what-is-deeplens.md)
+ [Getting Started with AWS DeepLens](deeplens-getting-started.md)
   + [Prerequisites](deeplens-prerequisites.md)
   + [Register Your AWS DeepLens Device](deeplens-getting-started-register.md)
   + [Connect AWS DeepLens to the Network](deeplens-getting-started-connect.md)
   + [Set Up Your AWS DeepLens Device](deeplens-getting-started-set-up.md)
   + [Verify That Your AWS DeepLens Is Connected](deeplens-getting-started-verify-connection.md)
+ [Working with AWS DeepLens Projects](deeplens-projects.md)
   + [Machine Learning Frameworks Supported by AWS DeepLens](deeplens-supported-frameworks.md)
   + [Deploying an AWS DeepLens Project](deeplens-deploy-project.md)
   + [Viewing AWS DeepLens Project Output](deeplens-viewing-output.md)
   + [Working with AWS DeepLens Sample Projects](deeplens-sample-projects.md)
      + [AWS DeepLens Sample Projects Overview](deeplens-templated-projects-overview.md)
      + [Creating and Deploying an AWS DeepLens Sample Project](deeplens-create-deploy-sample-project.md)
      + [Extending any Project's Functionality](deeplens-extend.md)
      + [Editing an Existing Model with Amazon SageMaker](deeplens-train-model.md)
   + [Working with AWS DeepLens Custom Projects](deeplens-custom-projects.md)
      + [Importing Your Amazon SageMaker Trained Model](deeplens-import-from-sagemaker.md)
      + [Importing an Externally Trained Model](deeplens-import-external-trained.md)
      + [Optimizing a Custom Model](deeplens-optimize-model.md)
      + [Creating an AWS DeepLens Inference Lambda Function](deeplens-inference-lambda-create.md)
+ [Managing Your AWS DeepLens Device](deeplens-managing-your-device.md)
   + [Updating Your AWS DeepLens Device](deeplens-manual-updates.md)
   + [Deregister an AWS DeepLens Device](deeplens-deregister-device.md)
+ [AWS DeepLens Model Optimzer API](deeplens-model-optimizer-api.md)
   + [Method mo.optimize(model_name, input_width, input_height, [platform], [aux_inputs])](deeplens-model-optimizer-api-methods.md)
+ [AWS DeepLens Device Library](deeplens-device-library.md)
   + [Model](deeplens-device-library-awscam-model.md)
      + [Constructor](deeplens-device-library-awscam-model-constructor.md)
      + [model.doInference(video_frame)](deeplens-device-library-awscam-model-doinference.md)
      + [model.parseResult(model_type, raw_infer_result)](deeplens-device-library-awscam-model-parseresult.md)
   + [Method awscam.getLastFrame()](deeplens-device-library-awscam-model-get-last-frame.md)
+ [Document History for AWS DeepLens](doc-history.md)
+ [AWS Glossary](glossary.md)