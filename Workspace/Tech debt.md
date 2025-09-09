

## Project tech debt
- GHA pipeline merge 3 node into one to reduce build/deploy time
	- we have to approve 3 times to deploy to single aws env
	- passing db password in base64 encoding between pipeline jobs
	- accelerate by introduce container
	- more slack notifications
- Migrate old apps into the new platform
	- class trade
	- class cash
- Testing
	- some test case are duplicated
	- fixtures can be simplified and well organised
	- make test data well-organised
		- polish the predefined data on local bigsite
		- sort out fixture
		- improve  consistent of test data
	- test coverage 95.72%
	- how to test stepfunction definition
	- notify to slack group
- Reconcilia
	- match our custom metrics with data team's snowflake
	- from our export trace back to the data source bigsite and s3, implement it as independent way
- UAT environment
	- need a way to track bigsite schema change
	- create clean and consistent test data on stg
		- create some agentic lambda on dev or stg to mimic user, generate test data every day

## Project OKRs
- Optimize the data feed performance
- Reduce unplaned manually re-trigger times
- keep major tech debt tickets less than
- keep bugfix ticket less than
- improve CI/CD pipeline to cuts our average build and deploy time by 30%



Personal
- reduce Review time、Merge time
- accelerate the deployment piepeline