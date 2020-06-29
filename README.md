# Walkthrough: Automating REST API tests using Postman and Azure DevOps 
## Overview
This walkthrough will take you through creating and automating some REST API tests using Postman and Azure DevOps. For this walkthrough, we'll be making use of HSBC's excellent (and public) branch locator API, which you can find here: https://api.hsbc.com/branch-locator.html.  

We assume you are comfortable creating basic API calls with Postman.    
  
We're going to do the following:
1. Create some API calls  
2. Write some tests against them  
3. Create an Azure DevOps Release pipeline to automate these tests  
4. Run our pipeline!  
## Prerequisites
* You must have installed the latest version of Postman App (https://www.postman.com/downloads/)  
* You must have created a free account with Azure DevOps Services (https://azure.microsoft.com/en-gb/services/devops/)  
* You must be able to access the API above  
## Creating the API calls
(If you don't want to create your own API calls, we've saved our own collection [here](https://github.com/bendalby82/azure-devops-rest-api/blob/master/postman/HSBC.postman_collection.json), which you can just import into Postman)  
1. Create a new Collection in Postman, and set a variable called `hostname` with initial value `api.hsbc.com`  

2. Under that Collection, create some test API calls against the API - you can see my choices in the screenshot below, including my use of that hostname collection variable:  
![Set of tests](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/01-Sketch-Out-Basic-API-Calls.png)   

3. Check your API requests are functioning and green, even though there aren't any tests:  
![Basic requests are green](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/02-Basic-Requests-Are-All-Green.png)  

## Write some tests against them
(Again, you can skip doing any work here by just importing our [completed collection](https://github.com/bendalby82/azure-devops-rest-api/blob/master/postman/HSBC.postman_collection.json))
1. Postman has excellent documentation on how to write tests [here](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/02-Basic-Requests-Are-All-Green.png) - but we want a nice mixture of performance tests, status tests and data tests. Note the use of the `pm.info.requestName` property, which will make our tests easier to read further down the line:
![JavaScript tests](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/03-Write-Some-Test-Cases-With-Data-Check.png)  

2. Make sure all of our requests have at least some tests against them:  
![All requests have tests](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/04-Requests-Now-All-Have-Tests.png)

## Create an Azure DevOps Release pipeline to automate these tests  
We're going to automate our tests using a 'classic' release pipeline. In theory, Microsoft now support YAML pipelines for CD (see [announcement here](https://devblogs.microsoft.com/devops/announcing-general-availability-of-azure-pipelines-yaml-cd/)), but at time of writing this was all very new, and it was not obvious at all how to use these features.
1. Once we've logged into Azure DevOps Services, our first step is to create a new release pipeline under Pipelines > Releases:
![Create new pipeline](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/10-Create-New-Release-Pipeline.png)  

2. Next step is to link the artifact containing your Postman collection and the npm package configuration you'll need to automate that. You're welcome to link to this repository, as shown below, although for some reason Azure wanted me to authenticate, even though the repo was public. Make sure you change the name of the artifact alias to `_Azure-DevOps-REST-API`:
![Link to Ben repo](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/11-Add-GitHub-Artifact.png)  

3. Create a new Stage called 'live', and take advantage of Microsoft's shared agent as shown:  
![Configure agent](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/12-Choose-Ubuntu-And-Azure-Pipelines.png)  

4. Add a new task of type 'npm', and configure as shown below (you can inspect our YAML [here](https://github.com/bendalby82/azure-devops-rest-api/blob/master/azure/YAML-01-NPM-Step.yaml)):  
![Set up npm](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/13-Add-NPM-Command.png)  

5. Add a new task of type 'Command line', and configure as shown below (again, our YAML is [here](https://github.com/bendalby82/azure-devops-rest-api/blob/master/azure/YAML-02-Command-Step.yaml)):  
![Calling newman](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/14-Add-Newman-Command.png)

6. Add a new task of type 'Publish Test Results', and configure as shown below (our YAML is [here](https://github.com/bendalby82/azure-devops-rest-api/blob/master/azure/YAML-03-Publish-Test-Results-Step.yaml)):  
![Publish test results](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/15-Add-Publish-Test-Results-Command.png)

7. Time to save all our work, before testing out our pipeline!  

## Running the pipeline

1. Create a new release from your pipeline in the usual way:  
![Create release](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/20-Create-Release.png)  

2. Check it is green once completed:  
![Green for live](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/21-Release-Is-Green.png)  

3. Click on the stage and then 'Tests' to see a summary:  
![Stage summary](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/22-Looking-At-Run-Summary.png)

4. Clear the filters from the test case pane to see detailed results:  
![Detailed results](https://github.com/bendalby82/azure-devops-rest-api/blob/master/images/23-Looking-At-Detailed-Results.png)  

## Credits
This post builds on work by Alee, which was written up on Medium:  
https://medium.com/younited-tech-blog/integrate-automated-test-in-azure-devops-using-the-postman-api-288f5566bf11
