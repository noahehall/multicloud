# Cloudformation template ref

- pretty sure this is going to have a bunch of overlap with the SAM doc
  - its in compute/SAM.md

## long example

```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: Some Template
Parameters: # inputs required when provisioning a stack
  BoomBringer:
    Type: String
    Default: ShakaLaka
    Description: Who brings the boom
Resources:
  MyQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Ref BoomBringer
  MyIAMRole:
    Type: AWS::IAM::Role
    Properties:
      - abcd
  MyEC2:
    Type: 'AWS::EC2::Instance' # all service namespaces follow this pattern
    Properties: # you'll mainly be in here
      ImageId: ami-abcdefg
      InstanceType: t2.micro
      ...
  MyEIP:
    Type: 'AWS::EC2::EIP'
    Properties:
      InstanceId: !Ref MyEC2 # link resources with Ref
Outputs:
  - abcd
```
