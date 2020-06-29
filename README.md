# Walkthrough: Automating REST API tests using Postman and Azure DevOps 
## Overview
This walkthrough will take you through creating and automating some REST API tests using Postman and Azure DevOps. For this walkthrough, we'll be making use of HSBC's excellent (and public) branch locator API, which you can find here: https://api.hsbc.com/branch-locator.html. We assume you are comfortable creating basic API calls with Postman.    
  
We're going to do the following:
1. Create some API calls  
2. Write some tests against them  
3. Create an Azure DevOps Release pipeline to automate these tests  
## Prerequisites
* You must have installed the latest version of Postman App (https://www.postman.com/downloads/)  
* You must have created a free account with Azure DevOps Services (https://azure.microsoft.com/en-gb/services/devops/)  
* You must be 
* You must be able to access the API above  
## Creating the API calls
(If you don't want to create your own API calls, we've saved our own collection [here](https://github.com/bendalby82/azure-devops-rest-api/blob/master/postman/HSBC.postman_collection.json), which you can just import into Postman)  
1. Create a new Collection in Postman, and set a variable called `hostname` with initial value `api.hsbc.com`  

2. Under that Collection, create some test API calls against the API - you can see my choices in the screenshot below, including my use of that hostname collection variable:  
![Set of tests](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/01-Sketch-Out-Basic-API-Calls.png)   

3. Check your API requests are functioning and green, even though there aren't any tests:  
![Basic requests are green](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/02-Basic-Requests-Are-All-Green.png)  

## Write some tests against them
1. Postman has excellent documentation on how to write tests [here](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/02-Basic-Requests-Are-All-Green.png) - but we want a nice mixture of performance tests, status tests and data tests. Note the use of the requestName property, which will make our tests easier to read further down the line:
![JavaScript tests](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/03-Write-Some-Test-Cases-With-Data-Check.png)  

2. Make sure all of our requests have at least some tests against them:  
![All requests have tests](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/04-Requests-Now-All-Have-Tests.png)
