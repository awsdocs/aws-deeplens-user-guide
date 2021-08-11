# AWS DeepLens Identity\-Based Policy Examples<a name="security_iam_id-based-policy-examples"></a>

By default, IAM users and roles don't have permission to create or modify AWS DeepLens resources\. They also can't perform tasks using the AWS Management Console, AWS CLI, or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\.

To learn how to create an IAM identity\-based policy using these example JSON policy documents, see [Creating Policies on the JSON Tab](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy Best Practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the AWS DeepLens Console](#security_iam_id-based-policy-examples-console)
+ [Allow Users to View Their Own Permissions](#security_iam_id-based-policy-examples-view-own-permissions)

## Policy Best Practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies are very powerful\. They determine whether someone can create, access, or delete AWS DeepLens resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started using AWS managed policies** – To start using AWS DeepLens quickly, use AWS managed policies to give your employees the permissions they need\. These policies are already available in your account and are maintained and updated by AWS\. For more information, see [Get started using permissions with AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-use-aws-defined-policies) in the *IAM User Guide*\.
+ **Grant least privilege** – When you create custom policies, grant only the permissions required to perform a task\. Start with a minimum set of permissions and grant additional permissions as necessary\. Doing so is more secure than starting with permissions that are too lenient and then trying to tighten them later\. For more information, see [Grant least privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) in the *IAM User Guide*\.
+ **Enable MFA for sensitive operations** – For extra security, require IAM users to use multi\-factor authentication \(MFA\) to access sensitive resources or API operations\. For more information, see [Using multi\-factor authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.
+ **Use policy conditions for extra security** – To the extent that it's practical, define the conditions under which your identity\-based policies allow access to a resource\. For example, you can write conditions to specify a range of allowable IP addresses that a request must come from\. You can also write conditions to allow requests only within a specified date or time range, or to require the use of SSL or MFA\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

## Using the AWS DeepLens Console<a name="security_iam_id-based-policy-examples-console"></a>

To access the AWS DeepLens console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the AWS DeepLens resources in your AWS account\. If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(IAM users or roles\) with that policy\.

To ensure that those entities can still use the AWS DeepLens console, also attach the following AWS managed policy to the entities\. For more information, see [Adding Permissions to a User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DeepLensIoTThingAccess",
            "Effect": "Allow",
            "Action": [
                "iot:CreateThing",
                "iot:DeleteThing",
                "iot:DeleteThingShadow",
                "iot:DescribeThing",
                "iot:GetThingShadow",
                "iot:UpdateThing",
                "iot:UpdateThingShadow"
            ],
            "Resource": [
                "arn:aws:iot:*:*:thing/deeplens_*"
            ]
        },
        {
            "Sid": "DeepLensIoTCertificateAccess",
            "Effect": "Allow",
            "Action": [
                "iot:AttachThingPrincipal",
                "iot:DetachThingPrincipal"
            ],
            "Resource": [
                "arn:aws:iot:*:*:thing/deeplens_*",
                "arn:aws:iot:*:*:cert/*"
            ]
        },
        {
            "Sid": "DeepLensIoTCreateCertificateAndPolicyAccess",
            "Effect": "Allow",
            "Action": [
                "iot:CreateKeysAndCertificate",
                "iot:CreatePolicy",
                "iot:CreatePolicyVersion"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Sid": "DeepLensIoTAttachCertificatePolicyAccess",
            "Effect": "Allow",
            "Action": [
                "iot:AttachPrincipalPolicy"
            ],
            "Resource": [
                "arn:aws:iot:*:*:policy/deeplens*",
                "arn:aws:iot:*:*:cert/*"
           ]
        },
        {
            "Sid": "DeepLensIoTDataAccess",
            "Effect": "Allow",
            "Action": [
                "iot:GetThingShadow",
                "iot:UpdateThingShadow"
            ],
            "Resource": [
                "arn:aws:iot:*:*:thing/deeplens_*"
            ]
        },
        {
            "Sid": "DeepLensIoTEndpointAccess",
            "Effect": "Allow",
            "Action": [
                "iot:DescribeEndpoint"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Sid": "DeepLensAccess",
            "Effect": "Allow",
            "Action": [
                "deeplens:*"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Sid": "DeepLensS3ObjectAccess",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::deeplens*/*"
            ]
        },
        {
            "Sid": "DeepLensS3Buckets",
            "Effect": "Allow",
            "Action": [
                "s3:DeleteBucket",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::deeplens*"
            ]
        },
        {
            "Sid": "DeepLensCreateS3Buckets",
            "Effect": "Allow",
            "Action": [
                "s3:CreateBucket"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Sid": "DeepLensIAMPassRoleAccess",
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "*"
            ],
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "greengrass.amazonaws.com",
                        "sagemaker.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Sid": "DeepLensIAMLambdaPassRoleAccess",
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": [
                "arn:aws:iam::*:role/deeplens*"
            ],
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "lambda.amazonaws.com"
                }
            }
        },
        {
            "Sid": "DeepLensGreenGrassAccess",
            "Effect": "Allow",
            "Action": [
                "greengrass:AssociateRoleToGroup",
                "greengrass:AssociateServiceRoleToAccount",
                "greengrass:CreateResourceDefinition",
                "greengrass:CreateResourceDefinitionVersion",
                "greengrass:CreateCoreDefinition",
                "greengrass:CreateCoreDefinitionVersion",
                "greengrass:CreateDeployment",
                "greengrass:CreateFunctionDefinition",
                "greengrass:CreateFunctionDefinitionVersion",
                "greengrass:CreateGroup",
                "greengrass:CreateGroupCertificateAuthority",
                "greengrass:CreateGroupVersion",
                "greengrass:CreateLoggerDefinition",
                "greengrass:CreateLoggerDefinitionVersion",
                "greengrass:CreateSubscriptionDefinition",
                "greengrass:CreateSubscriptionDefinitionVersion",
                "greengrass:DeleteCoreDefinition",
                "greengrass:DeleteFunctionDefinition",
                "greengrass:DeleteGroup",
                "greengrass:DeleteLoggerDefinition",
                "greengrass:DeleteSubscriptionDefinition",
                "greengrass:DisassociateRoleFromGroup",
                "greengrass:DisassociateServiceRoleFromAccount",
                "greengrass:GetAssociatedRole",
                "greengrass:GetConnectivityInfo",
                "greengrass:GetCoreDefinition",
                "greengrass:GetCoreDefinitionVersion",
                "greengrass:GetDeploymentStatus",
                "greengrass:GetDeviceDefinition",
                "greengrass:GetDeviceDefinitionVersion",
                "greengrass:GetFunctionDefinition",
                "greengrass:GetFunctionDefinitionVersion",
                "greengrass:GetGroup",
                "greengrass:GetGroupCertificateAuthority",
                "greengrass:GetGroupCertificateConfiguration",
                "greengrass:GetGroupVersion",
                "greengrass:GetLoggerDefinition",
                "greengrass:GetLoggerDefinitionVersion",
                "greengrass:GetServiceRoleForAccount",
                "greengrass:GetSubscriptionDefinition",
                "greengrass:GetSubscriptionDefinitionVersion",
                "greengrass:ListCoreDefinitionVersions",
                "greengrass:ListCoreDefinitions",
                "greengrass:ListDeployments",
                "greengrass:ListDeviceDefinitionVersions",
                "greengrass:ListDeviceDefinitions",
                "greengrass:ListFunctionDefinitionVersions",
                "greengrass:ListFunctionDefinitions",
                "greengrass:ListGroupCertificateAuthorities",
                "greengrass:ListGroupVersions",
                "greengrass:ListGroups",
                "greengrass:ListLoggerDefinitionVersions",
                "greengrass:ListLoggerDefinitions",
                "greengrass:ListSubscriptionDefinitionVersions",
                "greengrass:ListSubscriptionDefinitions",
                "greengrass:ResetDeployments",
                "greengrass:UpdateConnectivityInfo",
                "greengrass:UpdateCoreDefinition",
                "greengrass:UpdateDeviceDefinition",
                "greengrass:UpdateFunctionDefinition",
                "greengrass:UpdateGroup",
                "greengrass:UpdateGroupCertificateConfiguration",
                "greengrass:UpdateLoggerDefinition",
                "greengrass:UpdateSubscriptionDefinition"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Sid": "DeepLensLambdaAdminFunctionAccess",
            "Effect": "Allow",
            "Action": [
                "lambda:CreateFunction",
                "lambda:DeleteFunction",
                "lambda:GetFunction",
                "lambda:GetFunctionConfiguration",
                "lambda:ListFunctions",
                "lambda:ListVersionsByFunction",
                "lambda:PublishVersion",
                "lambda:UpdateFunctionCode",
                "lambda:UpdateFunctionConfiguration"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:function:deeplens*"
            ]
        },
        {
            "Sid": "DeepLensSageMakerAccess",
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:DescribeTrainingJob",
                "sagemaker:StopTrainingJob"
            ],
            "Resource": [
                "arn:aws:sagemaker:*:*:training-job/deeplens_*"
            ]
        },
        {
            "Sid": "DeepLensAcuityStreamAccess",
            "Effect": "Allow",
            "Action": [
                "acuity:CreateStream",
                "acuity:DescribeStream",
                "acuity:DeleteStream"
            ],
            "Resource": [
                "arn:aws:acuity:*:*:stream/deeplens*/*"
            ]
        },
        {
            "Sid": "DeepLensAcuityEndpointAccess",
            "Effect": "Allow",
            "Action": [
                "acuity:GetDataEndpoint"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

More granular access is not encouraged\. The device is typically accessed through the console and not through API calls\. There are no public APIs for AWS DeepLens\. 

## Allow Users to View Their Own Permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```