# CLI : STS

## examples

```sh

aws sts CMD ARGS...

# assume a role while passing a session policy to further restrict access
assume-role \
  --role-arn "arn:some:arn:here" \
  --role-session-name "some-name" \
  --policy file://some/session/policy.json
```
