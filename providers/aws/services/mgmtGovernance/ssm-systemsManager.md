# systems manager fka SSM

- a unified interface for a set of cloudnative, fully managed services for resources on AWS and in multicloud and hybrid environments
  - automated configuration management
  - track and resolve opertional issues
- Main Services (TODO: move them all in here)
  - operations Management
    - explorer
    - OpsCenter
    - [incident manager](./ssm-incidentManager.md)
  - application management
    - [application manager](./ssm-applicationManager.md)
    - [app config](./ssm-appConfig.md)
    - [parameter store](./ssm-parameterStore.md)
      - hierarchical store for configuration & secret data
  - change management
    - automation
    - [change manager](./ssm-changeManager.md)
    - Maintenance Windows
  - node management
    - [fleet manager](./ssm-fleetManager.md)
    - session manager
    - [patch manager](./ssm-patchManager.md)
      - select & deploy OS and software packages across instances
  - other services
    - Connect with ITSM and ITOM Software: its some kind of JIRA integration

## links

- [change calendar](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-change-calendar.html)
- [documents: intro](https://docs.aws.amazon.com/systems-manager/latest/userguide/documents.html)
- [faqs](https://aws.amazon.com/systems-manager/faq/)
- [inventory: tutorials](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-inventory-walk.html)
- [security best practices](https://docs.aws.amazon.com/systems-manager/latest/userguide/security-best-practices.html)
- [ssm agent: hybrid activations](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managedinstances.html)
- [ssm agent: intro](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)
- [user guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state.html)
  [operational capabilities: white paper](https://docs.aws.amazon.com/whitepapers/latest/aws-systems-manager-operational-capabilities/aws-systems-manager-operational-capabilities.html)

## best practices

- become one with:
  - the dashboard
  - Documents
  - State Manager
  - Automation
  - Secrets Manager + Parameter Store
- configure the SSM Agent to automatically send its log data to a log group in CloudWatch Logs for analysis
  - OR configure and use the CloudWatch Agent to collect metrics and logs from your instances instead of using Systems Manager Agent (SSM Agent) for these tasks.
  - the CloudWatch Agent gathers more metrics on EC2 instances and on-premises servers than are available using SSM Agent
- while SSM is split into distinct services, they all integrate with each other and other AWS services
  - get creative, especially on SSM integrating with other AWS services
- run command
  - you should stagger your command invocations across your fleet of instances
- many SSM log output contain sensitive data, you should encrypt the log data in the service settings

### anti patterns

## features

- automatically aggregates and displays operational data for each resource group through a dashboard
- eliminates the need for you to navigate across multiple AWS consoles to view your operational data
- supports resources in multicloud and hybrid workloads

### pricing

- shiz free yo
  - run command
  - Patch Manager
    - ec2 instances
    - on premise OS patches
- shiz cost yo
  - everything else

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

## SSM Services

### SSM Services Configuration

- Most SSM services utilize the same components

#### Documents

- where you define and configure tasks to be executed by the SSM agent
  - all SSM services generally require a Document
  - can pretty much invoke any AWS API Action
- an operational playbook containing a series of steps to be executed in sequence
  - versioned and shared across accounts
- managed documents: predefined documents created by AWS for common tasks
  - e.g. RunShellScript is a managed document
  - you can see the document content and use it for a custom document
- custom documents: !managedDocuments
  - builder: specify document attributes in input fields
    - role: by default documents are executed in user context, but supply a role for users without the appropriate permissions
      - you can then give users IAM access to the document, instead of the underlying resource
    - input parameters
    - outputs: for use in other processes
    - target type
    - document tags
    - steps: serial execution for workflows
      - name
      - action type: theres bunches of different types
        - run a script
        - wait on a resource
        - invoke AWS API actions
        - assert/change a resource state
        - pause for manual approval
        - etc
  - editor: specify document attributes as json
    - IMO more powerufl than the builder, you can switch back n forth between modes
    - e.g. you can copypasta managed document content then edit
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
    - CRON schedule builder: wyswyg
    - rate
    - Cron Rate expression: cron syntax
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

- how Resource Groups are created

#### Resource Data Sync

- aggregates data across accounts & regions collected by Inventory, Explorer, and other SSM services and stores it in a s3 bucket
- once its in the s3 bucket you can visualize it
- configuration
  - accounts: for each sync, you can specify which accounts are included in the aggregation
  - regions: see above but for regions

### Dashboards

- view operational data from multiple services and automate operational tasks across resources.

#### Compliance

- automatically aggregates and displays operational data for each resource group through a dashboard

#### Explorer

- customizable Ops dashboard for monitoring resources with an aggregated view of ops data across accounts and regions
  - reports, insights and analysis into the operational health and performance of your AWS/hybrid environment
- group, filter, categorize in the console/api
- uses Resource Data Sync to aggregate additional data across accounts & regions
- basic workflow
  - specify IAM role: enables creation of OpsItems from cloudwatch events
  - configure opscenter rules
    - rules that automatically create OpsItems from cloudwatch events
  - configure opsdata sources
  - specify tags for reporting: additional dimensions for categorizing & filtering OpsData
  - setup resource data sync for aggregation across accounts & regions
  - setup data export
    - s3 bucket
    - SNS topics
  - create dashboards

##### OpsData

- bunches of shiz about resources across accounts and regions and other dimensions
  - ec2 instances
  - System Manager patch compliance
  - OpsCenter OpsItems
  - etc etc
- can be exported to s3 and synchronized across Organization accounts

### State Manager

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

#### Associations

- define a state to apply to a set of matching targets
  - i.e. for SSM service X, apply document Y to matching managed instances
  - most SSM services allow you to create associations, and they will show up in state manager
- configuration
  - Document: where you define the state, see elsewhere
  - targets: of the document execution, see elseware
  - schedule: of document execution, see elseware

### Run Command

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

#### Commands

- any action you want to perform on a specific set of instances
- command configuration
  - document: the actual script to run, see elsware
  - targets: method for matching instances; see elseware
  - runtime parameters
  - rate control: how you stagger cmd invocations; see elsware
  - output options: usually either s3 or cloudwatch: see elsware
  - SNS notifications

### Distributor

- versioned package repostory tightly integrated with IAM
- centrally store and distribute versioned software packages
  - software agents
  - applications
  - drivers
- use IAM policies to control who can create or update packages in your account

#### Packages

- location: s3 bucket
- file types: any file, e.g. deb, rpm, etc
- requirements: platform, version, architecture
- can install
  - ondemand: via Run Command
  - schedule/automatically on new instances: via State Manager
- can uninstall: via arbitrary script

### Change Calendar

- setup date and time ranges when SSM actions may/not be executed
- create & share open/closed calendars for important business events with white/blacklisted time ranges
- organizations can create a single master change calendar for events across all accounts
- general workflow
  - create calendar
  - add scheuled events
  - query calendar state using Automation/API
    - with Automation you need to add a step that checks the calendar before the Automation executes the Document
    - if open, take action
    - if closed, block/reschedule actions
  - create approval-based override actions

#### Calendar

- open: allow event actions by default; except during an existing event
  - e.g. general change management
- closed: disallow event actions by defualt; except during a prescheduled event
  - e.g. CI/CD pipelines
- events: schedule potentially disruptive events based on the Calendar state (open/closed)

### Inventory

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
- utilizes Resource Data Sync

### Maintenance Windows

- define a time window / schedule for patch manager, run command and other disruptive tasks
  - SSM Patch Manager,
  - SSM Run Command,
  - SSM Automation,
  - Lambda Fns,
  - Step Fns
  - scheduling tasks in other supports resources, e.g. S3, SNS, etc
  - etc
- configuration
  - schedule
  - duration: how long the maintenance window lasts
  - targets: tags, resource groups, etc
    - allow unregistered targets: whether instances that meet the targeting criteria in the future should be in cluded
  - tasks:

### Automation

- safely automate & share common and repetitive IT operations and management tasks across multiple accounts and Regions
- simplifies any scriptable/automation task via workflows and playbooks
- track the execution of each step, require approvals, incrementally roll outs, and automatically halt if errors occur.
- use cases
  - creation & maintanence of AMIs
  - one-click configuration tasks, e.g. applying configuration
  - workflow tasks with manual approvals
  - remediation tasks
  - self-service actions via Service Catalogue
- architecture
  - assumes current user context by defualt
    - optionally can specify a service role
  - create custom automation playbooks
  - run the automation playbook

#### Playbooks (documents)

- works across multiple accounts and AWS Regions.
- can be scheduled in a maintenance window, AWS/multicloud/onpremise resource events, or executed directly through the AWS Management Console, CLIs, and SDKs.
- share automation documents across accounts
- managed playbooks: created by AWS, you just specify the input params
  - are segmented by categories, theres bunches
  - you should dive into how the managed documents are configured before creating your own
- custom playbooks: automation pretty much any AWS API Action
  - dynamic parameters
  - conditionally branch based on step results
  - configure approvals as part of workflow

### Session Manager

- auditable, interactive webshell via one-click/CLI/SDK and remote desktop access to AWS/hybrid/onpremise servers
  - the SSM agent creates an outbound connection to the SSM service so you dont have to open up firewall ports
- use cases
  - centralized access control (resources, root, nonroot) via IAM
  - auditable: send logs to s3, cloudwatch, cloudtrail
  - no need for inbound ports, ssh keys, or bastion hosts
  - enable connections to servers without internet access
  - port forwarding/redirection (e.g. for RDP, SSH, webservers, etc) via SSM Documents (check the cli docs)
- console
  - see live sessions
  - terminate sessions
  - preferences: e.g. KMS, log output, etc
    - run cmds as user: define a tag, apply it to the user, and the tags value will be the user for the ssh session
      - the tag value must be a user on the managed instance
  - connected to instances, audit session cmds, etc etc

### OpsCenter

- centralized ops/IT dashboard & remediation task management for cloud resources
- view, investigate, and resolve operational issues related to resources on AWS, multicloud and hybrid environments using the Systems Manager console or via the Systems Manager OpsCenter APIs.
- aggregates and stadardizes opsItems rules across services

#### opsItems

- operational issues and their associated metadata
  - event, resource and account details
  - related ops items and aws resources
  - related AWS config changes
  - cloudtrails
  - etc etc, bunches of shiz
- can be created from:
  - cloudwatch events
  - console or API
- can be integrated with external task management products via SNS

#### Remediation

- uses SSM Documents to automate OpsItem remediation

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

- logs for tracking state and changes
- events for notifications when patterns/specific actions take place

### CloudFormation

### Athena

- visualize logs output to s3 buckets

### License Manager

- apply licensing policies to managed instances based on the info pulled from inventory

### Service Catalogue

- create self-service actions that execute automation playbooks
- give users automation access at the playbook level, without permissions to the underlying resource

### Config

- most SSM services integrate with config for compliance management, enhanced logic and automation steps

### EC2

- SSM is all about managing EC2 servers, but also important
  - connect to any managed instance via Session manager without ssh keys
  - bunch of automation stuff

### SNS
