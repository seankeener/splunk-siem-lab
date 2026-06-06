### Watch me do it here

<https://loom.com/share/e583c238f11248c6abad54c0dd0f1713>

### SIEM Lab SPL/Scripts/Code

https://docs.google.com/document/d/1o4ExvPUToo2WYkTBcRkDA-HyG7bR7nGGfpB_adpi1Ac/edit?usp=sharing


# Splunk SIEM Lab for Failed Logins
Splunk Enterprise SIEM Lab showcasing how to build it and what results we pulled from false login attempts 
## SOP: Build a Splunk Enterprise Dashboard for Failed Logins and Account Lockouts

### Objective

Create a Splunk Enterprise lab environment that collects Windows event logs from a forwarder, generates failed login activity, and visualizes the results in a dashboard with alerts. This SOP guides a team member through setting up the infrastructure, configuring log collection, triggering test events, and validating the dashboard output.

### Key Steps

 

**1. Provision the lab environment in Azure** [0:10](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=10)
<img width="1353" height="360" alt="image" src="https://github.com/user-attachments/assets/b7033880-385f-4518-aa95-b9e5168a9004" />


- Create **two virtual machines** in Azure: 
  - **Linux VM** to host Splunk Enterprise
  - **Windows VM** to generate and forward logs
- Confirm both machines are reachable and properly named for easy identification.
- Plan the Linux VM as the central Splunk server and the Windows VM as the log source.

 

**2. Connect to the Linux VM and install Splunk Enterprise** [0:53](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=53)

<img width="1067" height="1341" alt="image" src="https://github.com/user-attachments/assets/4ed8191b-2aba-47be-b348-b73427d1f471" />


- Use **PuTTY** or another SSH client to connect to the Linux VM.
- Download the **Splunk Enterprise Linux package** on the Linux machine.
- Install Splunk Enterprise and complete any required setup commands to start the service.
- Verify that Splunk is installed successfully before moving on.

 

**3. Open required network ports on the Linux VM** [1:12](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=72)

<img width="1327" height="417" alt="image" src="https://github.com/user-attachments/assets/eeee00a1-a32c-4ab2-8659-ef88235ebdc7" />


- In Azure, edit the **Network Security Group (NSG)** for the Linux VM.
- Add inbound rules for the following ports: 
  - **Port 8000** for the Splunk web interface
  - **Port 9997** for Splunk forwarder communication, as used in this lab
- Save the NSG changes and confirm the rules are active.

 

**4. Install the Splunk Universal Forwarder on the Windows VM** [1:50](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=110)

<img width="1123" height="647" alt="image" src="https://github.com/user-attachments/assets/96bae547-5724-4372-b5b8-a32790c4bd2b" />


- Download and install the **Splunk Universal Forwarder** on the Windows VM.
- Confirm the forwarder installation completes without errors.
- Ensure the forwarder service is available for configuration.

 

**5. Configure Windows event log collection** [2:00](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=120)

<img width="752" height="404" alt="image" src="https://github.com/user-attachments/assets/dd3850c2-5b50-425e-9a82-458ff8d37180" />


- Open the forwarder configuration in **VS Code** or a text editor.
- Create or update the **inputs.conf** file.
- Configure the file to collect: 
  - **System logs**
  - **Application logs**
  - **Windows Event Logs**
- Save the configuration and keep the SPL/code references available for the SOP appendix if needed.

 

**6. Restart the Splunk Forwarder to apply the configuration** [2:32](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=152)

<img width="1107" height="608" alt="image" src="https://github.com/user-attachments/assets/1f26099a-3a19-43df-a235-78ff481bd0a0" />


- Open **PowerShell** on the Windows VM.
- Restart the Splunk Forwarder service so it loads the new `inputs.conf` settings.
- Confirm the forwarder is running and ready to send logs to Splunk Enterprise.

 

**7. Generate failed login activity on the Windows VM** [2:42](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=162)

<img width="926" height="499" alt="image" src="https://github.com/user-attachments/assets/f2b3a0c4-157f-458c-a42b-95668ac0c525" />



- Open **Windows PowerShell ISE**.
- Run the provided script that generates multiple event IDs and failed login attempts.
- Allow the script to complete so it produces repeated authentication failures and account lockout behavior.
- Verify that the script is creating the intended error messages and login failures.

 

**8. Wait for log ingestion and search Splunk** [3:19](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=199)

<img width="1656" height="1061" alt="image" src="https://github.com/user-attachments/assets/1cdf931f-220d-40ef-a4db-105829c84d3f" />


- After running the script, wait approximately **60 seconds** for logs to reach Splunk.
- Open the Splunk dashboard or search interface.
- Confirm that failed login attempts and related events appear in Splunk from the Windows VM source.

 

**9. Build and validate the dashboard visualizations** [3:39](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=219)

<img width="2511" height="954" alt="image" src="https://github.com/user-attachments/assets/beb1c85c-e41a-4e85-b48d-fb20ced757fa" />


- Create dashboard panels to display the collected security events.
- Include the following visualizations: 
  - **Failed logins** as a bar chart
  - **Top processes in the last 24 hours** as an event panel
  - **Logins over time** as a bar chart
  - **After-hours logins** as an event panel
- Verify the data is coming from the correct host, specifically the **Windows Splunk VM**.

 

**10. Create and test an alert for privileged logons** [4:33](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=273)

<img width="2175" height="701" alt="image" src="https://github.com/user-attachments/assets/58209df3-04c0-4402-8d27-74ee401d2f66" />


- Configure an alert for **high-privileged logon count**.
- Test the alert by reviewing the triggered events.
- Open one of the alert results to confirm it shows the expected activity from the Splunk VM.
- Validate that the alert is tied to the correct data source and event set.

 

**11. Finalize the dashboard and document the SPL logic** [5:09](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=309)


- Add the SPL queries and trigger logic used to build the dashboard into the SOP appendix or supporting documentation.
- Confirm the dashboard clearly shows: 
  - Failed logins
  - Account lockouts
  - Login activity over time
- Record the approximate setup time if useful for planning future labs or deployments.

### Cautionary Notes

- Ensure Azure NSG rules are restricted to only the ports required for the lab.
- Confirm the Splunk forwarder is pointing to the correct Splunk Enterprise host before testing.
- The failed-login script may lock the account quickly; use a test account only.
- Wait for log ingestion before troubleshooting dashboard data, as events may take time to appear.
- If the dashboard shows no data, verify the forwarder service, inputs.conf, and network connectivity first.

### Tips for Efficiency

- Prepare the SPL queries and dashboard panel definitions before generating test events.
- Keep the Windows test script and forwarder configuration in a shared lab folder for reuse.
- Use clear hostnames for the Linux and Windows VMs to simplify validation.
- Test connectivity to port 8000 early so you can access the Splunk web UI without delays.
- Reuse the same lab setup for future detections by saving the dashboard and alert configurations.
