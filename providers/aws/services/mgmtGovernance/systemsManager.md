# systems manager fka SSM

- Manage your resources on AWS and in multicloud and hybrid environments
- a unified user interface where you can track and resolve operational issues across services and cloud providers

## links

- [accessing secrets manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/integration-ps-secretsmanager.html)
- [security best practices](https://docs.aws.amazon.com/systems-manager/latest/userguide/security-best-practices.html)
- [state manager user guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state.html)
- [faqs](https://aws.amazon.com/systems-manager/faq/)
- [ssm-agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent.html)

## best practices

- become one with the dashboard
- configure the SSM Agent to automatically send its log data to a log group in CloudWatch Logs for analysis
  - OR configure and use the CloudWatch Agent to collect metrics and logs from your instances instead of using Systems Manager Agent (SSM Agent) for these tasks.
  - the CloudWatch Agent gathers more metrics on EC2 instances and on-premises servers than are available using SSM Agent

### anti patterns

## features

- automatically aggregates and displays operational data for each resource group through a dashboard
- eliminates the need for you to navigate across multiple AWS consoles to view your operational data

### pricing

- theres a free tier that microtransacts you to death
- use the pricing calculator

## basics

- maintain security and compliance by scanning your managed instances and reporting on (or taking corrective action on) any policy violations it detects
- operations hub for your AWS applications and resources, and is broken into four core feature groups.
  - operations Management
    - [explorer](./systemsManager-explorer.md)
    - [opsCenter](./systemsManager-opsCenter.md)
    - [incident manager](./systemsManager-incidentManager.md)
  - application management
    - [application manager](./systemsManager-applicationManager.md)
    - [app config](./systemsManager-appConfig.md)
    - [parameter store](./systemsManager-parameterStore.md)
  - change management
    - [automation](./systemsManager-automation.md)
    - [change manager](./systemsManager-changeManager.md)
    - [maintenance windows](./systemsManager-maintenanceWindows.md)
  - node management
    - [fleet manager](./systemsManager-fleetManager.md)
    - [session manager](./systemsManager-sessionManager.md)
    - [patch manager](./systemsManager-patchManager.md)
- other services
  - Connect with ITSM and ITOM Software: its some kind of JIRA integration

### SSM Agent

- installed and configured on servers
  - makes it possible for Systems Manager to update, manage, and configure the server.
  - sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service (service prefix: ec2messages).
- managed instance: any server with the SSM installed; a machine that has been configured for use with Systems Manager.
- store agent configuration settings in the Systems Manager Parameter Store for use with the CloudWatch Agent
- use cases
  - manage server configuration
  - Use SSM to connect to your hosts, not exposing port 22.
  - Log SSM API calls with CloudTrail.
  - Sending instance logs to CloudWatch Logs.
  - Configure CloudWatch Logs for Run command.
  - monitor Run Command metrics using CloudWatch.

### console

- view operational data from multiple AWS services and automate operational tasks across your AWS resources.

### Compliance Dashboard

- automatically aggregates and displays operational data for each resource group through a dashboard

### Inventory Dashboard

- collects information about system configurations and installed applications
- applications, files, network configurations, Windows services, registries, server roles, updates, and any other system properties
- manage application assets, track licenses, monitor file integrity, discover applications not installed by a traditional installer, and more.

## Security

## considerations

## integrations

- it integrates with all the foundational ops services
  - cloudtrail, cloudwatch, config, trusted advisor, personal health dashboard, and EC2 instances

### EC2

- see your ec2 instances, any on-premises servers, or virtual machines in your hybrid environment, communicating with ec2messages. endpoints.
- information about executions, commands, scheduled actions, errors, and health statuses to log files on each instance.
- Logs files by manually connecting to an instance or automatically send logs to CloudWatch Logs.

### VPC

### Cloudtrail

### CloudWatch
