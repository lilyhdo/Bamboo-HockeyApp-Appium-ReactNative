# Running automation test with Kobiton

## Table of contents
- [1. Prepare Kobiton configuration for executing automation testing](#1-prepare-kobiton-configuration-for-executing-automation-testing)
    - [Username](#username)
    - [API key](#api-key)
    - [Desired caps](#desired-caps)
- [2. Write the automation test script](#2-write-the-automation-test-script)
- [3. Run automation script on Bamboo](#3-run-automation-script-on-bamboo)
    - [Create task to run automation test](#create-task-to-run-automation-test)
      - [First task: npm install](#first-task-npm-install)
      - [Second task: run automation script](#second-task-run-automation-script)
    - [Build](#build)
    - [Troubleshooting](#troubleshooting)
- [4. Get the automation session data through Kobiton REST API](#4-get-the-automation-session-data-through-kobiton-rest-api)
    - [Authorization](#authorization)
    - [Get session info](#get-session-info)
    - [Commands](#commands)
    - [Final Result](#final-Result)

## 1. Prepare Kobiton configuration for executing automation testing

#### Username
1. Go to https://portal.kobiton.com/
2. In the upper right hand corner, click on your name and in the drop down menu, click on Profile. 

![profile](assets/profile-user.png)

3. You should see the username. 

![username](assets/username.png)

#### API key
1. Click on your name in the upper righthand corner again and select settings. 
2. You should be able to find your API key under 'API Keys'. 

![api-key](assets/apikey.png)

#### Desired caps
1. In the navigation bar at the top of the Kobiton website, select Devices. 

![devices](assets/devices.png)

2. Hover over any device and click on the Automation settings button (the gear symbol). 

![automation-gear](assets/automation-gear.png)

3. On the left hand side, you can select your preferred language, as well as any other variables you would like to adjust, such as **App Type**, **Device Group**, and **Orientation**. 

> Note: Adjusting the settings on the left side will affect the desiredCaps, which you can find in the right side of the window. 

![automation-settings](assets/automationsettings.png)

## 2. Write the automation test script

For examples of automation tests, go to https://github.com/kobiton/samples . 

Choose a language for your test script, and decide whether you want to test on Android or iOS, and either do a web test or an app test. Make sure in the code you specify your Kobiton username, API key, and information under desiredCaps. 

In the below Node.JS script example, you can see the indicated fields and replace the information with your own. 

![sample](assets/sample.png)

## 3. Run automation script on Bamboo

### Create task to run automation test

#### First task: npm install

On your project plan, configure a new job. 

![config-job](assets/config-job.png)

Click on 'Add task'. 

In this guideline, we will be running the NodeJS test script, so we need to select the task type 'npm'. 

![npm-task](assets/npm-task.png)

In the 'Node.js executable' section, make sure the executable is pointing to the directory that has your node version. You can do this by clicking 'Add a new executable'. 

![new-executable](assets/new-executable.png)

For the command, put 'install'. 

Click 'Save'. 

![npm-install](assets/npm-install.png)

#### Second task: run automation script

Add another task of type 'npm'. 

In the 'Node.js executable' section, make sure the executable is the same as the 'npm install' task we created previously. 

For the command, run your automation script. 

If necessary, you can click on 'Advanced options' and provide environment variables. 

![run-task](assets/run-task.png)

Click 'Save'. 

### Build

Once you are done editing, click on 'Create' to start your first build. 

![create](assets/create.png)

Your build will now run. 

![building](assets/building.png)

To edit your plan after a build, you can click on your job near the top of the screen, next to where it says 'Build dashboard' to return to your job configurations. 

![edit-plan](assets/go-job.png)

### Troubleshooting

If your build is a failure, you can check the "Logs" tab. To see further details on the log, click on 'Default Job'. 

![default-job](assets/defaultjob.png)

If your build was successful, check Kobiton cloud devices to see if a test session was created. 

## 4. Get the automation session data through Kobiton REST API

- Update test result to session (https://api.kobiton.com/docs/#update-session-information)

```
PUT /sessions/{sessionId}
``` 
To make a request:

1. Encode your credential in base64 for HTTP Basic Authentication, you may use below command and note the text result

```
echo -n <your username>:<your api-key> | base64
```
2. Use encoded base64 from above in curl commands, like below

```
curl -X GET https://api.kobiton.com/v1/sessions 
  -H 'Authorization: Basic dGVzdHVzZXI6MTIzZWQtMTIzZmFjLTkxMzdkY2E='
  -H 'Accept: application/json'
```

Below are necessary Kobiton Rest API endpoints that you may need.

### Authorization

To make a request:
```
curl -X GET https://api.kobiton.com/v1/sessions/{sessionId}
  -H 'Authorization: Basic dGVzdHVzZXI6MTIzZWQtMTIzZmFjLTkxMzdkY2E='
  -H 'Accept: application/json'
```

### Get session info
```
GET /sessions{sessionId}
```

To make a request: 
```javascript
curl -X GET https://api.kobiton.com/v1/sessions/{sessionId} 
  -H 'Authorization: Basic dGVzdHVzZXI6MTIzZWQtMTIzZmFjLTkxMzdkY2E='
  -H 'Accept: application/json'
```

The output should look something like below:

![session-information](assets/session-information.png)

### Commands
```
GET /sessions/{sessionId}/commands
```
To make a request:

```Shell
curl -X GET https://api.kobiton.com/v1/sessions/{sessionId}/commands
  -H 'Authorization: Basic dGVzdHVzZXI6MTIzZWQtMTIzZmFjLTkxMzdkY2E='
  -H 'Accept: application/json'

```
To get to a certain page in your commands, add the page number to the commands URL. For example:
```shell
https://api.kobiton.com/v1/sessions/${sessionId}/commands?page=2
```

> For more details on how to retrieve information about your session, go to https://api.kobiton.com/docs/

## Final result

The test is a either a success or failure.

#### Failure case

* Error: "The environment you requested was unavailable." 
    - This means that the device you selected is already booked. Either select a different device or wait a few moments until your device becomes available
* Other 
    - Contact Kobiton for support
    - Go to portal.kobiton.com
    - In the navigation bar at the top of the page, click on 'Support'

    ![support](assets/support.png)

    - Fill in the information for your request and submit your ticket

    ![submit-ticket](assets/submit-ticket.png)