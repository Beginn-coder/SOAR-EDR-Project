# SOAR-EDR-Project

## Objective
The objective of the SOAR-EDR project is to establish a robust Security Orchestration, Automation, and Response (SOAR) system integrated with Endpoint Detection and Response (EDR) using Tines, LimaCharlie, Slack and Gmail. This project aims to automate incident response workflows, enhance cybersecurity defenses, and validate detection rules through comprehensive testing and integration.

### Skills Learned

- Endpoint Detection and Response (EDR) configuration.
- SOAR playbook design and automation.
- Event monitoring and rule-based threat detection.
- Incident response using automated workflows.

### Playbook Workflow
1. Detection Retrieval: A LimaCharlie webhook detects a suspecious event
2. Alerting: Details such as Title, Username, source IP are sent to Slack and Email
3. Decision Prompt: Admins receive a Slack prompt asking if the affected machine should be isolated. A NO response will send a Slack Message indicating no action taken. A YES response will execute LimaCharlie isolation and send the status update to Slack

## Step 1
### Configuring the Windows Server
For Step 1, I used the Vultr Cloud Services platform to host the Windows Server 2022 target system. The configuration is shown in the screenshot below. 
![image](https://github.com/user-attachments/assets/a2c36d8d-7638-425d-8f97-a92ff3c48c16) 
Once the system is setup, it can take a while to configure so you'll want to head over to LimaCharlie.io, create an account, and set up an org along the lines of SOAR-EDR or something similar to that as you wait.  
In LimaCharlie, you'll see a list of available sensors. Head over to Installation Keys, select create installation key and give it a name. Once done, delete the other keys. Now, scroll down under your newly created key to sensor downloads and right-click on Windows 64-bit EDR to copy the link address. Head back to the server and paste the link into a browser, it will automatically download. 
![image](https://github.com/user-attachments/assets/1e1279fe-ceb9-4fd2-b602-778188af5bc1)
Copy the sensor key as well and open powershell as admin in the server, navigate to the downloads folder and type in .\[the downloaded file] -i [INSTALLATION_KEY] and press enter. Within a few seconds, the service will be installed. In Services, you should see LimaCharlie running. In LimaCharlie, under the Sensor list, you should see your sensor running. 

## Step 2
### Configuring LaZagne
In a browser on your server, search for https://github.com/AlessandroZ/LaZagne and click on releases in the github page. Disable the firewall under Real-Time Protection in Windows Security. Then click on the 3 dots in the blocked file and select 'keep'. Open your downloads file and right-click on Lazagne.exe, select 'open powershell window here'. This should auto-run the file.
In LimaCharlie, under the timeline tab in your sensor, if you search for LaZagne you should see something similar to the below image
![image](https://github.com/user-attachments/assets/8a288f0e-0340-4ae2-b4f0-9f413993dc69)
Scroll down to Automation and under D&R rules, select New Rule. Your rule will look somehting like the following:
![image](https://github.com/user-attachments/assets/fe7ed739-f0a2-4e8f-9f8f-951e65463297)


In the respond section, it'll look like this:
![image](https://github.com/user-attachments/assets/b1366adc-5ec3-41fd-a057-52fba8b24cc4)

Now save the rule. In order to test the rule, click on Target Event and paste the event from earlier. Scroll down and click on Test Event. You should see all green after running the test. 

## Step 3
### Setting up Slack and Tines
On your desktop, create an account with Slack using a valid email account and create a workspace. Give the workspace a name and run the through the process and make the channel public. In a separate tab, create a tines account with a valid email. 

The workflow will look something like this: ![image](https://github.com/user-attachments/assets/5ca190b3-802d-4952-a220-6a5630f3ba5b)
The user prompt will have the following: ![image](https://github.com/user-attachments/assets/5502add4-bf18-4542-b663-affa1426cabe)

If done correctly, you should receive slack messages similar to the below images. 

Non-isolated computer: ![image](https://github.com/user-attachments/assets/e4575410-0b9e-4baa-b232-c581976b95f0)


Isolated computer: ![image](https://github.com/user-attachments/assets/e40ab45e-fcd3-4a1b-8db7-fded8594eb94) 


## Issues Encountered
I did encounter a major issue with this project when configuring the workflow. For some reason, I couldn't get the LimaCharlie Sensor to work with Tines as I kept getting HTTP 400 response codes. It appears that when I copied the credential over to Tines, it would revert back to something else after a period of time. I'm not sure how I was able to solve it but I was able to get it to work for a short period of time. 

## Acknowledgement
I want to give a big thanks to MyDFIR as the creator of this project. His SOAR-EDR project guide was invaluable in gaining practical experience on how to automate alerts. Check his channel out at https://www.youtube.com/@MyDFIR for more tutorials.
