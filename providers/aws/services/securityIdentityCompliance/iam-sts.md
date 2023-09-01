# IAM Security Token Service (STS)

- generate temporary security credentials for trusted IAM users/roles
- users need the assumeRole permission

## my thoughts

## links

- [landing page](https://docs.aws.amazon.com/STS/latest/APIReference/welcome.html)
- [saml: intro](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html)
- [saml: relying party and claims](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_relying-party.html)
- [saml: roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_saml.html)
- [saml: creating identity providers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html)
- [IdP: creating identity proviers](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create.html)
- [saml: federated access with ABAC](https://aws.amazon.com/blogs/aws/new-for-identity-federation-use-employee-attributes-for-access-control-in-aws/)
- [web identity federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_oidc.html)
- [Temporary security credentials in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_request.htmly)
- [web identity playground](https://aws.amazon.com/blogs/aws/the-aws-web-identity-federation-playground/)
- [assumeRoleWithSAML api](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRoleWithSAML.html)

## best practices

### anti patterns

## features

- provide access to AWS resources without having to define an AWS identity or embed long-term credentials with an application.
- manage users in an external system outside AWS and grant them access to perform AWS tasks and access your AWS resources
- cross-account delegation: provide users access to resources in other aws accounts

### pricing

## basics

- Temporary security credentials are required when assuming an IAM role,
- almost identically to the IAM access key/secret except:
  - can last from a few minutes to several hours
  - not stored with the user but are generated dynamically and provided to the user when requested
  - can request new credentials as long as the user requesting them still has permissions to do so
- global service, and all AWS STS API requests go to a single endpoint at `https://sts.amazonaws.com`
- general workflow
  - user or application requires temporary security credentials to access AWS resources
  - make the AssumeRole API request
  - response includes temporary credentials: an access key ID, a secret access key, and a security session token
  - Each time a role is assumed and a set of temporary security credentials is generated, an IAM role session is created.
- components
  - access key id: identifies the temporary credentials
  - secret: signs api calls
  - session/security token: passed to the destination service for it to valdate the request
- use cases
  - identity federation: manage your users in an external system outside AWS and grant them access to perform AWS tasks and access your AWS resources
  - cross account access: Using roles and cross-account access, you can define user identities in one account and use those identities to access AWS resources in other accounts that belong to your organization.
  - roles for ec2: provide temporary security credentials to your instances when you launch them.
    - available to all applications that run on the instance,

### AssumeRole

- AssumeRole API provides a user the ability to switch their IAM role such that they have different or increased access only provided by another role
  - This enables you to practice the principle of least privilege.
- can be used within/across accounts
- assumeRole request: check the docs for the parameters available when making the request, its highly configurable
  - durationSeconds: defaults to 1 hr
  - Policy: iam policy for use as a inline session policy
  - PolicyArns.member.N: ARNs of the IAM managed policies that you want to use as managed session policies.
    - policies must exist in the same account as the role
    - up to 10
  - Tags.member.N: the session tags that you want to pass with the role
  - MFA parameters
    - SerialNumber:
    - TokenCode:
- assumeRole response: contains the creds
  - assumedRoleUser: the ARN of the issued temp creds and the unique identifer of the role ID
  - credentials: contains the access key id, secret, and security/session token
  - packedPolicySize: if this value is greater than 100% the request is rejected

### AssumeRoleWithSAML

- enable
  - federated access to AWS accounts
  - a separate SAML 2.0-based IdP for each AWS account and use federated user attributes for access control
  - pass user attributes, such as cost center or job role, from your IdPs to AWS, and implement fine-grained access permissions based on these attributes.
- everything from `# AssumeRole` still applies here
- setup process
  - use IAM to create a SAML provider entity in your AWS account that represents your identity provider.
  - create an IAM role that specifies this SAML provider in its trust policy.
  - configure your SAML IdP to issue the claims that AWS requires
  - you/apps can now call AssumeRoleWithSAML
- saml request: check the docs for how to make the API call and which params are required/optional
- saml response:
  - assumedRoleUser
  - credentials: access key id, secret, and session token
  - audience
  - subject type
  - packed policy size
  - name qualifier

### AssumeRoleWithWebIdentity

- logical workflow (e.g. with Cognito as identity provider)
  - user starts the app on their mobile device, the app asks the user to sign in and authenticate with Amazon
  - The app uses Amazon Cognito API operations to exchange the Amazon ID token for an Amazon Cognito token.
    - Amazon Cognito makes the exchange and requests temporary security credentials from AWS STS with the AssumeRoleWithWebIdentity API, passing the Amazon Cognito token for the temporary credentials
    - The role associated with the temporary security credentials and its assigned policy determine what can be accessed.
- setup proces
  - you must have an identity token from a supported identity provider and create a role that the application can assume
  - the identity provider must be specified in the role's trust policy.
  - your application can now call AssumeRoleWithWebIdentity
    - does not require the use of AWS security credentials.
    - you can distribute an application that requests temporary security credentials without including long-term AWS credentials in the application
- web identity request: check the doc for the optional & required params
- web identity response: pretty much identitial to SAML respons

## considerations

## integrations
