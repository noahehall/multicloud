# AWS CLI - Systems Manager

## Session Manager

- requires an additional plugin
- enables you to connect to any managed instance

```sh
aws ssm X Y Z

start-session \
  --target abcdefg
  --parameters "e.g. port & local port, etc"
  --document-name ThatUsesTheParameters

```
