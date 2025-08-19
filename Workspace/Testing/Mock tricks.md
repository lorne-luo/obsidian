After applied singleton pattern

```
from common.aws_resources import instance as aws_resources_instance
# repleace with 
from common.aws_resources import AwsResources
```

we have to replace the mock statement "

```
@patch("export.contract_note.contract_note.AwsResources.public_domain", "test-domain")
to 
@patch("common.aws_resources.AwsResources.public_domain", "test-domain")

```
