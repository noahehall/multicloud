# elastic beanstalk

- platform for deploying and managing applications developed with common web languages

## my thoughts

- heroku for aws

## links

- [landing page](https://aws.amazon.com/elasticbeanstalk/)
- [advanced environment and configuration](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions.html)
- [supported platforms](https://docs.aws.amazon.com/elasticbeanstalk/latest/platforms/platforms-supported.html)
- [faqs](https://aws.amazon.com/elasticbeanstalk/faqs/?da=sec&sec=prep)

## best practices

### anti patterns

## features

- Migrate stateful applications off legacy infrastructure to Elastic Beanstalk and connect securely to your private network.
- Deploy scalable web applications in minutes
- 0 infrastructure management but you can still pick an instance type, choose db, autoscaling, etc
- supports bunchets of stuff: most importantly packer and multicontainer docker

### pricing

- pay for AWS resources (e.g. EC2 instances or S3 buckets) you create to store and run your application.
- the principal costs for a web application will typically be for the Amazon EC2 instance(s) and for the Elastic Load Balancing that distributes traffic between the instances running your application.

## terms

## basics

- basic workflow
  - create an application
  - upload version
  - launch environment
  - manage environment
    - repeat from upload version to redeploy

## considerations

## integrations
