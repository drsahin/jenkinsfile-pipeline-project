# jenkinsfile-pipeline-project
## Integrating Jenkins Pipeline with GitHub Webhook

- Go to your Github ``jenkinsfile-pipeline-project`` repository page and click on `Settings`.

- Click on the `Webhooks` on the left hand menu, and then click on `Add webhook`.

- Copy the Jenkins URL from the AWS Management Console, paste it into `Payload URL` field, add `/github-webhook/` at the end of URL, and click on `Add webhook`.

```text
http://ec2-54-144-151-76.compute-1.amazonaws.com:8080/github-webhook/
```

- Go to the Jenkins dashboard and click on `New Item` to create a pipeline.

- Enter `pipeline-with-jenkinsfile-and-webhook` then select `Pipeline` and click `OK`.

- Enter `Simple pipeline configured with Jenkinsfile and GitHub Webhook` in the description field.

- Put a checkmark on `GitHub Project` under `General` section, enter URL of the project repository.

```text
https://github.com/<your-github-account-name>/jenkinsfile-pipeline-project/
```

- Put a checkmark on `GitHub hook trigger for GITScm polling` under `Build Triggers` section.

- Go to the `Pipeline` section, and select `Pipeline script from SCM` in the `Definition` field.

- Select `Git` in the `SCM` field.

- Enter URL of the project repository, and let others be default.

```text
https://github.com/<your-github-account-name>/jenkinsfile-pipeline-project.git
```

- Click `apply` and `save`. Note that the script `Jenkinsfile` should be placed under root folder of repo.

- Go to the Jenkins project page and click `Build Now`.The job has to be executed manually one time in order for the push trigger and the git repo to be registered.

- Now, to trigger an automated build on Jenkins Server, we need to change any file it the repo, then commit and push the change into the GitHub repository. So, update the `Jenkinsfile`on your local repository with the following pipeline script.

```groovy
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo 'Clarusway_Way to Reinvent Yourself'
                sh 'echo Integrating Jenkins Pipeline with GitHub Webhook using Jenkinsfile'
            }
        }
    }
}
```

- Commit and push the change to the remote repo on GitHub.

```bash
git add .
git commit -m 'updated Jenkinsfile'
git push
```

- Observe the new built triggered with `git push` command under `Build History` on the Jenkins project page.

- Explain the built results, and show the `Integrating Jenkins Pipeline with GitHub Webhook using Jenkinsfile` output from the shell.

- Explain the role of `Jenkinsfile` and GitHub Webhook in this automation.



##- Configuring Jenkins Pipeline with GitHub Webhook to Run the Python Code

- To build the `python` code with Jenkins pipeline using the `Jenkinsfile` and `GitHub Webhook`, we will leverage from the same job created in Part 2 (named as `pipeline-with-jenkinsfile-and-webhook`). 

- To accomplish this task, we need;

  - a python code to build

  - a python environment to run the pipeline stages on the python code

  - a Jenkinsfile configured for an automated build on our repo


- Create a python file on the `jenkinsfile-pipeline-project` local repository, name it as `pipeline.py`, add coding to print `My first python job which is run within Jenkinsfile.` and save.

```python
print('My first python job which is run within Jenkinsfile.')
```

- Update the `Jenkinsfile` with the following pipeline script, and explain the changes.

```groovy
pipeline {
    agent any
    stages {
        stage('run') {
            steps {
                echo 'Clarusway_Way to Reinvent Yourself'
                sh 'python --version'
                sh 'python pipeline.py'
            }
        }
    }
}
```


- Commit and push the changes to the remote repo on GitHub.

```bash
git add .
git commit -m 'updated jenkinsfile and added pipeline.py'
git push
```

- Observe the new built triggered with `git push` command on the Jenkins project page.




##- Creating a Pipeline with Poll SCM

- Go to the Jenkins dashboard and click on `New Item` to create a pipeline.

- Enter `jenkinsfile-pipeline-pollSCM` then select `Pipeline` and click `OK`.

- Enter `This is a pipeline project with pollSCM` in the description field.

- We will use same github repo project in Part 2 (named as `jenkinsfile-pipeline-project`).

- Go to the `Pipeline` section.
  - for definition, select `Pipeline script from SCM`
  - for SCM, select `Git`
    - for `Repository URL`, select `https://github.com/<your-github-account-name>/jenkinsfile-pipeline-project/`, show the `Jenkinsfile` here.
    
    pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo 'Compiling the java source code...'
                sh 'javac Hello.java'
            }
        }
        stage('run') {
            steps {
                echo 'Running the compiled java code.'
                sh 'java Hello'
            }
        }
    }
}


    - approve that the `Script Path` is `Jenkinsfile`
- `Save` and `Build Now` and observe the behavior.

- Go to the the `Configure` and skip to the `Build Triggers` section
  - Select Poll SCM, and enter `* * * * *` (5 stars)
- `Save` the configuration.

- Go to the GitHub repo and modify some part in the `Jenkinsfile` and commit.

- Observe the auto build action at Jenkins job.
