# Bamboo Integration

## 1. Setup Bamboo

### Installation

Download Bamboo from https://www.atlassian.com/software/bamboo. Follow their [installation guide](https://confluence.atlassian.com/bamboo/getting-started-with-bamboo-289277283.html?_ga=2.95849887.246880307.1531709232-1995250601.1528082340). 

### Create a project and link to GitHub

1) Login to Bamboo. In the top navigation bar, hover over the 'Create' dropdown menu and select 'Create Project'. 

![create-project](assets/create-project.png)

2) Fill out the information about the project. 

![config-proj](assets/config-proj.png)

3) Create a new plan for your project. 

![create-plan](assets/create-plan.png)

4) Fill out the information about the plan and link your GitHub repository. Click on 'Configure plan' when finished. 

![configure-plan](assets/configure-plan.png)

> Note: If you link to a GitHub repository and you use two-factor authentication, you must generate a personal access token in GitHub to use as the password

<!-- ## 2. Auto build in Bamboo

To deploy to HockeyApp from Bamboo, follow [this guide](https://www.atlassian.com/blog/archives/continuous-deployment-mobile-apps).

Once your app has been built, download the latest build so we can use Kobiton to test on it.  -->