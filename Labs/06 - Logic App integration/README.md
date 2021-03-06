﻿# 06 - Logic Apps integration
##### Estimated lab time: 25 minutes
In this lab you are going to explore the Security Center integration with Logic Apps.<br>
***Note**: More comprehensive guidance can be found* <a href="https://techcommunity.microsoft.com/t5/Security-Identity/Automate-Azure-Security-Center-actions-with-Playbooks-and/td-p/264843" target="_blank">here</a>.

The most asked automation integration use case is how Security Center integrates with ITSM solutions like ServiceNow, so in this lab we are going to explore exactly that integration.<br><br>

#### 1 - Create an ASC test alert
***Note**: if you want to run more validation steps to simulate attacks in VMs/Computers monitored by Azure Security Center (out of scope for this lab)* <a href="https://gallery.technet.microsoft.com/Azure-Security-Center-549aa7a4" target="_blank">go here</a><br>

Before you are going to create the playbook, let's make sure that we have an ASC alert we can act upon.
1. Login to your **Windows VM** that you have created earlier and is monitored by Security Center (Ask for JIT time slot)
2. Create a temp folder on c: (c:\temp)
3. Copy **calc.exe** from **c:\windows\system32** to **c:\temp**
4. Rename **calc.exe** to **ASC_AlertTest_662jfi039N.exe**
5. Open a command prompt and execute this file as follows (this will generate an ASC test alert):
```dos
ASC_AlertTest_662jfi039N.exe -foo
```

6. Close the calculator window

### ServiceNow integration
:bulb: Skip the following steps if you already have a ServiceNow instance <br>

If you don't want to create an instance of your own and want to leverage a shared environment, make a note of the following:
```txt
Please ask your instructor for the shared ServiceNow URL, username and password
```
<br>

#### 2 - Create a ServiceNow developer instance
1. Navigate to the <a href="https://signon.service-now.com/ssoregister.do?redirectUri=https://developer.servicenow.com" target="_blank">ServiceNow developer website</a> and create a developer instance
2. Go through all the steps until you have a developer instance which is active and running
3. Take a note of the following information, because you will need that in the following steps:
- The URL of your developer instance, similar to
```txt
https://dev12345.service-now.com
```
- admin account name & password <br><br>

#### 3 - Create the ServiceNow playbook


1. Navigate to **Security Center>Playbooks** (under Automation & Orchestration)
2. Click on **Add Playbook**
3. Give your playbook a name like "ASC-Alert-To-ServiceNow", choose your **subscription**, create or choose your **resource group**, select a location and click on **Create**
4. When the playbook has been created, select the playbook. This should bring you to the **Logic Apps Designer** (if that does not happen, click on your playbook and click on **Edit**)
5. Under Templates, select the **Blank Logic App**
6. In the **Serach connectors and triggers field**, type in security center and select **When a response to an Azure Security Center alert is triggered**.
 <br>
**Note**: be aware **Not** to select the connector with the Preview tag.<br><br>

![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/asc_trigger.png
)<br><br>

7. Click on **+ New Step**
8. In the **Seach connectors and triggers field** search for **ServiceNow**
9. Under **Actions**, select **Create Record**<br><br>
![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/ServiceNowConnection.png
)<br><br>


10. Provide a **Connection Name** and fill in the ServiceNow **Instance Name**, **Username** and **Password** that you have captured in the previous steps and click on **Create**
11. In the record type select **Incident** and Click on **Add new parameter**, as shown below:<br><br>


![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/playbook.PNG
)<br><br>

12. Under **Record Type** dropdown box, select **Incident** (this collapses the incident options)<br><br>
13. In the **Record fields**, scroll down, click once in the fields highlighted below and select the values as shown below:<br>
*Note: if you are using a shared ServiceNow environment, put your name in the Short description field as well*<br>


![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/record%20Fields.png
)<br><br>
14. Click on **Save**

#### 4 - Invoke the ServiceNow playbook from an ASC alert
1. Navigate to **Security Center>Security Alerts**
2. Look for the Azure Security Center test alert and click on it<br><br>
![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/test_alert.png
)<br><br>
3. Under Attacked Resource, click on your VM
4. On the bottom of the alert properties, click on **View playbooks**<br>
![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/view_playbooks_button.png)

5. Click on **Run playbook**<br><br>
![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/run_playbook.png
)<br><br>
6. Switch to the playbook history by drill shown on the playbook name and notice the status, but you can also click on the playbook to see details and even runtime information <br><br>
![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/showHistory.png) 
![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/showHistory02.png) <br><br>
![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/playbook_history_details.png)<br><br>



7. Switch to your ServiceNow developer instance and check for a new created incident record<br><br>
![alt text](https://raw.githubusercontent.com/yaniv-shasha/Azure-Security-Center-1/master/Labs/06%20-%20Logic%20App%20integration/Screenshots/snow_record.png)

<br>

### Continue with the next lab
07 - Preview Features, click <a href="https://github.com/yaniv-shasha/Azure-Security-Center-1/tree/master/Labs/07%20-%20Preview%20Features" target="_blank">here</a>

