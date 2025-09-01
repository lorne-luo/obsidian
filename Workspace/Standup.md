# 2025-08-30
- checked through all data domain 's ingestion, they are commit in effective way
- create auto_session decrator to replace the session_scope context manager

# 2025-08-29
- AP-563 reviewed  docs as sqlalchemy provided too many way to manipulate the db
- did a benchmark to insert 300k CashTransaction in different ways
	- ORM commit one by one: 930.96s
	- ORM add into session one by one, commit together: 28.43s
	- ORM session.session then commit: 29.97s
	- Core Bulk save objects: 11.73s
	- Core Bulk insert mappings: 7.96s
	- Core normal insert: 8.85s
- For ANZ transaction file ingestion it's good,  will review our ingestion code for other data domain, make sure it use effective way to commit db
- before I created a auto session management, polish and add documentation for it, 
#### Retro
https://selfwealth.atlassian.net/wiki/spaces/CTP/pages/edit-v2/2551218177

# 2025-08-28
Sorry I can attend standup due to having appointment this morning
Yesterday fixed the test and deploy pipeline, it now works with the newly introduced container. But I am still waiting AWS Platform team to merge the container update, so I can refer the correct container tag.
Today I will move to the AP-563 to refine the postgres batch insert. 

# 2025-08-27
- most of time are in meeting
- fix sam deploy pipeline, asked security team reviewed uv and added it into python's docker container
- kicked of batch insert's ticket

# 2025-08-26
- fix sam deploy pipeline
- simplified test configuration
- db migration pipeline still broken

changes:
token to replace broker id to link to bank account number

# 2025-08-25
- merge several

# 2025-08-23
- Solve first alembic revision conflict
- Review BGL single AUD account feature
- Created two new jira tickets

# 2025-08-22

- For the multi account export
    - create `account_id` and `account_id_currency` (or profile_id FK) field in data table
        - InvestmentHolding
        - CashTransaction
        - CashBalance
        - InvestmentTrade
    - create new lambda to transfer and fill `account_id` and `account_id_currency`
    - In export step, convert the query result in to dict, index by `account_id` or `account_id_currency` according to `export.is_single_account_export`
    - merge them with Account

# 2025-08-21

- merged 561 and 564
- reviewing 565
- One AUD account for BGL
- updated Go-live task

# 2025-08-20

- fixed a small arguments bug for stepfunction, sftp works on aws dev
- refine 564 and 561, updated to use uv to handle the

# 2025-08-19

- AP-561 Raise PR for python version upgrade
- AP-564 AP-564 Create XSD copy for each target, making owners and advisors optional for BGL
- Todo
    - AP-562 ingested data expiration

# 2025-08-18

- Finished AP-561, upgraded to newest patch version for all dependency pakcage, for some package update to newest minor version
- integration test on stg, checked the xsd error , only Owners and Advisors under Account Details

# 2025-08-11

- added cashactive account number and bsb in postgres model
- ingested them from bigsite TradingSignupCashAccountHistory
- need more time to polish the export query, in CLASS-cash I created a configuration to decide export Cashactive or ACMC, different case for BGL
- after the ACMC migration we still receive 0 balance in CA ANZ file, we will get

# 2025-08-08

- investigate AP-536, found it's different than I expected, note todo in the comment
- having another appointment this noon, will be off 2hr between 1pm-3pm

TODO

- individual OKR
- performance review

# 2025-08-07

- AP-468 pushed SNS feature to aws dev for CLASS-cash, works well for test run, merged into main
- dont want push to prod now, we can deliver together with my next ticket the db query update
- AP-507 already start to tune the sql query, a little bit confused because of inconsistence of stg's bigsite, today may consult JB for help
- reviewed a series of PR from Matt for prod readiness, I also have a code lint branch on my local, will finalize it when I got time
- will have a GP appointment at lunch time

# 2025-08-06

- pushed ap-468 to dev tested for anz/apl ingest lambda in BGL project, works well
- also made the similar change for CLASS-cash, today will also to push to dev and have a test
- once class-cash test passed, we can have a CR to push the SNS configuration and Lambda event trigger to prod
- talked George about the internal sftp server, here is no available sftp server on prod, so we can only use selfwealth target in dev/stg
- created the related ssm/secret manager on dev and stg, the sftp lambda can works with these sftp configure, no connection failure anymore, I will raise a small PR for selfwealth target

# 2025-08-05

- update feedback for singleton aws resource
- finished the code change for ANZ/PCL ingestion lambda, involve 4 lambdas and the step function update, today will push to dev conduct integration testing

# 2025-08-04

- catched up with George about the SNS encrytion, the limitation is E3 event not works for the AWS manager KMS, so we decide to remove in encryption for the SNS
- raised a pullrequest which applied singleton for aws resource retrieving
- postgres migration pushed to stging, add works well for Matt's first model changeset

**todo**

- start to refactor ANZ and PCL ingestion Lambda to works SNS

# 2025-08-01

- for ap-468, I deployed the SNS topic for S3 and linked ANZ/PCL ingest lambda to SNS
    - Have to remove the encryption for KMS SNS, as if encrypted the s3 events can't be passed to lambda, george, not in yesterday I would like him know my change
    - Another change is our occurring to injection is accepting s3 events, what was even received from event is SNS event, so we have to create some warrant lambda to receive both S3 events and SNS event. SNS event works for when the file arriving in S3 bucket, S3 even works for historic file ingestion
- finally merged ap-392 postgres migration, tested on dev it works well, Today I will push to stg to let alembic handle stg db
- it's not a decent way as base64 is not encrypted encoding, and for the split jobs we have to manually approve twice if review require enabled
- it pass secret vars between GHA jobs via base64 encoding, George and Bhushan happy with that, but here actually have a potential nice solution which is get aws access key from the ec2 metadata then pass it into the python3.12 container
- Matt's PR is reviewing
- will review shivendra's new PR

# 2025-07-31

- Update SAM to link sns with ANZ/PCL ingest Lambda, today I will push everything to aws dev for a integration test
- Matt's PR started but not finished
- spend whole afternoon face to face catch up with Bhushan and George solve the dependency install issue in container, still have a secret value passing issue between github action jobs. They are happy for me to pass the secret in base64 encoding, I will have a try to pass the ec2 host 's IAM role or access key into the container.
- Today will have appointment in the noon

# 2025-07-30

- finalized 359 sftp lambda for contract note
- try to use the new py312 container in the pipeline, still met issue, alpine not use glibc
    - use 3.12-slim, one vnlerability
    - create custom action to install python version on amazon linux

# 2025-07-28

CICD pipeline for db migration , need turn to george

Update for feedback for Contract note's SFTP lambda

Reviewed Matt's advisor export

## todo

catch up with george for

- create secret manager
- python 3.12 container have no aws resource access

# 2025-07-25

- Tested sftp lambda on aws dev environment, no
- integrate sftp lambda with contract node, but not test as no contract note can generate on aws dev
- Mark export as delivered in sftp lambda
- Review Shivendra's PR

### discussion

- the stepfunction chart for contract note export
- the export id, mark as delivered
- create test purpose secrect manager sftp credential
- disable selfwealth target on prod?

# 2025-07-24

- Move Class-cash schedule time to 7.30 am, deployed to prod, looks all good this morning, expect this change can eliminate the file arrive late alert
- Applied retry to both sftp connection and upload, should able handle the unstable netwrok's issue
- Send custom metrics for sftp upload

Todo

- test sftp on aws environment, I got the internal sftp server info from JB, I dont want to update the secret manager for sftp cerdential on aws, so will do some hack on lambda source code to conduct the test
- create datadog dashboard for sftp uploading