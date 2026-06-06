# Splunk SIEM Lab for Failed Logins
Splunk Enterprise SIEM Lab showcasing how to build it and what results we pulled from false login attempts 
## SOP: Build a Splunk Enterprise Dashboard for Failed Logins and Account Lockouts

### Objective

Create a Splunk Enterprise lab environment that collects Windows event logs from a forwarder, generates failed login activity, and visualizes the results in a dashboard with alerts. This SOP guides a team member through setting up the infrastructure, configuring log collection, triggering test events, and validating the dashboard output.

### Key Steps

 

**1. Provision the lab environment in Azure** [0:10](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=10)

![generated-image-at-00:00:10](https://loom.com/i/15ade66e14834420a37db7d0a7f30b0a?workflows_screenshot=true)

- Create **two virtual machines** in Azure: 
  - **Linux VM** to host Splunk Enterprise
  - **Windows VM** to generate and forward logs
- Confirm both machines are reachable and properly named for easy identification.
- Plan the Linux VM as the central Splunk server and the Windows VM as the log source.

 

**2. Connect to the Linux VM and install Splunk Enterprise** [0:53](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=53)

![generated-image-at-00:00:53](https://loom.com/i/b8951fe5553644fa8203fa05a1c07a21?workflows_screenshot=true)

- Use **PuTTY** or another SSH client to connect to the Linux VM.
- Download the **Splunk Enterprise Linux package** on the Linux machine.
- Install Splunk Enterprise and complete any required setup commands to start the service.
- Verify that Splunk is installed successfully before moving on.

 

**3. Open required network ports on the Linux VM** [1:12](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=72)

![generated-image-at-00:01:12](https://loom.com/i/1032d6e668874425aa33a758c7875a40?workflows_screenshot=true)

- In Azure, edit the **Network Security Group (NSG)** for the Linux VM.
- Add inbound rules for the following ports: 
  - **Port 8000** for the Splunk web interface
  - **Port 997** for Splunk forwarder communication, as used in this lab
- Save the NSG changes and confirm the rules are active.

 

**4. Install the Splunk Universal Forwarder on the Windows VM** [1:50](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=110)

![generated-image-at-00:01:50](https://loom.com/i/be7f422d7e994fb8978dd6ee1ed767ff?workflows_screenshot=true)

- Download and install the **Splunk Universal Forwarder** on the Windows VM.
- Confirm the forwarder installation completes without errors.
- Ensure the forwarder service is available for configuration.

 

**5. Configure Windows event log collection** [2:00](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=120)

![generated-image-at-00:02:00](https://loom.com/i/23a8afd0d2394f6d88952274157206e9?workflows_screenshot=true)

- Open the forwarder configuration in **VS Code** or a text editor.
- Create or update the **inputs.conf** file.
- Configure the file to collect: 
  - **System logs**
  - **Application logs**
  - **Windows Event Logs**
- Save the configuration and keep the SPL/code references available for the SOP appendix if needed.

 

**6. Restart the Splunk Forwarder to apply the configuration** [2:32](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=152)

![generated-image-at-00:02:32](https://loom.com/i/dda748f1552f440bb7d98d913d6d91d0?workflows_screenshot=true)

- Open **PowerShell** on the Windows VM.
- Restart the Splunk Forwarder service so it loads the new `inputs.conf` settings.
- Confirm the forwarder is running and ready to send logs to Splunk Enterprise.

 

**7. Generate failed login activity on the Windows VM** [2:42](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=162)

![generated-image-at-00:02:42](https://loom.com/i/fe32992f9083400689332d5e3a26cc50?workflows_screenshot=true)

- Open **Windows PowerShell ISE**.
- Run the provided script that generates multiple event IDs and failed login attempts.
- Allow the script to complete so it produces repeated authentication failures and account lockout behavior.
- Verify that the script is creating the intended error messages and login failures.

 

**8. Wait for log ingestion and search Splunk** [3:19](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=199)

![generated-image-at-00:03:19](https://loom.com/i/e4f5cdf0a1e7419f820538062a0f930e?workflows_screenshot=true)

- After running the script, wait approximately **60 seconds** for logs to reach Splunk.
- Open the Splunk dashboard or search interface.
- Confirm that failed login attempts and related events appear in Splunk from the Windows VM source.

 

**9. Build and validate the dashboard visualizations** [3:39](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=219)

![generated-image-at-00:03:39](https://loom.com/i/4d7ddce1380d46f7b2a7876930b44349?workflows_screenshot=true)

- Create dashboard panels to display the collected security events.
- Include the following visualizations: 
  - **Failed logins** as a bar chart
  - **Top processes in the last 24 hours** as an event panel
  - **Logins over time** as a bar chart
  - **After-hours logins** as an event panel
- Verify the data is coming from the correct host, specifically the **Windows Splunk VM**.

 

**10. Create and test an alert for privileged logons** [4:33](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=273)

![generated-image-at-00:04:33](https://loom.com/i/6ed3098e0c6940be8f2054df2aa05d19?workflows_screenshot=true)

- Configure an alert for **high-privileged logon count**.
- Test the alert by reviewing the triggered events.
- Open one of the alert results to confirm it shows the expected activity from the Splunk VM.
- Validate that the alert is tied to the correct data source and event set.

 

**11. Finalize the dashboard and document the SPL logic** [5:09](https://loom.com/share/e583c238f11248c6abad54c0dd0f1713?t=309)

![generated-image-at-00:05:09](https://loom.com/i/54bdefa4b5f04648a242f49dfaab2add?workflows_screenshot=true)

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

### Link to Loom

<https://loom.com/share/e583c238f11248c6abad54c0dd0f1713>
