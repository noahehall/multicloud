# Migration Evaluator (fka TSO Logic)

- migration assessment service to accelerate AWS evaluation, planning and migration decisions
- build a use case for migrating to AWS based on existing onpremise resource consumption and estimated cost savings
- create a cost model of a customers ideal future state

## my thoughts

- you have to request an assessment from your AM or on the landing page

## links

- [landing page](https://aws.amazon.com/migration-evaluator/?did=ap_card&trk=ap_card)

## best practices

- atleast a 2 week evaluation period for the Collector

### anti patterns

- evaluator does not
  - support application dependency mapping
    - but does support environment groups: e.g. prod vs dev which can drive cost optimization decisions
    - use application discovery service for depending mapping
  - right size storage: 500 gb on premise will be evaluated at 500gb EBS
    - but does right size servers
- evaluator is not a good fit for
  - servers on non-x86 architecture or non-server hardware
    - e.g. SAN/NAS storage, mainframes, routers, Solaris, AIX
  - providing soft costs as part of TCO
    - e.g. people/labor, disk IOPS, migration fees/tools, professional services
  - instance matching with other cloud providers
  - modeling of cost for replatform or rearchitecture
    - evaluator is for lift and shift to EC2 & EBS

## features

- the first step in MAP and OLA programs
- Install a complementary Agentless Collector to conduct broad-based discovery, or securely upload exports if you have existing inventory.
- Take a snapshot of your current on-premises footprint to fine-tune licensing, view server dependencies, and gain visibility into multiple migration scenarios.
- Analyze your current state, define your target state, and develop a migration readiness plan with projected cloud costs to reach your financial business objectives faster.
- provides inventory assessments with insights to build a migration strategy

### pricing

- shiz free yo

## basics

- key activities
  - provides on premise analysis of compute, storage and microsoft licenses
    - funny AF they single out microsoft
      - automated SQL server discovery
      - ability to model BYOL
      - dedicated hosts
      - SQL Core optimization
    - right sizes workloads based on actual CPU, memory and storage utilization
  - algorithmically determines best fit, lowest-cost replacement for each workload
  - optimized cost modeling scenarios for workloads on AWS
- key insights
  - identify shiz with Microsoft licensing
  - uncover the optimal lowest-cost EC2 option for each workload based on actual use
  - model different options based on workload types
- deliverables
  - develop an initial migration assessment
    - 1 page summary of projected AWS costs
    - the full assessment takes time to complete
  - discover
    - applicable AWS programs, e.g. MAP and AWS OLA
    - fix scoping issues, e.g. missing servers/environments/licensing
  - additional insights with a directional use case
    - on premise estimate
  - analytics engine: cost modeling scenarios to tweak findings and export results
    - discovered compute and storage inventory
    - CPU & RAM utilization, CPU Processor Age, OS
    - right-size EC2 instances
    - Elastic Block Storage recommendations
    - customer provided software licenses
- inputs into cost estimate
  - on premise costs
    - server provisioning: CPU & RAM utilization, CPU Processor Age, OS (windows/linux)
    - attached storage
    - power
    - software licensing: seems to focus specifically on Mirosoft SQL Server
  - AWS costs
    - microsoft:
      - sql server core optimization: BYOL, dedicated hosts or license included purchase plans
    - ec2: right sizing, purchase plans
      - environment groupings: e.g. dev vs prod will have different purchase commitments

### Programs, Incentives and AWS Teams

- BYOL: bring your own license model

#### MAP:

- Migration Accelerator Program
- gives you credits/partner discounts n stuff like that
- have access to external partners that provide expertise/consulting support in migrating to AWS
- you can request a free evaluation for compute, storage and microsoft licensing to see how much you can save by migrating
- requirements
  - yo startup must have incremental annual recurring revenue for AWS workloads that exceed:
    - 1 million bucks
    - 500k in specific markets/segments (that AWS is interested in)

#### AWS OLA:

- Optmization and License Assessment
- provides a microsoft specialist to conduct a deep dive on licensing optimization and insights from a migration evaluator assessment report
- verifies if a customer is qualified for additional incentives for windows workloads

#### Cloud Economics Team

- adds costs for external storage, labor, networking IOPS, professional services, market agility, go-to-market costs
- projects a cost model over several years
- uses actual customer financials to build an on premise estimate
- provides a deep dive on SAP and Oracle

#### DB Freedom:

- abcd

### Collector

- an agent that collects cpu, ram, etc usage data from current on premise workloads
- collects usage data for windows & linux operating systems
- infrastructure discovery and time-series performance data
  - supports VMWare and Hyper-V
- automates SQL server detection
- integrates data provided by
  - customer provided inventory and usage data
  - industry benchmarks if there are gaps in data
  - other assumptions if there is a desire to move fast

## considerations

## integrations

### Migration Hub

- upload migration evaluator assessment into migration hub for:
  - abcd

### MGN

- upload migration evaluator assessment into a MGN blueprint
  - right sizes aws services based on evaluator data
