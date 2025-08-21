
- always lock the built-in package versions, do not relay on the package versions built-in lambda container, the built-in packages can be found in docker image `public.ecr.aws/lambda/python:3.12-x86_64` 

## Limitation
- 15 mins longest duration
- 250Mb for the package size(umcompressed)
- Step F==unctions max request payloa==d < 256KB
