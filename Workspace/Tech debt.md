

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


## AWS sso control:
- ClassSupport group defined in `platform-sso-rbac/packages/api/src/manifest
/sso-groups.ts`
```
{
    notes: 'Class Prod support group',
    name: 'ClassSupport',
    awsAccount: 'PCT_PRD',
    description: 'SelfWealth Managed SSO Group for Class Support team',
    members: [
      'lorne.luo'
    ],
  },
```
- Permission defined in `platform-aws-foundations/packages/org-iam-roles/template.yml`
```yaml
Statement:
          - Sid: ListStepFunctions
            Effect: Allow
            Resource: "*"
            Action:
              - states:List*
              - states:Describe*
              - states:Get*
          - Sid: ExecuteStepFunctions
            Effect: Allow
            Resource:
              - arn:aws:states:ap-southeast-2:846765181387:stateMachine:sw-classdatafeed-stepfunction-prod
              - arn:aws:states:ap-southeast-2:846765181387:stateMachine:b2b-datafeeds-cash-transactions-stepfunction-prod
            Action:
              - states:StartExecution
              - states:StartSyncExecution
```

## Exporter class

Is it possible to reuse these portfolio ids between ?
We called `get_consented_portfolios` 4-5 times for per export.

I think we can improve it by extracting all data domain export funcs as an Exporter class, to make it easier to share data between data domains.

```
class Exporter:
    newly_consented_portfolios=None
    removed_consented_portfolios=None
    current_consented_portfolios=None    

    def init():
          newly_consented_portfolios, removed_consented_portfolios, current_consented_portfolios = DbIngestion.get_consented_portfolios)

    def create_movement_transactions_element()

    def create_investment_holdings_element()

    def create_account_details_element()
    
    def create_account_balances()
    
    def generate_doc():
         """entry"""
         return E.EPIDataResponse(
              ....
         )
```