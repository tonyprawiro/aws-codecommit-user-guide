# Using Identity\-Based Policies \(IAM Policies\) for CodeCommit<a name="auth-and-access-control-iam-identity-based-access-control"></a>

The following examples of identity\-based policies demonstrate how an account administrator can attach permissions policies to IAM identities \(users, groups, and roles\) to grant permissions to perform operations on CodeCommit resources\.

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your CodeCommit resources\. For more information, see [Overview of Managing Access Permissions to Your CodeCommit Resources](auth-and-access-control-iam-access-control-identity-based.md)\.

**Topics**
+ [Permissions Required to Use the CodeCommit Console](#console-permissions)
+ [Viewing Resources in the Console](#console-resources)
+ [AWS Managed \(Predefined\) Policies for CodeCommit](#managed-policies)
+ [Customer Managed Policy Examples](#customer-managed-policies)

The following is an example of an identity\-based permissions policy: 

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codecommit:BatchGetRepositories"
      ],
      "Resource" : [
        "arn:aws:codecommit:us-east-2:111111111111:MyDestinationRepo",
        "arn:aws:codecommit:us-east-2:111111111111:MyDemo*"
      ]
    }
  ]
}
```

This policy has one statement that allows a user to get information about the CodeCommit repository named `MyDestinationRepo` and all CodeCommit repositories that start with the name `MyDemo` in the **us\-east\-2** Region\. 

## Permissions Required to Use the CodeCommit Console<a name="console-permissions"></a>

To see the required permissions for each CodeCommit API operation, and for more information about CodeCommit operations, see [CodeCommit Permissions Reference](auth-and-access-control-permissions-reference.md)\.

To allow users to use the CodeCommit console, the administrator must grant them permissions for CodeCommit actions\. For example, you could attach the AWSCodeCommitPowerUser managed policy or its equivalent to a user or group, as shown in the following permissions policy:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "codecommit:BatchGet*",
        "codecommit:Get*",
        "codecommit:List*",
        "codecommit:Create*",
        "codecommit:DeleteBranch",
        "codecommit:Describe*",
        "codecommit:Put*",
        "codecommit:Post*",
        "codecommit:Merge*",
        "codecommit:TagResource",
        "codecommit:Test*",
        "codecommit:UntagResource",
        "codecommit:Update*",
        "codecommit:GitPull",
        "codecommit:GitPush"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchEventsCodeCommitRulesAccess",
      "Effect": "Allow",
      "Action": [
        "events:DeleteRule",
        "events:DescribeRule",
        "events:DisableRule",
        "events:EnableRule",
        "events:PutRule",
        "events:PutTargets",
        "events:RemoveTargets",
        "events:ListTargetsByRule"
      ],
      "Resource": "arn:aws:events:*:*:rule/codecommit*"
    },
    {
      "Sid": "SNSTopicAndSubscriptionAccess",
      "Effect": "Allow",
      "Action": [
        "sns:Subscribe",
        "sns:Unsubscribe"
      ],
      "Resource": "arn:aws:sns:*:*:codecommit*"
    },
    {
      "Sid": "SNSTopicAndSubscriptionReadAccess",
      "Effect": "Allow",
      "Action": [
        "sns:ListTopics",
        "sns:ListSubscriptionsByTopic",
        "sns:GetTopicAttributes"
      ],
      "Resource": "*"
    },
    {
      "Sid": "LambdaReadOnlyListAccess",
      "Effect": "Allow",
      "Action": [
        "lambda:ListFunctions"
      ],
      "Resource": "*"
    },
    {
      "Sid": "IAMReadOnlyListAccess",
      "Effect": "Allow",
      "Action": [
        "iam:ListUsers"
      ],
      "Resource": "*"
    },
    {
      "Sid": "IAMReadOnlyConsoleAccess",
      "Effect": "Allow",
      "Action": [
        "iam:ListAccessKeys",
        "iam:ListSSHPublicKeys",
        "iam:ListServiceSpecificCredentials",
        "iam:ListAccessKeys",
        "iam:GetSSHPublicKey"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "IAMUserSSHKeys",
      "Effect": "Allow",
      "Action": [
        "iam:DeleteSSHPublicKey",
        "iam:GetSSHPublicKey",
        "iam:ListSSHPublicKeys",
        "iam:UpdateSSHPublicKey",
        "iam:UploadSSHPublicKey"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    },
    {
      "Sid": "IAMSelfManageServiceSpecificCredentials",
      "Effect": "Allow",
      "Action": [
        "iam:CreateServiceSpecificCredential",
        "iam:UpdateServiceSpecificCredential",
        "iam:DeleteServiceSpecificCredential",
        "iam:ResetServiceSpecificCredential"
      ],
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    }
  ]
}
```

In addition to permissions granted to users by identity\-based policies, CodeCommit requires permissions for AWS Key Management Service \(AWS KMS\) actions\. An IAM user does not need explicit `Allow` permissions for these actions, but the user must not have any policies attached that set the following permissions to `Deny`:

```
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt",
        "kms:GenerateDataKey",
        "kms:GenerateDataKeyWithoutPlaintext",
        "kms:DescribeKey"
```

For more information about encryption and CodeCommit, see [AWS KMS and Encryption](encryption.md)\.

## Viewing Resources in the Console<a name="console-resources"></a>

The CodeCommit console requires the `ListRepositories` permission to display a list of repositories for your AWS account in the AWS Region where you are signed in\. The console also includes a **Go to resource** function to quickly perform a case insensitive search for resources\. This search is performed in your AWS account in the AWS Region where you are signed in\. The following resources are displayed across the following services:
+ AWS CodeBuild: Build projects
+ AWS CodeCommit: Repositories
+ AWS CodeDeploy: Applications
+ AWS CodePipeline: Pipelines

To perform this search across resources in all services, you must have the following permissions:
+ CodeBuild: `ListProjects`
+ CodeCommit: `ListRepositories`
+ CodeDeploy: `ListApplications`
+ CodePipeline: `ListPipelines`

Results are not returned for a service's resources if you do not have permissions for that service\. Even if you have permissions for viewing resources, specific resources will not be returned if there is an explicit `Deny` to view those resources\.

## AWS Managed \(Predefined\) Policies for CodeCommit<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS managed policies grant required permissions for common use cases\. The managed policies for CodeCommit also provide permissions to perform operations in other services, such as IAM, Amazon SNS, and Amazon CloudWatch Events, as required for the responsibilities for the users who have been granted the policy in question\. For example, the AWSCodeCommitFullAccess policy is an administrative\-level user policy that allows users with this policy to create and manage CloudWatch Events rules for repositories \(rules whose names are prefixed with `codecommit`\) and Amazon SNS topics for notifications about repository\-related events \(topics whose names are prefixed with `codecommit`\), as well as administer repositories in CodeCommit\. 

The following AWS managed policies, which you can attach to users in your account, are specific to CodeCommit:
+ **AWSCodeCommitFullAccess** – Grants full access to CodeCommit\. Apply this policy only to administrative\-level users to whom you want to grant full control over CodeCommit repositories and related resources in your AWS account, including the ability to delete repositories\.

  The AWSCodeCommitFullAccess policy contains the following policy statement:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "codecommit:*"
        ],
        "Resource": "*"
      },
      {
        "Sid": "CloudWatchEventsCodeCommitRulesAccess",
        "Effect": "Allow",
        "Action": [
          "events:DeleteRule",
          "events:DescribeRule",
          "events:DisableRule",
          "events:EnableRule",
          "events:PutRule",
          "events:PutTargets",
          "events:RemoveTargets",
          "events:ListTargetsByRule"
        ],
        "Resource": "arn:aws:events:*:*:rule/codecommit*"
      },
      {
        "Sid": "SNSTopicAndSubscriptionAccess",
        "Effect": "Allow",
        "Action": [
          "sns:CreateTopic",
          "sns:DeleteTopic",
          "sns:Subscribe",
          "sns:Unsubscribe",
          "sns:SetTopicAttributes"
        ],
        "Resource": "arn:aws:sns:*:*:codecommit*"
      },
      {
        "Sid": "SNSTopicAndSubscriptionReadAccess",
        "Effect": "Allow",
        "Action": [
          "sns:ListTopics",
          "sns:ListSubscriptionsByTopic",
          "sns:GetTopicAttributes"
        ],
        "Resource": "*"
      },
      {
        "Sid": "LambdaReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "lambda:ListFunctions"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListUsers"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyConsoleAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListAccessKeys",
          "iam:ListSSHPublicKeys",
          "iam:ListServiceSpecificCredentials",
          "iam:ListAccessKeys",
          "iam:GetSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      },
      {
        "Sid": "IAMUserSSHKeys",
        "Effect": "Allow",
        "Action": [
          "iam:DeleteSSHPublicKey",
          "iam:GetSSHPublicKey",
          "iam:ListSSHPublicKeys",
          "iam:UpdateSSHPublicKey",
          "iam:UploadSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      },
      {
        "Sid": "IAMSelfManageServiceSpecificCredentials",
        "Effect": "Allow",
        "Action": [
          "iam:CreateServiceSpecificCredential",
          "iam:UpdateServiceSpecificCredential",
          "iam:DeleteServiceSpecificCredential",
          "iam:ResetServiceSpecificCredential"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      }
    ]
  }
  ```
+ **AWSCodeCommitPowerUser** – Allows users access to all of the functionality of CodeCommit and repository\-related resources, except it does not allow them to delete CodeCommit repositories or create or delete repository\-related resources in other AWS services, such as Amazon CloudWatch Events\. We recommend that you apply this policy to most users\.

  The AWSCodeCommitPowerUser policy contains the following policy statement:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "codecommit:BatchGet*",
          "codecommit:Get*",
          "codecommit:List*",
          "codecommit:Create*",
          "codecommit:DeleteBranch",
          "codecommit:Describe*",
          "codecommit:Put*",
          "codecommit:Post*",
          "codecommit:Merge*",
          "codecommit:TagResource",
          "codecommit:Test*",
          "codecommit:UntagResource",
          "codecommit:Update*",
          "codecommit:GitPull",
          "codecommit:GitPush"
        ],
        "Resource": "*"
      },
      {
        "Sid": "CloudWatchEventsCodeCommitRulesAccess",
        "Effect": "Allow",
        "Action": [
          "events:DeleteRule",
          "events:DescribeRule",
          "events:DisableRule",
          "events:EnableRule",
          "events:PutRule",
          "events:PutTargets",
          "events:RemoveTargets",
          "events:ListTargetsByRule"
        ],
        "Resource": "arn:aws:events:*:*:rule/codecommit*"
      },
      {
        "Sid": "SNSTopicAndSubscriptionAccess",
        "Effect": "Allow",
        "Action": [
          "sns:Subscribe",
          "sns:Unsubscribe"
        ],
        "Resource": "arn:aws:sns:*:*:codecommit*"
      },
      {
        "Sid": "SNSTopicAndSubscriptionReadAccess",
        "Effect": "Allow",
        "Action": [
          "sns:ListTopics",
          "sns:ListSubscriptionsByTopic",
          "sns:GetTopicAttributes"
        ],
        "Resource": "*"
      },
      {
        "Sid": "LambdaReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "lambda:ListFunctions"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListUsers"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyConsoleAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListAccessKeys",
          "iam:ListSSHPublicKeys",
          "iam:ListServiceSpecificCredentials",
          "iam:ListAccessKeys",
          "iam:GetSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      },
      {
        "Sid": "IAMUserSSHKeys",
        "Effect": "Allow",
        "Action": [
          "iam:DeleteSSHPublicKey",
          "iam:GetSSHPublicKey",
          "iam:ListSSHPublicKeys",
          "iam:UpdateSSHPublicKey",
          "iam:UploadSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      },
      {
        "Sid": "IAMSelfManageServiceSpecificCredentials",
        "Effect": "Allow",
        "Action": [
          "iam:CreateServiceSpecificCredential",
          "iam:UpdateServiceSpecificCredential",
          "iam:DeleteServiceSpecificCredential",
          "iam:ResetServiceSpecificCredential"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      }
    ]
  }
  ```
+ **AWSCodeCommitReadOnly** – Grants read\-only access to CodeCommit and repository\-related resources in other AWS services, as well as the ability to create and manage their own CodeCommit\-related resources \(such as Git credentials and SSH keys for their IAM user to use when accessing repositories\)\. Apply this policy to users to whom you want to grant the ability to read the contents of a repository, but not make any changes to its contents\.

  The AWSCodeCommitReadOnly policy contains the following policy statement:

  ```
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": [
          "codecommit:BatchGet*",
          "codecommit:Get*",
          "codecommit:Describe*",
          "codecommit:List*",
         "codecommit:GitPull"
        ],
        "Resource": "*"
      },
      {
        "Sid": "CloudWatchEventsCodeCommitRulesReadOnlyAccess",
        "Effect": "Allow",
        "Action": [
          "events:DescribeRule",
          "events:ListTargetsByRule"
        ],
        "Resource": "arn:aws:events:*:*:rule/codecommit*"
      },
      {
        "Sid": "SNSSubscriptionAccess",
        "Effect": "Allow",
        "Action": [
          "sns:ListTopics",
          "sns:ListSubscriptionsByTopic",
          "sns:GetTopicAttributes"
        ],
        "Resource": "*"
      },
      {
        "Sid": "LambdaReadOnlyListAccess",
        "Effect": "Allow",
        "Action": [
          "lambda:ListFunctions"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyListAccess",
        "Effect": "Allow",
       "Action": [
          "iam:ListUsers"
        ],
        "Resource": "*"
      },
      {
        "Sid": "IAMReadOnlyConsoleAccess",
        "Effect": "Allow",
        "Action": [
          "iam:ListAccessKeys",
          "iam:ListSSHPublicKeys",
          "iam:ListServiceSpecificCredentials",
          "iam:ListAccessKeys",
          "iam:GetSSHPublicKey"
        ],
        "Resource": "arn:aws:iam::*:user/${aws:username}"
      }
    ]
  }
  ```

For more information, see [AWS Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

## Customer Managed Policy Examples<a name="customer-managed-policies"></a>

You can create your own custom IAM policies to allow permissions for CodeCommit actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. You can also create your own custom IAM policies for integration between CodeCommit and other AWS services\.

**Topics**
+ [Customer Managed Identity Policy Examples](#customer-managed-policies-identity)
+ [Customer Managed Integration Policy Examples](#integration-policy-examples)

### Customer Managed Identity Policy Examples<a name="customer-managed-policies-identity"></a>

The following example IAM policies grant permissions for various CodeCommit actions\. Use them to limit CodeCommit access for your IAM users and roles\. These policies control the ability to perform actions with the CodeCommit console, API, AWS SDKs, or the AWS CLI\.

**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Allow a User to Perform CodeCommit Operations in a Single Region](#identity-based-policies-example-1)
+ [Example 2: Allow a User to Use Git for a Single Repository](#identity-based-policies-example-2)
+ [Example 3: Allow a User Connecting from a Specified IP Address Range Access to a Repository ](#identity-based-policies-example-3)
+ [Example 4: Deny or Allow Actions on Branches](#identity-based-policies-example-4)
+ [Example 5: Deny or Allow Actions on Repositories with Tags](#identity-based-policies-example-5)

#### Example 1: Allow a User to Perform CodeCommit Operations in a Single Region<a name="identity-based-policies-example-1"></a>

The following permissions policy uses a wildcard character \(`"codecommit:*"`\) to allow users to perform all CodeCommit actions in the us\-east\-2 Region and not from other AWS Regions\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "codecommit:*",
            "Resource": "arn:aws:codecommit:us-east-2:111111111111:*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": "us-east-2"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "codecommit:ListRepositories",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": "us-east-2"
                }
            }
        }
    ]
}
```

#### Example 2: Allow a User to Use Git for a Single Repository<a name="identity-based-policies-example-2"></a>

In CodeCommit, the `GitPull` IAM policy permissions apply to any Git client command where data is retrieved from CodeCommit, including git fetch, git clone, and so on\. Similarly, the `GitPush` IAM policy permissions apply to any Git client command where data is sent to CodeCommit\. For example, if the `GitPush` IAM policy permission is set to `Allow`, a user can push the deletion of a branch using the Git protocol\. That push is unaffected by any permissions applied to the `DeleteBranch` operation for that IAM user\. The `DeleteBranch` permission applies to actions performed with the console, the AWS CLI, the SDKs, and the API, but not the Git protocol\. 

The following example allows the specified user to pull from, and push to, the CodeCommit repository named `MyDemoRepo`:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "codecommit:GitPull",
        "codecommit:GitPush"
      ],
      "Resource" : "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo"
    }
  ]
}
```

#### Example 3: Allow a User Connecting from a Specified IP Address Range Access to a Repository<a name="identity-based-policies-example-3"></a>

You can create a policy that only allows users to connect to a CodeCommit repository if their IP address is within a certain IP address range\. There are two equally valid approaches to this\. You can create a `Deny` policy that disallows CodeCommit operations if the IP address for the user is not within a specific block, or you can create an `Allow` policy that allows CodeCommit operations if the IP address for the user is within a specific block\.

You can create a `Deny` policy that denies access to all users who are not within a certain IP range\. For example, you could attach the AWSCodeCommitPowerUser managed policy and a customer\-managed policy to all users who require access to your repository\. The following example policy denies all CodeCommit permissions to users whose IP addresses are not within the specified IP address block of 203\.0\.113\.0/16:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Deny",
         "Action": [
            "codecommit:*"
         ],
         "Resource": "*",
         "Condition": {
            "NotIpAddress": {
               "aws:SourceIp": [
                  "203.0.113.0/16"
               ]
            }
         }
      }
   ]
}
```

The following example policy allows the specified user to access a CodeCommit repository named MyDemoRepo with the equivalent permissions of the AWSCodeCommitPowerUser managed policy only if their IP address is within the specified address block of 203\.0\.113\.0/16:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "codecommit:BatchGetRepositories",
            "codecommit:CreateBranch",
            "codecommit:CreateRepository",
            "codecommit:Get*",
            "codecommit:GitPull",
            "codecommit:GitPush",
            "codecommit:List*",
            "codecommit:Put*",
            "codecommit:Post*",
            "codecommit:Merge*",
            "codecommit:TagResource",
            "codecommit:Test*",
            "codecommit:UntagResource",
            "codecommit:Update*"
         ],
         "Resource": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
         "Condition": {
            "IpAddress": {
               "aws:SourceIp": [
                  "203.0.113.0/16"
               ]
            }
         }
      }
   ]
}
```

#### Example 4: Deny or Allow Actions on Branches<a name="identity-based-policies-example-4"></a>

You can create a policy that denies users permissions to actions you specify on one or more branches\. Alternatively, you can create a policy that allows actions on one or more branches that they might not otherwise have in other branches of a repository\. You can use these policies with the appropriate managed \(predefined\) policies\. For more information, see [Limit Pushes and Merges to Branches in AWS CodeCommit](how-to-conditional-branch.md)\.

For example, you can create a `Deny` policy that denies users the ability to make changes to a branch named master, including deleting that branch, in a repository named *MyDemoRepo*\. You can use this policy with the **AWSCodeCommitPowerUser** managed policy\. Users with these two policies applied would be able to create and delete branches, create pull requests, and all other actions as allowed by **AWSCodeCommitPowerUser**, but they would not be able to push changes to the branch named *master*, add or edit a file in the *master* branch in the CodeCommit console, or merge branches or a pull request into the *master* branch\. Because `Deny` is applied to `GitPush`, you must include a `Null` statement in the policy, to allow initial `GitPush` calls to be analyzed for validity when users make pushes from their local repos\.

**Tip**  
If you want to create a policy that applies to all branches named *master* in all repositories in your AWS account, for `Resource`, specify an asterisk \( `*` \) instead of a repository ARN\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "codecommit:GitPush",
                "codecommit:DeleteBranch",
                "codecommit:PutFile",
                "codecommit:Merge*"
            ],
            "Resource": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
            "Condition": {
                "StringEqualsIfExists": {
                    "codecommit:References": [
                        "refs/heads/master"   
                    ]
                },
                "Null": {
                    "codecommit:References": false
                }
            }
        }
    ]
}
```

The following example policy allows a user to make changes to a branch named master in all repositories in an AWS account\. You might use this policy with the AWSCodeCommitReadOnly managed policy to allow automated pushes to the repository\. Because the Effect is `Allow`, this example policy would not work with managed policies such as AWSCodeCommitPowerUser\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "codecommit:GitPush",
                "codecommit:Merge*"
            ],
            "Resource": "*",
            "Condition": {
                "StringNotEqualsIfExists": {
                    "codecommit:References": [
                        "refs/heads/master"
                    ]
                }
            }
        }
    ]
}
```

#### Example 5: Deny or Allow Actions on Repositories with Tags<a name="identity-based-policies-example-5"></a>

You can create a policy that allows or denies actions on repositories based on the AWS tags associated with those repositories, and then apply those policies to the IAM groups you configure for managing IAM users\. For example, you can create a policy that denies all CodeCommit actions on any repositories with the AWS tag key *Status* and the key value of *Secret*, and then apply that policy to the IAM group you created for general developers \(*Developers*\)\. You then need to make sure that the developers working on those tagged repositories are not members of that general *Developers* group, but belong instead to a different IAM group that does not have the restrictive policy applied \(*SecretDevelopers*\)\.

The following example denies all CodeCommit actions on repositories tagged with the key *Status* and the key value of *Secret*:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Deny",
      "Action" : "codecommit:*"
      "Resource" : "*",
      "Condition" : {
         "StringEquals" : "ec2ResourceTag/Status": "Secret"
        }
    }
  ]
}
```

You can further refine this strategy by specifying specific repositories, rather than all repositories, as resouces\. You can also create policies that allow CodeCommit actions on all repositories that are not tagged with specific tags\. For example, the following policy allows the equivalent of AWSCodeCommitPowerUser permissions for all repositories except those tagged with the specified tags:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "codecommit:BatchGetRepositories",
            "codecommit:CreateBranch",
            "codecommit:CreateRepository",
            "codecommit:Get*",
            "codecommit:GitPull",
            "codecommit:GitPush",
            "codecommit:List*",
            "codecommit:Put*",
            "codecommit:TagResource",
            "codecommit:Test*",
            "codecommit:UntagResource",
            "codecommit:Update*"
         ],
         "Resource": "*",
         "Condition": {
            "StringNotEquals": {
               "ec2ResourceTag/Status": "Secret",
               "ec2ResourceTag/Team": "Saanvi"
            }
         }
      }
   ]
}
```

### Customer Managed Integration Policy Examples<a name="integration-policy-examples"></a>

This section provides example customer\-managed user policies that grant permissions for integrations between CodeCommit and other AWS services\. For specific examples of policies that allow cross\-account access to a CodeCommit repository, see [Configure Cross\-Account Access to an AWS CodeCommit Repository](cross-account.md)\.

**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) when a region is required, and contain fictitious account IDs\.

 **Examples**
+ [Example 1: Create a Policy That Enables Cross\-Account Access to an Amazon SNS Topic](#access-permissions-sns-int)
+ [Example 2: Create an Amazon Simple Notification Service \(Amazon SNS\) Topic Policy to Allow Amazon CloudWatch Events to Publish CodeCommit Events to the Topic ](#access-permissions-SNS-CWE)
+ [Example 3: Create a Policy for AWS Lambda Integration with a CodeCommit Trigger](#access-permissions-lambda-int)

#### Example 1: Create a Policy That Enables Cross\-Account Access to an Amazon SNS Topic<a name="access-permissions-sns-int"></a>

You can configure a CodeCommit repository so that code pushes or other events trigger actions, such as sending a notification from Amazon Simple Notification Service \(Amazon SNS\)\. If you create the Amazon SNS topic with the same account used to create the CodeCommit repository, you do not need to configure additional IAM policies or permissions\. You can create the topic, and then create the trigger for the repository\. For more information, see [Create a Trigger for an Amazon SNS Topic](how-to-notify-sns.md)\.

However, if you want to configure your trigger to use an Amazon SNS topic in another AWS account, you must first configure that topic with a policy that allows CodeCommit to publish to that topic\. From that other account, open the Amazon SNS console, choose the topic from the list, and for **Other topic actions**, choose **Edit topic policy**\. On the **Advanced** tab, modify the policy for the topic to allow CodeCommit to publish to that topic\. For example, if the policy is the default policy, you would modify the policy as follows, changing the items in *red italic text* to match the values for your repository, Amazon SNS topic, and account:

```
{
  "Version": "2008-10-17",
  "Id": "__default_policy_ID",
  "Statement": [
    {
      "Sid": "__default_statement_ID",
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": [
        "SNS:Subscribe",
        "SNS:ListSubscriptionsByTopic",
        "SNS:DeleteTopic",
        "SNS:GetTopicAttributes",
        "SNS:Publish",
        "SNS:RemovePermission",
        "SNS:AddPermission",
        "SNS:Receive",
        "SNS:SetTopicAttributes"
      ],
      "Resource": "arn:aws:sns:us-east-2:111111111111:NotMySNSTopic",
      "Condition": {
        "StringEquals": {
          "AWS:SourceOwner": "111111111111"
        }
      }
     },
     {
      "Sid": "CodeCommit-Policy_ID",
      "Effect": "Allow",
      "Principal": {
        "Service": "codecommit.amazonaws.com"
      },
      "Action": "SNS:Publish",
      "Resource": "arn:aws:sns:us-east-2:111111111111:NotMySNSTopic",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
          "AWS:SourceAccount": "111111111111"
        }
      }
    }
  ]
}
```

#### Example 2: Create an Amazon Simple Notification Service \(Amazon SNS\) Topic Policy to Allow Amazon CloudWatch Events to Publish CodeCommit Events to the Topic<a name="access-permissions-SNS-CWE"></a>

You can configure CloudWatch Events to publish to an Amazon SNS topic when events occur, including CodeCommit events\. To do so, you must make sure that CloudWatch Events has permission to publish events to your Amazon SNS topic by creating a policy for the topic or modifying an existing policy for the topic similar to the following:

```
{
  Version":"2012-10-17",
  "Id":"__default_policy_ID",
  "Statement":[
    {
      "Sid":"__default_statement_ID",
      "Effect":"Allow",
      "Principal":"{"AWS":"*"},
      "Action":{
        "SNS:Publish"
      ]
      "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic",
      "Condition":{
        "StringEquals":{"AWS:SourceOwner":123456789012"}
      }
    },                    
    {
      "Sid":"Allow_Publish_Events",
      "Effect":"Allow",
      "Principal":{"Service":"events.amazonaws.com"},
      "Action":"sns:Publish",
      "Resource":"arn:aws:sns:us-east-2:123456789012:MyTopic"
    }
  ]
}
```

For more information about CodeCommit and CloudWatch Events, see [CloudWatch Events Event Examples From Supported Services](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#codecommit_event_type)\.

#### Example 3: Create a Policy for AWS Lambda Integration with a CodeCommit Trigger<a name="access-permissions-lambda-int"></a>

You can configure a CodeCommit repository so that code pushes or other events trigger actions, such as invoking a function in AWS Lambda\. For more information, see [Create a Trigger for a Lambda Function](how-to-notify-lambda.md)\. This information is specific to triggers, and not CloudWatch Events\.

If you want your trigger to run a Lambda function directly \(instead of using an Amazon SNS topic to invoke the Lambda function\), and you do not configure the trigger in the Lambda console, you must include a policy similar to the following in the function's resource policy:

```
{
  "Statement":{
     "StatementId":"Id-1",
     "Action":"lambda:InvokeFunction",
     "Principal":"codecommit.amazonaws.com",
     "SourceArn":"arn:aws:codecommit:us-east-2:111111111111:MyDemoRepo",
     "SourceAccount":"111111111111"
  }
}
```

When manually configuring a CodeCommit trigger that invokes a Lambda function, you must also use the Lambda [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) command to grant permission for CodeCommit to invoke the function\. For an example, see the [To allow CodeCommit to run a Lambda function](how-to-notify-lambda-cc.md#how-to-notify-lambda-create-function-perm) section of [Create a Trigger for an Existing Lambda Function](how-to-notify-lambda-cc.md)\. 

For more information about resource policies for Lambda functions, see [AddPermission](https://docs.aws.amazon.com/lambda/latest/dg/API_AddPermission.html) and [The Pull/Push Event Models](https://docs.aws.amazon.com/lambda/latest/dg/intro-invocation-modes.html) in the *AWS Lambda Developer Guide*\.