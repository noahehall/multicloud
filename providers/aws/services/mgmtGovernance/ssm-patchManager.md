# Patch Manager

- ensure that software is up-to-date and meets compliance policies.
- select and deploy operating system and software patches automatically across large groups of cloud or on-premises instances and edge devices

## my thoughts

## links

- [Patch Policies](https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-policies.html)

## best practices

- Patch Policies just rolled out, use that

### anti patterns

## features

- simplify OS/Software application pathing process via policies against managed instances
- scan, scan & install white/blacklist specific patches
- schedule & stagger patch roll outs with Maintenance Window
- control instance reboots after patch deployment
- run compliance reports

### pricing

- check main SSM docs for all SSM pricing

## basics

### Patch Baselines

- the old way of doing things
  - check the patch policy section
- rules to
  - auto-approve categories of patches to be installed, such as operating system or high severity patches
  - specify a list of patches that override these rules and are automatically approved or rejected.
- Managed baselines: generally based around security patches
- Custom baselines configuration
  - OS: patch manager needs to know the OS of the managed instance
  - auto approval Rules (max 10): specify how patches are applied to matching instances
    - product: whats being patched
    - type of patch to apply based on
      - severity
      - classification
    - auto approval delay: number of days after patch release date before it can be applied
    - compliance reporting: if a patch cant be applied, set the status to X
    - include nonsecurity patches
  - patch exceptions: specify which patches are automatically approved/disproved and ignore rule settings
    - how you set black/white lists
  - patch sources:
    - linux instances: you can define your source (e.g. apt) repo
    - windows: windows update/sus (server update service)
- patch configuration
  - targets: normal target configuration, or specify a patch group
  - schedule: normal schedule configuration, but also requires you to define a Maintenance Window
  - operation
    - scan and install: install patches & scan for any other updates
    - install only

### Patch Groups

- patch matching targets based on tags
- for each created baseline, apply up to 25 tags

### Patch Policies

- the new and recommended method for configuring your patching operations

## considerations

## integrations

### Config

- theres a config rule for identifying any managed instances that are not patch compliant
