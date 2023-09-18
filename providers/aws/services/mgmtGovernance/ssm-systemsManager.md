# systems manager fka SSM

- a unified interface for a set of cloudnative, fully managed services for resources on AWS and in multicloud and hybrid environments
  - automated configuration management
  - track and resolve opertional issues
- the following services are split into separate files
  - operations Management
    - [explorer](./ssm-explorer.md)
      - customizable dashboard for resources with an aggregated view of ops data
    - [opsCenter](./ssm-opsCenter.md)
      - centralized ops/IT dashboard & task management for cloud resources
    - [incident manager](./ssm-incidentManager.md)
  - application management
    - [application manager](./ssm-applicationManager.md)
    - [app config](./ssm-appConfig.md)
    - [parameter store](./ssm-parameterStore.md)
      - hierarchical store for configuration & secret data
  - change management
    - [automation](./ssm-automation.md)
      - simplifies any scriptable/automation task via workflows and playbooks
      - use custom/predefined playbooks to enable AWS resource management across multiple accounts and AWS Regions.
    - [change manager](./ssm-changeManager.md)
    - [maintenance windows](./ssm-maintenanceWindows.md)
      - define a time window / schedule for patch manager, run command and other disruptive tasks
  - node management
    - [fleet manager](./ssm-fleetManager.md)
    - [session manager](./ssm-sessionManager.md)
      - auditable user access to servers through a webshell/ssh/cli without requiring inbound network ports
    - [patch manager](./ssm-patchManager.md)
      - select & deploy OS and software packages across instances
  - other services
    - Connect with ITSM and ITOM Software: its some kind of JIRA integration

## links

- [accessing secrets manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-ps-secretsmanager.html)
- [security best practices](https://docs.aws.amazon.com/systems-manager/latest/userguide/security-best-practices.html)
- [state manager user guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state.html)
- [faqs](https://aws.amazon.com/systems-manager/faq/)
- [ssm-agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)
- [change calendar](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-change-calendar.html)

## best practices

- become one with the dashboard
- configure the SSM Agent to automatically send its log data to a log group in CloudWatch Logs for analysis
  - OR configure and use the CloudWatch Agent to collect metrics and logs from your instances instead of using Systems Manager Agent (SSM Agent) for these tasks.
  - the CloudWatch Agent gathers more metrics on EC2 instances and on-premises servers than are available using SSM Agent
- while SSM is split into distinct services, they all highly integrate with eachother

### anti patterns

## features

- automatically aggregates and displays operational data for each resource group through a dashboard
- eliminates the need for you to navigate across multiple AWS consoles to view your operational data
- supports resources in multicloud and hybrid workloads

### pricing

- theres a free tier, beyond that it microtransacts you to death
- use the pricing calculator

## basics

- maintain security and compliance by scanning your managed instances and reporting on (or taking corrective action on) any policy violations it detects

### SSM Agent

- all other services use the SSM Agent for processing & executing tasks
- install and configur on servers
  - makes it possible for Systems Manager to update, manage, and configure the server.
  - sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service (service prefix: ec2messages).
- managed instances: any server with the SSM installed, configured, and associated with an instance profile
  - AMIs with SSM Agent preinstalled: Windows, Amazon Linux, and Ubuntu Server

#### Documents

- an operational playbook containing a series of steps to be executed in sequence
- where you define and configure tasks to be executed by the SSM agent
- versioned and shared across accounts

### Resource Groups

- a way to logically group resources in a specific account across regions

#### Tag Editor

- how resources are grouped

### console

- view operational data from multiple services and automate operational tasks across resources.

#### State Manager

- secure and scalable configuration management service
- e.g. bootstrapping instances, download/update agents on a schedule, network configuration, patching, run scripts, etc

#### Run Command

- way of executing tasks without connecting remotely to instances
- replacing the need for bastion hosts, SSH, or remote PowerShell.
- e.g. software updates, systems monitoring, or any OS/software configuration changes

#### Distributor

- centrally store and systematically distribute software packages while you maintain control over versioning
- create and distribute software packages and then install them using Systems Manager Run Command and State Manager
- use IAM policies to control who can create or update packages in your account

#### Change Calendar

- setup date and time ranges when SSM actions may/not be executed

#### Dashboards

##### Compliance

- automatically aggregates and displays operational data for each resource group through a dashboard

##### Inventory

- query & collect information about system configurations and installed applications
- applications, files, network configurations, Windows services, registries, server roles, updates, and any other system properties
- manage application assets, track licenses, monitor file integrity, discover applications not installed by a traditional installer, and more.

## Security

### networking

- resources need outbound access to the systems manager service
  - for private resources without an IGW, VPC endpoints can be used instead
- inbound access is not required

### authnz

- users and groups need IAM authnz to use and manage SSM
- SSM also needs to be installed on servers that are associated with an instance profile so that SSM can managed the server on your behalf

## considerations

## integrations

- it integrates with all the foundational ops services
  - cloudtrail, cloudwatch, config, trusted advisor, personal health dashboard, and EC2 instances

### VPC

### IAM

### EC2

- see your ec2 instances, any on-premises servers, or virtual machines in your hybrid environment, communicating with ec2messages. endpoints.
- information about executions, commands, scheduled actions, errors, and health statuses to log files on each instance.
- Logs files by manually connecting to an instance or automatically send logs to CloudWatch Logs.

### VPC

- vpc endpoints for connecting to the SSM service on servers without an IGW
  - use with privatelink to grant instances access to non AWS endpoints, e.g. for retrieving patches

### Cloudtrail

- audit everything SSM does

### CloudWatch

- events for tracking state and changes

### CloudFormation
