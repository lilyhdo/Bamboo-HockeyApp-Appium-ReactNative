# Running automation test with Kobiton

## 1. Prepare Kobiton configuration for executing automation testing

* #### Username 
1. Go to https://portal.kobiton.com/
2. In the upper right hand corner, click on your name and in the drop down menu, click on Profile. 

![profile](assets/profile-user.png)

3. You should see the username. 

![username](assets/username.png)

* #### API key
1. Click on your name in the upper righthand corner again and select settings. 
2. You should be able to find your API key under 'API Keys'. 

![api-key](assets/apikey.png)

* #### Desired caps
1. In the navigation bar at the top of the Kobiton website, select Devices. 
2. Hover over any device and click on the Automation settings button (the gear symbol). 

![automation](assets/automation.png)

3. On the left hand side, you can select your preferred language, as well as any other variables you would like to adjust, such as **App Type**, **Device Group**, and **Orientation**. Adjusting the settings on the left side will affect the desiredCaps, which you can find in the right side of the window. 

![automation-settings](assets/automationsettings.png)

## 2. Write the automation test script

For examples of automation tests, go to https://github.com/kobiton/samples . 

Choose a language for your test script, and decide whether you want to test on Android or iOS, and either do a web test or an app test. Make sure in the code you specify your Kobiton username, API key, and information under desiredCaps. 

## 3. Setup Bamboo

### Installation

Download Bamboo from https://www.atlassian.com/software/bamboo. Follow their [installion guide](https://confluence.atlassian.com/bamboo/getting-started-with-bamboo-289277283.html?_ga=2.95849887.246880307.1531709232-1995250601.1528082340). 

### Create a project

1) Login to Bamboo. In the top navigation bar, hover over the 'Create' dropdown menu and select 'Create Project'. 

![create-project](assets/create-project.png)

2) Fill out the information about the project. 

![config-proj](assets/config-proj.png)

3) Create a new plan for your project. 

![create-plan](assets/create-plan.png)

4) Fill out the information about the plan. and click on 'Configure plan' when finished. 

![configure-plan](assets/configure-plan.png)

> Note: If you link to a GitHub repository and you use two-factor authentication, you must generate a personal access token in GitHub to use as the password

### Create task to run automation test

#### First task: npm install

Now that we have created the plan, we must now configure a job. 

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

Click on 'Create' to start your first build. 

If your build is a failure, you can check the "Logs" tab. 

![failure](assets/failure.png)

To see further details on the log, click on 'Default Job'. 

![default-job](assets/default-job.png)

To edit your plan, you can click on it near the top of the screen, next to where it says 'Build dashboard'. 

![go-to-job](assets/go-to-job.png)

Then click on the edit pencil icon on the right side of the plan you wish to edit. 

![edit-plan](assets/edit-plan.png)

Now you can select any job you want to edit. 

![select-job](assets/select-job.png)

Once you are done editing, you can click on the 'Run' button near the top right corner of the screen. 

![run-plan](assets/run-plan.png)

If your build was successful, check Kobiton cloud devices to see if a test session was created. 

## Get the automation session data through Kobiton REST API

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
curl -X GET https://api.kobiton.com/v1/sessions \
  -H 'Authorization: Basic dGVzdHVzZXI6MTIzZWQtMTIzZmFjLTkxMzdkY2E='
  -H 'Accept: application/json'
```

Below are necessary Kobiton Rest API endpoints that you may need.

- Get session info
```
GET /sessions{sessionId}
```
To make a request:
```
curl -X GET https://api.kobiton.com/v1/sessions/{sessionId} \
  -H 'Authorization: Basic dGVzdHVzZXI6MTIzZWQtMTIzZmFjLTkxMzdkY2E='
  -H 'Accept: application/json'
```

With this line of code, you can print the session information to the console. 
```javascript
console.log(sessionCapabilities)
```

- Get session commands
```
GET /sessions/{sessionId}/commands
```
To make a request:

```Shell
curl -X GET https://api.kobiton.com/v1/sessions/{sessionId}/commands \
  -H 'Authorization: Basic dGVzdHVzZXI6MTIzZWQtMTIzZmFjLTkxMzdkY2E='
  -H 'Accept: application/json'

```
To get to a certain page in your commands, add the page number to the commands URL. For example:
```javascript
`https://api-test.kobiton.com/v1/sessions/${sessionId}/commands?page=2`
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