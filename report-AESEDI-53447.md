# PostMortem/Root Cause Analysis for Data not sent from AES EDI | JIRA Issue [AESEDI-53447] 

## Date
23-08-2020

## Authors
*Abhishek Yadav*
*Ops Team*

## Status
Resolved

## Summary
The Jira issue [AESEDI-53447] was logged which describe that customer data was not sent from the AES EDI. However, further investigation showed that the file with data was sent but was not processed due to a know error in the AES CIS service (Jira Issue No: [AESCIS-38263]).

## Impact
Precisely 486,000 records were affected and also EDI to CIS monitoring service working in CIS side.

## Root Causes
Sending files with a large number of records while simultaneously running the patching script caused a wreck in the data processing. This issue with AES CIS service futher escalated to data not getting processed leading to missing records.

## Trigger
A large amount of files were not processed.

## Resolution
Reloading the *AES CIS* monitoring service allowed us to spot the missed records that were not discovered automatically. Following this the data file was resent leading to the resolution.

## Detection
Customer created a Jira Ticket to alert us on this failure. *Please refer JIRA Issue: [AESEDI-53447]*


## Action Items
| Action Item | Type | Owner | Bug |
| ----------- | ---- | ----- | --- |
| Define monitoring policy for missing record detection | prevent | Prasanjit Singh | **DONE** |
| Systematic monitoring of data processors in ETL | prevent | Prasanjit Singh | (Jira Issue No: AESCIS-38263)**TODO** |

## Lessons Learned
1. More monitoring plugins and modules to watch this critical part of our infrastructure. 
2. Slack notifications have to be added for alerting the team whenever a data discrepancy is detected in future so that such occurences are prevented in future.
3. Patching opearations should not be executed while data processing is in progress at AES EDI.

## Timeline

2019-06-07 (*all times UTC*)

| Time  | Description |
| ----- | ----------- |
| 11:56 | Missing records discovery |
| 12:05 | AES CIS monitoring module reload |
| 12:11 | Test the AES CIS monitoring module with small sample data |
| 12:16 | Observe behavior of the AES CIS monitoring module to ensure integrity |
| 12:25 | Initiate data processing of records file |
| 13:00 | Completion of the data processing of all 486,000 records |
