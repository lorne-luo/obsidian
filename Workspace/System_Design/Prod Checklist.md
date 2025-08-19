- Make sure all PRs in other repos arge merged in
  - backend-infra for S3 SNS (https://github.com/selfwealth/product-backend-infra/pull/124)
  - bigsite-service for bigsite permission
  - storage repo(b2bDatafeedStorageService) to create aurora database
- update bulk_insert_mappings to make it commit in batch
- Check all # todo in source code
- Review all warning and lint issue(sonarcloud)
- Review all security issues in snyk
- lock requirements.txt versions for all package
- Check datadog dashboard and alert
- check mssql server permission
- outdata clean up





# Backlog refinement

* Ingested data expiry(DB and S3)
  * S3 Lifecycle expiration rule
  * Lambda to cleanup postgres
* Update bulk\_insert\_mappings to make it commit in batches
* Refine session

# Pre-Checklist

* Lock requirements.txt versions for all packages

  * Migrate pip to uv
  * Update minor versions
* Squash DB migrations
* Review Snyk and SonarCloud
* Review warnings and lint issues

  * Ruff
  * flake8
* Deploy PRs

  * backend-infra for S3 SNS ([https://github.com/selfwealth/product-backend-infra/pull/124](https://github.com/selfwealth/product-backend-infra/pull/124))
  * Deploy repo b2bDatafeedStorageService to prod to create Aurora db
  * Update Bigsite DB permission([https://github.com/selfwealth/product-bigsite-services/pull/1149](https://github.com/selfwealth/product-bigsite-services/pull/1149))
  * Deploy BGL project


# Post-Checklist

* Configuration

  * setup secret manager
