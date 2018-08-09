# Bamboo Integration

## Table of contents
- [Prerequisites](#prerequisites)
- [Create a project](#Create-a-project-and-link-to-GitHub)

### Prerequisites

* Bamboo is installed

    > * If not installed, follow their [installation guide](https://confluence.atlassian.com/bamboo/getting-started-with-bamboo-289277283.html?_ga=2.95849887.246880307.1531709232-1995250601.1528082340) to download and setup Bamboo

### Create a project and link to GitHub

_Follow the steps below to setup a Bamboo plan for GitHub._

In your Bamboo project, create a new plan. 
> If you need more information on creating Bamboo projects, you may reference [this guideline](https://www.360logica.com/blog/how-to-create-and-run-your-first-project-in-bamboo/).

![create-plan](assets/create-plan.png)

Fill out the information about the plan. Under `Link repository to new build plan`, add your GitHub repository. Click on 'Configure plan' when finished. 

![configure-plan](assets/configure-plan.png)

Now that you have created a project and plan, you can now start configuring tasks. In the [next guideline](running-automation-testing.md), we will configure tasks that will allow us to run an automation test on your project with Kobiton. 

