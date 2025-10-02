We were trying to update env var of Lambda in SAM template with :

```
-          CLASS_SFTP_HOST: "uatsftp.hubconnect.com.au"
+          CLASS_SFTP_HOST: !If
+            - IsProd
+            - "sftp.hubconnect.com.au"
+            - "uatsftp.hubconnect.com.au"
```


## Issue
Error raised in GHA SAM deployment:

`arn:aws:lambda:ap-southeast-2:468746937115:function:b2b-datafeeds-cash-transactions-cash_class_export-dev:124 already exists in stack arn:aws:cloud formation:ap-southeast-2:468746937115:stack/b2b-datafeeds-cash-transactions-dev/a776d4d0-a780-11ef-971e-0adb7dc6714f Resource update cancelled. The following resource(s) failed to create: [CashClassExporterFunctionVersion694fafa6b5]. The following resource(s) failed to update: [CashFeedStateMachineRole]`

## Solution:

- Manually delete `CLASS_SFTP_HOST` env var from Lambda and rerun SAM deploy
- Introduce some essential code change into the build package