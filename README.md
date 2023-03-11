## Disclaimer

**THIS CODE IS PROVIDED *AS IS* WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS OR IMPLIED, INCLUDING ANY IMPLIED WARRANTIES OF FITNESS FOR A PARTICULAR PURPOSE, MERCHANTABILITY, OR NON-INFRINGEMENT.**

## Minimal Path to Awesome

- Clone this repository
- in the command line run:
  - `npm install`
  - `gulp bundle --ship && gulp package-solution --ship`

#### Local Mode

This solution doesn't work on local mode.

<img src="https://pnptelemetry.azurewebsites.net/sp-dev-fx-extensions/samples/js-application-appinsights-advanced" />

Sample KQL Queries that's part of my YT Video
https://www.youtube.com/watch?v=FH26zvkUwL8&t=145s


KQL Queries Executed
=================
Data Mining Queries
=================
Query 1: 
pageViews
| where url startswith "https" and duration > 1000
| sort by duration desc


Query 2:
pageViews 
| where url startswith 'https’
| extend user = parse_json(tostring(customDimensions.CustomProps)).UserEmail
| distinct tostring(user)


Query 3:
pageViews 
| where url startswith 'https'
| extend user = parse_json(tostring(customDimensions.CustomProps)).UserEmail
| where user == 'user@yourtenant.onmicrosoft.com' and duration >= 1000
| project user, url, duration
| sort by duration desc



Build Dashboard Queries
====================
Query 1:
pageViews
| where duration != 0
| summarize NumberofUsers = dcount(user_Id), AverageDuration=avg(duration) by url
| sort by AverageDuration desc
| render columnchart 

Query 2:
let uniqueUsers = pageViews
| distinct user_Id, url;
let testDuration = pageViews
| where url startswith "https"
| summarize NumberofUsers =dcount(user_Id) ,AverageDuration=avg(duration) by url;
let myResult = testDuration
| where AverageDuration > 1000 and NumberofUsers > 0
| sort by AverageDuration desc;
myResult
| join (uniqueUsers) on url
| sort by AverageDuration desc 
| project-away url1



Create Alert Query
===============
let uniqueUsers = pageViews
| distinct user_Id, url;
let testDuration = pageViews
| where timestamp > ago(5m)
| summarize NumberofUsers =dcount(user_Id) ,AverageDuration=avg(duration) by url;
let myResult = testDuration
| where AverageDuration > 5000 and NumberofUsers > 0
| sort by AverageDuration desc;
myResult
| join (uniqueUsers) on url
| sort by AverageDuration desc 
