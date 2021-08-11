# How AWS DeepLens Works with IAM<a name="security_iam_service-with-iam"></a>

Before you use IAM to manage access to AWS DeepLens, you should understand what IAM features are available to use with AWS DeepLens\. To get a high\-level view of how AWS DeepLens and other AWS services work with IAM, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [AWS DeepLens Identity\-Based Policies](#security_iam_service-with-iam-id-based-policies)
+ [AWS DeepLens Resource\-Based Policies](#security_iam_service-with-iam-resource-based-policies)
+ [Access Control Lists \(ACLs\)](#security_iam_service-with-iam-acls)
+ [Authorization Based on AWS DeepLens Tags](#security_iam_service-with-iam-tags)
+ [AWS DeepLens IAM Roles](#security_iam_service-with-iam-roles)

## AWS DeepLens Identity\-Based Policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources as well as the conditions under which actions are allowed or denied\. AWS DeepLens supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

Policy actions in AWS DeepLens use the following prefix before the action: `aws-deeplense:`\. For example, to grant someone permission to run an Amazon EC2 instance with the Amazon EC2 `RunInstances` API operation, you include the `ec2:RunInstances` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. AWS DeepLens defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "ec2:action1",
      "ec2:action2"
```

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "ec2:Describe*"
```



To see a list of AWS DeepLens actions, see [Actions Defined by AWS DeepLens](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_awskeymanagementservice.html#awskeymanagementservice-actions-as-permissions) in the *IAM User Guide*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

AWS DeepLens does not support specifying resource ARNs in a policy\.

### Condition Keys<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

AWS DeepLens does not provide any service\-specific condition keys\. 

## AWS DeepLens Resource\-Based Policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

AWS DeepLens does not support resource\-based policies\.

## Access Control Lists \(ACLs\)<a name="security_iam_service-with-iam-acls"></a>

 AWS DeepLens does not support access control lists \(ACLs\)\. 

## Authorization Based on AWS DeepLens Tags<a name="security_iam_service-with-iam-tags"></a>

 AWS DeepLens does not support tagging resources or controlling access based on tags\. 

## AWS DeepLens IAM Roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using Temporary Credentials with AWS DeepLens<a name="security_iam_service-with-iam-roles-tempcreds"></a>

AWS DeepLens supports using temporary credentials\. 

### Service\-Linked Roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles appear in your IAM account and are owned by the service\. An IAM administrator can view but not edit the permissions for service\-linked roles\.

AWS DeepLens does not support service\-linked roles\.

### Service Roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles appear in your IAM account and are owned by the account\. This means that an IAM administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

AWS DeepLens supports service roles\. 

### Choosing an IAM Role in AWS DeepLens<a name="security_iam_service-with-iam-roles-choose"></a>

When you create a *widget* resource in AWS DeepLens, you must choose a role to allow AWS DeepLens to access Amazon EC2 on your behalf\. If you have previously created a service role or service\-linked role, then AWS DeepLens provides you with a list of roles to choose from\. It's important to choose a role that allows access to start and stop Amazon EC2 instances\. For more information, see ***ADD XREF TO YOUR DOCS HERE***\.