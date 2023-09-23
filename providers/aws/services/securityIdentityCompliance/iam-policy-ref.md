# Policy ref

## links

- [AAA: policy generator](https://awspolicygen.s3.amazonaws.com/policygen.html)
- [AAA: policy reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)
- [blog: writing least privilege IAM policies](https://aws.amazon.com/blogs/security/techniques-for-writing-least-privilege-iam-policies/)
- [condition: global keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html)
- [condition: operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html)

## basics

- check the API Reference for a particular service to see available actions and such

## policy elements

- version: seems to always be `2012-10-17`

### Statement array

- the `NOT*` keys help you specify exceptions to policies
  - should only be used with Deny policies
    - i.e. DENY if NotPrincipal: superDuperRooter
  - you can still add an allow but in a separate statement
- Some services do not let you specify actions for individual resources
  - actions that you list in the Action or NotAction element apply to all resources in that service.
  - In these cases, you use the wildcar `*` in the Resource element.
- object definition
  - effect: allow/deny
  - action: the aws service and a potentially a filtered set of api calls that are allowed/denied
    - `serviceName:*` this specifies all actions (api calls) for this service
    - `serviceName:*blah` anything ending in blah
  - NotAction: matches everything except the specified list of actions.
    - can result in a shorter policy by listing only a few actions that should not match
    - use to provide scope for the policy
  - resource: ARN denoting resource(s) this policy covers
    - `resource: "*"` indicates all resources for this service
  - NotResource: matches every resource except those specified
    - can result in a shorter policy by listing only a few resources that should not match
    - never use with the "Effect": "Allow" and "Action": `*` elements together.
      - it would allow all actions in AWS on all resources except the resource specified in the policy.
  - Principal: an account, federated user, user, role, or service
  - NotPrincipal: applies to everyone except this person or account
    - also specify the account ARN of the not denied principal, else risk denying access to the entire account
    - help ensure that only necessary parties can access a resource.
    - can only be used with trust policies for IAM roles and in resource-based policies
    - do not use NotPrincipal in the same policy statement as "Effect": "Allow".
      - Doing so allows all principals except the one named in the NotPrincipal element.
  - conditions: conditions that control when a policy is in effect
    - compare keys in the request context to the key-values in the policy
    - service specific: prefixed with the service id, e.g. `ec2:InstanceType`
    - global: prefixed with `aws:`
    - condition keys:
      - service-specific: contain the services ID in the key, e.g. ec2:InstanceType
      - global
  - sid: unique description of the permission

## examples

- generaly any string value can be an array value if the key logically should accept multiple values

```jsonc
{
  "Version": "2012-10-17",
  "Id": "some-id?",
  "Statement": [
    {
      "Sid": "someUniqueDescription", // required in some services
      "Effect": "Allow",
      "Action": [
        "dynamodb:PutItem",
        "sts:AssumeRole" // required if service is a principal and a trust policy doesnt exist
      ],
      "Principal": {
        "Service": ["lambda.amazonaws.com"], // this service can assume this role
        "Federated": [
          "www.amazon.com", // web identities
          "arn:aws:iam::account-id:saml-provider/SomeProvider" // or SAML users
        ],
        "AWS": [
          // or any user in this specific account id
          "ACCOUNT-iD",
          // or this specific user in account-id
          // user name is case-sensitive; cannot use a wildcard (*) to mean "all users."
          "arn:aws:iam::account-id:user/MyName",
          // or this role
          "arn:aws:iam::account-id:role/RoleName"
        ]
      },
      "Resource": ["arn:aws:dynamodb:us-west-2:###:table/test"], // on these resources
      "Condition": {
        // if these conditions are met apply the Effect
        "someOperator": {
          "SomeKey": "Somevalue"
        }
      }
    }
  ]
}
```

## Quick Ref

- it would be futile to list everything here, check the docs or one of the sections below

```sh
## principals: generally its an ARN, can also use the following values
*  # for everyone, and can then be used with other policies
AWS # applies to all resources, and no other policies are taken into account
AWS: 12345 # specific account
AWS: arn:aws:iam:::/user/someName # a specific case-sensitive user in an account
AWS: arn:aws:iam::1234:role/roleName
AWS: arn:aws:iam:1234 # same as above, but uses an ARN
Federated: arn:aws:iam::12345:saml-provider/provider-name # federated saml providers
Federated: www.amazon.com # federated web identities
Service: [elasticmapreduce.amazonaws.com] # i.e. longServiceName.amazonaws.com

### Services: SERVICE_NAME.amazonaws.com
lambda, s3, cloudwatch, cloudformation, dynamodb

## resources: use `aws` for account_id for a resource created/managed by aws
### arn:aws:SERVICE_KEY:REGION:ACCOUNT_ID:RESOURCE_SCOPE

arn:aws:dynamodb:us-west-2::table/test
arn:aws:ec2:::instance/abcdefg
arn:aws:execute-api:us-east-1:*:account-id/stage/POST/mydemoresource/*
arn:aws:iam:::{policy,role}/abcd
arn:aws:logs

## Condition operators: append IfExists to any operator
Arn{Like,Equals}
Bool
Date{Greater,Less}Than
For{Any,All}Value:OTHER_OPERATOR
NumericLessThan
String{NotEquals,Equals,Like}

### global condition keys: not supported by all services
aws:{Epoch,TokenIssue}Time # date format: YYYY-MM-DDTHH:MM:SSz
aws:CurrentTime #  limit access to a certain period of time
aws:CalledVia # ordered list of services that made requests on behalf of a user
aws:CalledVia{First,Last}
aws:MultiFactorAuth{Present,Age}
aws:Principal{Account,OrgId,OrgPaths,Type,Tag}
aws:PrincipalArn # the ARN for the principal making the request.
aws:RequestedRegion
aws:Referer
aws:RequestTag/SomeTagName # restrict resource tags to this tag with the optional values
aws:ResourceTag
aws:SecureTransport # the request was made via SSL/TLS
aws:Source{Account,Arn,Vpc}
aws:SourceVpce # source VPC endpoint
aws:SourceIp # the IP address range used by the requester.
aws:TagKeys # checks the tags being used in the request
aws:UserAgent
aws:user{id,name}
aws:ViaAWSService #  control access during a service-to-service call.
aws:VpcSourceIp
IpAddress


#### variables, can be used to match against values in the request context
${aws:SomeKeyFromContext}
```

### IAM

```sh
# actions
iam:GetUser
iam:PassRole # this identity can pass role to a service

# conditions
iam:AWSServiceName # control access for a specific service role
iam:PolicyARN # control how users can apply AWS managed and customer managed policies
iam:AssociatedResourceArn # the ARN of the destination service resource that a role can be associated with
iam:PassedToService # the service to which a role can be passed
iam:PermissionsBoundary # the specified policy is attached as a permissions boundary on the IAM principal resource.
iam:ResourceTag/SomeTagName # checks the resource tag SomeTagName is attached to the resource
iam:OrganizationsPolicyId #  provides the IAM entity access to specific SCPs


```

### organizations

```sh

# actions
organizations:{describe*,list*}
```

### sts

```sh
# actions
sts:AssumeRole # allows the stated principals to assume the rule
sts:AssumeRolewithSAML
sts:AssumeRoleWithWebIdentity
sts:TagSession # able to add session tags

# conditions
## controls how IAM principals and applications name their role sessions when they assume an IAM role
sts:RoleSessionName
```

### KMS

```sh
# actions
kms:Decrypt
kms:DescribeKey
kms:GenerateDataKeyWithoutPlainText
kms:ReEncrypt
kms:CreateGrant # should be used with a conditional

# conditions
## for use with CreateGrant action: only approved if called on behalf of KMS
"Bool" > "kms:GrantIsForAWSResource" > true

```

### API Gateway

```sh

# actions
apigateway:*
execute-api:Invoke
```

### dynamodb

```sh

# actions
dynamodb:PutItem
```

### cloudwatch

```sh
# actions
logs:Create* # cloudwatch logs: any action starting with Create
```
