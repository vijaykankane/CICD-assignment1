# Jenkins CI/CD Pipeline for Flask Application

## 1. Sample Flask Application Structure

Below files are in teh github repository: https://github.com/vijaykankane/CICD-assignment1
### app.py
### requirements.txt
### test_app.py
## 2. Jenkinsfile (WITH VIRTUAL ENVIRONMENT)


## 3. Jenkins Setup Steps

### Install Jenkins:
```bash
# For Ubuntu/Debian
sudo apt update
sudo apt sudo apt install openjdk-21-jdk  python3-full python3-venv -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

### Install Required Plugins: http://63.178.106.225:8080/
1. Go to Jenkins Dashboard → Manage Jenkins → Manage Plugins
2. Install these plugins:
   - Pipeline
   - Git
   - Email Extension Plugin
   - GitHub Integration Plugin

## 4. Configure Jenkins Pipeline

1. **Create New Pipeline Job:**
   - Jenkins Dashboard → New Item → Pipeline → Enter name "flask-ci-cd"

http://63.178.106.225:8080/job/flask-ci-cd/


2. **Configure Pipeline:**
   - Under "Pipeline" section:
     - Definition: Pipeline script from SCM
     - SCM: Git
     - Repository URL: Your forked repo URL
     - Branch: */main

3. **Configure GitHub Webhook:**
   - In GitHub repo: Settings → Webhooks → Add webhook
   - Payload URL: (http://63.178.106.225:8080/github-webhook/)
   - Content type: application/json
   - Events: Push events
   Enable the same at the jenkins and enable the build on webohooks : 
   go to http://63.178.106.225:8080/job/flask-ci-cd/configure
   in triggers enable the below option to make build on trigger from github webhook.
   GitHub hook trigger for GITScm polling




4. **Configure Email Notifications:**
   - Manage Jenkins → Configure System → Email Extension Plugin
   - SMTP server: smtp.gmail.com (for Gmail)
   - Port: 587
   - Username/Password: Your email credentials

able to get the emails when we are going to push anything on this repo.

# Flask CI/CD Pipeline with Jenkins

## Prerequisites
- Jenkins server with Python 3 installed
- Git configured on Jenkins server
- Email SMTP settings configured

## Pipeline Stages
1. **Build**: Creates virtual environment and installs Python dependencies
2. **Test**: Runs pytest unit tests in virtual environment
3. **Deploy**: Deploys app to staging environment using virtual environment

## Setup Instructions
1. Fork this repository
2. Create Jenkins pipeline job
3. Configure webhook for automatic triggering
4. Update email address in Jenkinsfile


## Pipeline Triggers
- Automatic: Push to main branch
- Manual: Build Now in Jenkins


## Notifications
- Email sent on build success/failure
- Check Jenkins console output for details
```

Started by GitHub push by vijaykankane
Obtained Jenkinsfile from git https://github.com/vijaykankane/CICD-assignment1.git
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/flask-ci-cd
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Declarative: Checkout SCM)
[Pipeline] checkout
Selected Git installation does not exist. Using Default
The recommended git tool is: NONE
No credentials specified
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/flask-ci-cd/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/vijaykankane/CICD-assignment1.git # timeout=10
Fetching upstream changes from https://github.com/vijaykankane/CICD-assignment1.git
 > git --version # timeout=10
 > git --version # 'git version 2.43.0'
 > git fetch --tags --force --progress -- https://github.com/vijaykankane/CICD-assignment1.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 945e7742adb9913c1e6da954629d1d3d29d0bec1 (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 945e7742adb9913c1e6da954629d1d3d29d0bec1 # timeout=10
Commit message: "change in app.py updte return"
 > git rev-list --no-walk 87ef21dc3061d26de73aa8ce2e4e6ac9b7a4f9a9 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build)
[Pipeline] echo
Creating virtual environment and installing dependencies...
[Pipeline] sh
+ python3 -m venv venv
+ . venv/bin/activate
+ deactivate nondestructive
+ [ -n  ]
+ [ -n  ]
+ hash -r
+ [ -n  ]
+ unset VIRTUAL_ENV
+ unset VIRTUAL_ENV_PROMPT
+ [ ! nondestructive = nondestructive ]
+ [  = cygwin ]
+ [  = msys ]
+ export VIRTUAL_ENV=/var/lib/jenkins/workspace/flask-ci-cd/venv
+ _OLD_VIRTUAL_PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/snap/bin
+ PATH=/var/lib/jenkins/workspace/flask-ci-cd/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/snap/bin
+ export PATH
+ [ -n  ]
+ [ -z  ]
+ _OLD_VIRTUAL_PS1=$ 
+ PS1=(venv) $ 
+ export PS1
+ VIRTUAL_ENV_PROMPT=(venv) 
+ export VIRTUAL_ENV_PROMPT
+ hash -r
+ pip install -r requirements.txt
Requirement already satisfied: Flask==2.3.3 in ./venv/lib/python3.12/site-packages (from -r requirements.txt (line 1)) (2.3.3)
Requirement already satisfied: pytest==7.4.2 in ./venv/lib/python3.12/site-packages (from -r requirements.txt (line 2)) (7.4.2)
Requirement already satisfied: pytest-flask==1.2.0 in ./venv/lib/python3.12/site-packages (from -r requirements.txt (line 3)) (1.2.0)
Requirement already satisfied: Werkzeug>=2.3.7 in ./venv/lib/python3.12/site-packages (from Flask==2.3.3->-r requirements.txt (line 1)) (3.1.3)
Requirement already satisfied: Jinja2>=3.1.2 in ./venv/lib/python3.12/site-packages (from Flask==2.3.3->-r requirements.txt (line 1)) (3.1.6)
Requirement already satisfied: itsdangerous>=2.1.2 in ./venv/lib/python3.12/site-packages (from Flask==2.3.3->-r requirements.txt (line 1)) (2.2.0)
Requirement already satisfied: click>=8.1.3 in ./venv/lib/python3.12/site-packages (from Flask==2.3.3->-r requirements.txt (line 1)) (8.2.1)
Requirement already satisfied: blinker>=1.6.2 in ./venv/lib/python3.12/site-packages (from Flask==2.3.3->-r requirements.txt (line 1)) (1.9.0)
Requirement already satisfied: iniconfig in ./venv/lib/python3.12/site-packages (from pytest==7.4.2->-r requirements.txt (line 2)) (2.1.0)
Requirement already satisfied: packaging in ./venv/lib/python3.12/site-packages (from pytest==7.4.2->-r requirements.txt (line 2)) (25.0)
Requirement already satisfied: pluggy<2.0,>=0.12 in ./venv/lib/python3.12/site-packages (from pytest==7.4.2->-r requirements.txt (line 2)) (1.6.0)
Requirement already satisfied: MarkupSafe>=2.0 in ./venv/lib/python3.12/site-packages (from Jinja2>=3.1.2->Flask==2.3.3->-r requirements.txt (line 1)) (3.0.2)
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] echo
Running tests...
[Pipeline] sh
+ . venv/bin/activate
+ deactivate nondestructive
+ [ -n  ]
+ [ -n  ]
+ hash -r
+ [ -n  ]
+ unset VIRTUAL_ENV
+ unset VIRTUAL_ENV_PROMPT
+ [ ! nondestructive = nondestructive ]
+ [  = cygwin ]
+ [  = msys ]
+ export VIRTUAL_ENV=/var/lib/jenkins/workspace/flask-ci-cd/venv
+ _OLD_VIRTUAL_PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/snap/bin
+ PATH=/var/lib/jenkins/workspace/flask-ci-cd/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/snap/bin
+ export PATH
+ [ -n  ]
+ [ -z  ]
+ _OLD_VIRTUAL_PS1=$ 
+ PS1=(venv) $ 
+ export PS1
+ VIRTUAL_ENV_PROMPT=(venv) 
+ export VIRTUAL_ENV_PROMPT
+ hash -r
+ python -m pytest test_app.py -v
============================= test session starts ==============================
platform linux -- Python 3.12.3, pytest-7.4.2, pluggy-1.6.0 -- /var/lib/jenkins/workspace/flask-ci-cd/venv/bin/python
cachedir: .pytest_cache
rootdir: /var/lib/jenkins/workspace/flask-ci-cd
plugins: flask-1.2.0
collecting ... collected 2 items

test_app.py::test_hello PASSED                                           [ 50%]
test_app.py::test_health PASSED                                          [100%]

============================== 2 passed in 0.02s ===============================
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Deploy)
[Pipeline] echo
Deploying to staging...
[Pipeline] sh
+ . venv/bin/activate
+ deactivate nondestructive
+ [ -n  ]
+ [ -n  ]
+ hash -r
+ [ -n  ]
+ unset VIRTUAL_ENV
+ unset VIRTUAL_ENV_PROMPT
+ [ ! nondestructive = nondestructive ]
+ [  = cygwin ]
+ [  = msys ]
+ export VIRTUAL_ENV=/var/lib/jenkins/workspace/flask-ci-cd/venv
+ _OLD_VIRTUAL_PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/snap/bin
+ PATH=/var/lib/jenkins/workspace/flask-ci-cd/venv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/snap/bin
+ export PATH
+ [ -n  ]
+ [ -z  ]
+ _OLD_VIRTUAL_PS1=$ 
+ PS1=(venv) $ 
+ export PS1
+ VIRTUAL_ENV_PROMPT=(venv) 
+ export VIRTUAL_ENV_PROMPT
+ hash -r
+ pkill -f python app.py
+ true
+ sleep 5
+ nohup python app.py
+ curl -f http://localhost:5000/health
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed

  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
100    26  100    26    0     0  10668      0 --:--:-- --:--:-- --:--:-- 13000
{
  "status": "healthy"
}
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions)
[Pipeline] emailext
Sending email to: vijay.kankane@gmail.com
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
