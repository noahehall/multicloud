# Process

## key questions

- before you can design a system, you need to understand the business requirements, existing infrastructure, and capabilities of the IT determine to manage and iterate
- 4 levels of technical maturity
  - manual
    - infrastructre is provisioned through a UI/CLI
    - configuration changes do not leave a traceable history, and arent always visible
    - limited or no naming standards in place
  - semi-automated
    - infrastructure provisioned thorugh a combination of UI/CLI, IaaC and scripts/config management
    - traceability is limited, e.g. different record-keeping methods used across the organization
    - rollbacks hard to achieve due to differing record-keeping methods
  - infrastructure as code
    - provisioned via a tool (e.g. Terraform)
    - provisioning and deployment processes are automated
    - infrastructure configuration is consistent, with all necessary details fully documented (nothing siloed in sysadmins head)
    - source files stored in version control to record editing history, and if necessary, rollback to older versions
    - configuration is split into modules to promote consistent reuse of organizations common architectural patterns
  - collaborative infrastructure as code
    - users across the organization can safely provision infrastructure, without conflicts and with clear understanding of their access permissions
    - expert users can produce standardized infrastructure templates, and beginners can consume those to follow infrastructure best practices
    - access controls per application-environment pair helps committers and approvers on workspaces protect production environments
    - functional gorups that dont directly develop infrastructure code have visibility into infrastructure status and changes
- determining the current infrastructure configuration and provisioning practices
  - how do you currently manage infrastructure?
    - through a UI/CLI ?
    - reusable cli scripts?
    - infrastructure as code tool?
      - e.g. terraform / cloudformation
    - general-purpose automation famework?
      - e.g. jenkins + scripts / jenkins + teraform
  - what topology is in place for your service provider account?
    - flat structure, sigle acocunt: all infra provisioned within the same account
    - flat structure, multiple accounts: infrastructure is provisioned using different infrastructure providers with an account per environment
    - tree hierarchy: features a master billing account, audit/security/logging account, and project/environment spcific infrastructure accounts
  - how do you manage the infrastructure for different environments
    - manual: no configuration managent in place
    - siloed: each application team has its own way of managing infrastructure
    - IaaC with different code base per environment: leads to untracked changes from on environment ot the other if there is no change-promotion within environments
    - IaaC: with a single code base and different variables for each environment
      - all resources, regardless of environment, are provisioned with the same code, ensuring that changes promote through your deployment tiers in a predictable way
  - how do teams collaborate and share infrastructure configuration and code
    - N/A: IaaC is not used
    - locally: infrastructure configuration is hosted locally and shared via email, documents, spreadsheets
    - ticketing system: code shared through journal entries in change requests/proboem/incident tickets
    - centralized without version control:
      - i.e. changes are not versioned and rollback are oly possible through restores from backups/snapshots
    - through version control system (e.g. git)
  - do you use reusable modules for writing infrastructure as code?
    - everythin is manual
    - no modularity: e.g. as one-off configurations that users usually dont share/reuse
    - teams use modules but dont share across teams
    - modules are shared organization-wide
      - e.g. similar to shared software libraries, a module for a common infrastructure pattern can be updated once and the entire organization benefits
- whats your current change workflow?
  - how do you govern the access to control changes to infrastructure
    - access is not restricted/audited: everyone has flexibility to create, change and destroy all infrastructure
    - access is not restricted, only audited: easier to track changes after the fact, but doesnt proactively protect stability
    - access is restricted based on service provider account level: team members have admin access to different accounts based on the env they are responsible for
    - access is restricted based on user roles: all access is restricted based on user roles at provider level
  - what is the process for changing existing infrastructure?
    - manual changes by remotely logging into machines
    - runtime configuration management tools: lets you quickly iterate but generaly dont produce static artifacts so the outcome isnt 100% verifable or repeatable
      - e.g. puppet, chef, etc
    - immutable infrastructure: can be replaced for every dpeloyment (rather than updated in place) using static deployment artifacts
      - maintaining sharp boundaries between ephemeral layers and state-storing layers, immutable infrastructure can be much easier to test, validate, and rollback
      - e.g. images & containers
  - how do you deploy applications?
    - manually (ssh, rsync, etc)
    - with scripts (fabric, capistrano, etc)
    - configuration management tool (chef, puppet, ansible, salt, etc) or by passing userdata scripts into cloudformation/terraform
    - with a scheduler: kuernetes, nomad, mesos, swarm, ECS, etc
- whats your current security model
  - how are infrastructure service provider credentials managed?
    - hardcoded in source
    - infrastructure provider roles (e.g. ec2 instance roles for AWS)
      - enables granting machine permission to make API rquests without giving them a copy of your actual credentials
    - secrets management solution (vault, keywish, etc)
    - shortlived tokens: most secure methods since temp credentials you distribute expire quickly and difficult o epxloit
      - most complex implementation as well
  - how do you control users and objects hosted by infrastructure providers (e.g. logins, access and role control, etc)
    - commmon admin/superuser account shared by engineers
    - indiviudal named user accounts: doesnt scale very well as the team grows
    - LDAP and/ active directory integration: requires additional architectural considerations to ensure that the providers access into your corp network is configured correctly
    - single signon through OAuth or SAML
      - providers token-based access into your infrastructure provider while not requiring your provider to have access to your corporate network
  - how do you track changes made by different users in your infrastructure providers env
    - no logging in place
    - manual changelog: users manually write down their changes in a shared document
    - logging all API calls to an audit trail/log management service (e.g. cloudtrail, loggly, splunk)
  - how is access of former employees revoked
    - immediately, manually
    - delayed, as part of the next release
      - e.g. when your release process is extremely coupled and most security changes pass through a CAB meeting in order to be executed in production
    - immdiately, writing a hot-fix in the infrastructure as code
      - the most secure option and should occur before employee leaves the building

## key principals and best practices

- general design principles (cloud perspective)
  - technically understand capacity needs: never accept idle resources/performance implications of limited capacity
  - test systems at production scale: create a production-scale test environment on demand, run tests, then decomission resource
  - automate everywhere you can: create and replicate workloads at low cost to avoid the expense of manual engineering effort
    - track changes, audit impact, and rollback parameters when necessary
  - allow for evolutionary architectures: architectural decisions should never be static, one time events
    - and business & technicaly evolves, so should your architecture
  - data should always drive architecture decisions: collect data on how your architectural choices affect the behavior of your workload
    - make factbased decisions onw how & where to improve
    - cloud infrastructure is code; treat it as such
  - improve through game days
    - understand where improvements can be made
  - learn from all operational failures: drive improvement through lessons learned from all operational events and failures
    - share what is learned across temas and through the entire organization

### system design artifacts and actions

- game days: simulation events to reularly review and validate that all procedures are effect and that teams are familiar with them
  - spin up production scale infrastructure, run your tests, then tear it all down
  - test workloads and the team responses to simluated events
- pre-mortom exercises: to identity potential sources of failure so that they can be removed or mitigated
  - test your failure scenarios and validate your understanding of their impact
  - test your response procedures to ensure they are effective and that teams are familiar with their execution

### system breakdown

- change control system
  - formal process to coordinate and approve changes to a product/system
    - minimize disruption to services
    - reducing rollbacks
    - reducing overall cost of changes
    - preventing unecessary changes
    - allowing users to make changes without impacting changes made by others
- monitoring
  - single dashboard to view the status and compliance of all infrastructure and deployed resources
    - analize, identify, and quickly fix misconfigurations and malfunctions

## HECCYA Modeling

- generalizes the environment of an applicaiton, and other applications, services and the users that exist within the environment, independent on how the applications and services themselves are designed
  - captures the design structures of various systems and elements so that they can be reused
- the process for reviewing an architecture is a constructive conversation about architectural decisions, and is not an audit mechanism

### hardware

- local
- cloud
- multi-cloud
- hybrid-cloud

### environment

### component

### communication

### Yielding

### Architecture

- the degree to which a system fits into a particular architecture
