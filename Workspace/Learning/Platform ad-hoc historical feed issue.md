
# Issue
We forgot to add  `ingested_portfolio_ids=[]` in the payload to disable the current day's incremental data export.

# Explain

Leverage AI to draw image show what happen
![[Pasted image 20251211133538.png]]

I just found we have a misconfiguration in the historical data, which may have caused us to also send today's incremental data.

The file BGL-ASW_20251209_SWADVISERS_009.xml actually includes these data:

|   |   |   |
|---|---|---|
|**Date range(AEST)**|**User token**|**Data explain**|
|2023-07-01 00:00:00   <br>2025-11-27 09:05:54|pfNWjpjs8KMNyiBtStEXrr|Historical data since 2023-07-01  for pfNWjpjs8KMNyiBtStEXrr|
|2025-12-09 09:06:03  <br>2025-12-09 17:02:15|All consented users(include  pfNWjpjs8KMNyiBtStEXrr)|Partial daily Incremental data generated on day time of 9 Dec|

And for tomorrow's daily scheduled BGL-ASW_202512**10**SWADVISERS_0*10*.xml, we will send some duplication data:

|   |   |   |
|---|---|---|
|Date range(AEST)|**User token**|**Data explain**|
|2025-12-09 09:06:03  <br>2025-12-10 09:xx:xx|All consented users|Whole day incremental data generated between 8 Dec to 9 Dec|
|It will include duplication data with BGL-ASW_20251209_SWADVISERS_009.xml|   |   |
  
We can catch up tomorrow morning, sorry for the inconvenience.


# Solution

Create a 