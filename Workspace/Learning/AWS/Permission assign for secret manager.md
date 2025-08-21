Aws may attach a random string at the end of ARN, is better to always attach a traling *

its secret name may be `vendor/aaaa/secrets`

but the arn may be `arn:aws:secretsmanager:${AWS::Region}:${AWS::AccountId}:secret:vendor/aaaa/secrets-Sfdsfwekc`


# Soluition

Simply attach traling * at end of the arn

```
arn:aws:secretsmanager:${AWS::Region}:${AWS::AccountId}:secret:vendor/aaaa/secrets*
```
