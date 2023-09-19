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
- [change calendar](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-change-calendar.html)
- [faqs](https://aws.amazon.com/systems-manager/faq/)
- [security best practices](https://docs.aws.amazon.com/systems-manager/latest/userguide/security-best-practices.html)
- [ssm agent: hybrid activations](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html)
- [ssm agent: intro](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)
- [state manager user guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state.html)

## best practices

- become one with the dashboard
- configure the SSM Agent to automatically send its log data to a log group in CloudWatch Logs for analysis
  - OR configure and use the CloudWatch Agent to collect metrics and logs from your instances instead of using Systems Manager Agent (SSM Agent) for these tasks.
  - the CloudWatch Agent gathers more metrics on EC2 instances and on-premises servers than are available using SSM Agent
- while SSM is split into distinct services, they all highly integrate with eachother
- run command
  - you should stagger your command invocations across your fleet of instances

### anti patterns

## features

- automatically aggregates and displays operational data for each resource group through a dashboard
- eliminates the need for you to navigate across multiple AWS consoles to view your operational data
- supports resources in multicloud and hybrid workloads

### pricing

- shiz free yo
  - run command
- shiz cost yo
  - use the pricing calculator

## basics

- maintain security and compliance by scanning your managed instances and reporting on (or taking corrective action on) any policy violations it detects

### SSM Agent

- all other services use the SSM Agent for processing & executing tasks
- install on servers for SSM agent to update, manage, and configure the server.
  - sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service (service prefix: ec2messages).

#### Managed Instances

- any server with the SSM installed and associated with an instance profile with required policies
  - AMIs with SSM Agent preinstalled: Windows, Amazon Linux, and Ubuntu Server

##### server registration methods

- basic steps
  - install and start the SSM Agent
  - assign an IAM role to the server that has the required policies
  - verify the server shows up as a Managed Instance, e.g. via the cli/console
- a new ec2 instance
  - using an AMI with the SSM agent preintalled
  - or install the SMS agent yourself via userData script
  - in instance details config section, assign the role with the instance proflie policies
- an existing ec2 isntance
  - ssh into the instance and install & start the SSM agent
  - configure an IAM role with the instance profile that has the required policies
    - e.g. in the console > instance settings > attach IAM ROle
    - can also be done using a cli/sdk
- an arbitrary server: e.g. on premise / other cloud provider
  - create a managed instance activation
    - e.g. in console > hybrid activations > create activation
      - this is where you assign the IAM Role, access expiration, etc
      - access expiration is when new registrations will no longer be accepted, but existing servers will still contiue to work
    - retrieve the activation code & ID
      - up to 1000 servers can reuse the same code & id
    - install & start the SSM Agent using the activation code & ID,
      - check the docs for the registration cmd
- or use cloudformation to automate the process
- benefits of using Quick Setup: alternative to the manual processes above but only for EC2 instances
  - the server is now managed by the SSM service
  - one-time installation and configuration of cloudwatch agent
    - monthly update of the cloudwatch agent
  - scheduled
    - biweekly update of the SSM agent
    - collection of inventory (servers) metadata every 30 minutes
    - daily scan of inventory to identify missing patches

#### Resource Groups

- a way to logically group managed instances in a specific account across regions

### SSM Services Configuration

- Most SSM services utilize the same components

#### Documents

- where you define and configure tasks to be executed by the SSM agent
  - all SSM services generally require a Document
- an operational playbook containing a series of steps to be executed in sequence
  - versioned and shared across accounts
- managed documents: predefined documents created by AWS for common tasks
  - e.g. RunShellScript is a managed document
- custom documents: !managedDocuments
- configuration: depends on the document type
  - commands: what to execute
  - working directory
  - execution timeout

#### Targets

- most SSM services allow you to specify the target of service execution
- method for matching managed instances
  - matching tags
  - manual selection
  - resource groups

#### Schedules

- most SSM services allow you to specify the schedule of document execution
- how often the document should be executed against targets
- depends on the service type, bug generally
  - On Schedule: execute as defined
    - CRON
    - rate
    - Cron/Rate expression
  - No Schedule: execute only once

#### Compliance Severity

- most SSM services allow you to specify status of execution failure
- determines what happens when document execution fails
  - e.g. flag this execution as Critical
- depends on the service type, but generally
  - Critical
  - High
  - Medium
  - Low

#### Rate Control

- most SSM services allow you to roll out document execution againt targets
- how you stagger cmd invocations
  - concurrency: e.g. execute at most 10% at a time
  - error threshold: e.g. X errors will stop cmd

#### Outpot Options

- most SSM services enable you to specify what to do what document exection logs
  - cloudwatch logs
  - send to s3

#### Tag Editor

- how resources are grouped

### SSM Services

#### State Manager

- secure and scalable state management service
  - state as in server state
  - basically is Run Command on a schedule
- control how/when configurations are applied to managed instances
  - e.g. bootstrapping instances, OS/Software settings, download/update agents on a schedule, network configuration, patching, run scripts, etc
- use cases
  - enforce enterprise wide compliance against a set of running servers
  - toggling system-level services
  - scheduling recurring tasks, e.g. ensuring a port is always closed, system scans, etc
  - collecting inventory
  - running arbitrary scripts at specific lifecycle events
  - etc

##### Associations

- define a state to apply to a set of matching targets
  - i.e. for SSM service X, apply document Y to matching managed instances
  - most SSM services allow you to create associations, and they will show up in state manager
- configuration
  - Document: where you define the state, see elsewhere
  - targets: of the document execution, see elseware
  - schedule: of document execution, see elseware

#### Run Command

- way of executing tasks without connecting remotely to instances
  - replaces the need for bastion hosts, SSH, or remote PowerShell.
  - e.g. software updates, systems monitoring, or any OS/software configuration changes
- use cases: basically any scriptable task to be executed against a set of managed instances
  - system monitoring
  - joining instances to active domain
  - on-demand patching
  - deploying code to instances
  - process management
  - application bootstrap scripts
  - user and account management
- key benefits
  - fully managed instances at no additional cost
  - single view of configuration change
  - no need for SSH/RDP/etc
  - auditability!
  - native integration with IAM

##### Commands

- any action you want to perform on a specific set of instances
- command configuration
  - document: the actual script to run, see elsware
  - targets: method for matching instances; see elseware
  - runtime parameters
  - rate control: how you stagger cmd invocations; see elsware
  - output options: usually either s3 or cloudwatch: see elsware
  - SNS notifications

#### Distributor

- centrally store and systematically distribute software packages while you maintain control over versioning
- create and distribute software packages and then install them using Systems Manager Run Command and State Manager
- use IAM policies to control who can create or update packages in your account

#### Change Calendar

- setup date and time ranges when SSM actions may/not be executed

#### Dashboards

- view operational data from multiple services and automate operational tasks across resources.

##### Compliance

- automatically aggregates and displays operational data for each resource group through a dashboard

#### Inventory

- query & collect information about OS and instance level details
- manage application assets, track licenses, monitor file integrity, discover applications not installed by a traditional installer, and more.
- integrates with AWS Config to add additional inventory logic
  - you send inventory information to config, where config can execute compliance management
  - blacklist applications/configurations/etc
  - automate remiediation actions
- configuration
  - targets
  - schedule
  - parameters: define type of data to gather
    - e.g. applications, files, file paths, network configurations, Windows services, registries, server roles, updates, billing info, and any other system properties
    - custom inventory (e.g. on premise managed instances)

##### Resource Data Sync

- aggregates data collected by Inventory and stores it in a s3 bucket
- once its in the s3 bucket you can visualize it

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

- also integrates with auto scaling groups
- manage your EC2 instances at scale

### VPC

- vpc endpoints for connecting to the SSM service on servers without an IGW
  - use with privatelink to grant instances access to non AWS endpoints, e.g. for retrieving patches

### Cloudtrail

- audit everything SSM does
- can send TRAIL logs to CLOUDWATCH logs so you can query the actions taken

### CloudWatch

- events for tracking state and changes

### CloudFormation

### Athena

- visualize logs output to s3 buckets

### License Manager

- apply licensing policies to managed instances based on the info pulled from inventory
