# Title: Critical Security Vulnerability in XAMPP for Windows Default Apache Service Configuration

# Table of Contents
- [Description](#description)
- [Issue Overview](#issue-overview)
  - [Impact and Criticality](#impact-and-criticality)
- [Proof of Concept (PoC)](#proof-of-concept-poc)
  - [Description of PoC](#description-of-poc)
  - [Scenario Overview](#scenario-overview)
  - [Detailed Steps Taken](#detailed-steps-taken)
  - [Intent and Outcome](#intent-and-outcome)
  - [Ethical Considerations](#ethical-considerations)
- [Video Demonstration](#video-demonstration)
- [Reproducible Environments](#reproducible-environments)
- [Security Researcher](#security-researcher)
- [Recommended Solution](#recommended-solution)
  - [Create a Dedicated User](#create-a-dedicated-user)
  - [Assign Ownership](#assign-ownership)
  - [Modify Apache Service Configuration](#modify-apache-service-configuration)

## Description:
> A critical security vulnerability has been identified in the default settings of the Apache service configuration within XAMPP on Windows systems. This flaw, discovered by Security Researcher Kaotickj, raises significant security concerns that could potentially compromise the integrity, confidentiality, and availability of Windows systems utilizing XAMPP. This report provides a comprehensive analysis of the vulnerability, its implications, a proof of concept, and a video demonstration, along with essential mitigation measures to protect users' environments.

## Issue Overview:
The core of this vulnerability lies in the default "Log On" configuration for the Apache service, as established during XAMPP's installation via the control panel. By default, the service is configured to run under the local system account, which belongs to the nt authority\system built-in group. This configuration endows the Apache service with unwarranted elevated privileges, increasing the potential impact of any security breaches.

### Impact and Criticality:
The criticality of this vulnerability arises from its potential to facilitate unauthorized access, data breaches, and malicious activities capable of rendering systems inoperable. The local system account possesses extensive permissions, and employing it to run the Apache service magnifies the attack surface, thereby exposing systems to considerable risk.

#### Potential malicious exploitation of this vulnerability could:

- Facilitate unauthorized access to sensitive data, configuration files, and databases hosted by XAMPP.
- Execute arbitrary code within the local system account's context, potentially leading to full system compromise.
- Capitalize on the service's elevated privileges to further infiltrate the network and execute advanced attacks.

## Proof of Concept (PoC):

For the purpose of responsibly demonstrating the severity of the identified vulnerability, a proof of concept (PoC) was developed. This PoC provides a detailed insight into the potential security risks associated with the default configuration of the Apache service within XAMPP on Windows systems. The PoC draws upon well-known and previously reported vulnerabilities, detailing the steps taken while adhering to ethical considerations.

### Description of PoC:

Vulnerability Target: The PoC focuses on the default configuration of the Apache service within XAMPP on Windows systems, where the service is configured to run under the local system account (nt authority\system).

### Scenario Overview:

The PoC leverages well-known and documented vulnerabilities in the Online Learning Management System 1.0 web application, previously reported in public sources such as Exploit Database (EDB-ID: 49326 and EDB-ID: 49365). The PoC demonstrates the combined impact of these vulnerabilities to illustrate how an attacker could exploit the configuration to achieve unauthorized access, privilege escalation, and arbitrary code execution.

### Detailed Steps Taken:

##### a. Exploiting SQL Injection (EDB-ID: 49326):
 Exploited the known SQL injection vulnerability to obtain user account information and plaintext passwords.

##### b. Using Compromised Credentials to Access System (Student Account):

 Logged in to the Online Learning Management System using compromised credentials as a student, leveraging the obtained plaintext password.

##### c. Uploading PHP Command Shell (EDB-ID: 49365):

 Leveraged another known vulnerability to upload The Not So Simple PHP Command Shell (https://github.com/kaotickj/The-Not-So-Simple-PHP-Command-Shell) to the server.
After some brief basic enumeration, found that the service was running with elevated privileges as nt authority\system. 

##### d. Elevating Privileges and Gaining Control:

Used The Not So Simple PHP Command Shell to create a new user, set a password, and elevate the user to administrator.

##### e. Executing Metasploit Payload:

Used The Not So Simple PHP Command Shell to upload and execute an msfvenom windows/meterpreter/reverse_tcp shell (venom.exe) to the system. Because venom.exe was executed as nt authority \ system, the reverse shell is also elevated.

##### f. Migrating to Elevated Process and Persistence:

Migrated to the first process running under nt authority\system found.
Installed persistence via meterpreter shell to the Windows registry for future access.

### Intent and Outcome:

The primary purpose of this PoC is to responsibly illustrate the potential security risks introduced by the default configuration of the Apache service within XAMPP for Windows systems. By detailing the specific steps taken within a controlled environment, the PoC aims to emphasize the potential consequences of the vulnerability.

### Ethical Considerations:

It is imperative to reiterate that this PoC adheres to ethical standards and responsible disclosure practices. The intent is to provide a comprehensive demonstration of the potential impact of the vulnerability, while still avoiding the dissemination of explicit exploit code.

## Video Demonstration:
Accompanying this report is a video demonstration showcasing the process of identifying and exploiting the vulnerability within XAMPP's default configuration. The video provides a visual walkthrough of the steps involved in the PoC, emphasizing the ease with which the vulnerability can be exploited and the potential consequences.

[Proof of Concept Video](https://kdgwebsolutions.com/assets/img/videos/Apache-Service-XAMPP-WindowsPrivEsc-Disclosure.mp4)

## Reproducible Environments:
- XAMPP versions tested: 5.6.3 - 8.2.4
- Operating systems tested: Windows 7, Windows 8.1, Windows 10 (all up-to-date as of 8/14/2023)

## Security Researcher:
This critical security vulnerability was unearthed by Security Researcher Kaotickj. Kaotickj's comprehensive analysis, including the PoC and video demonstration, brought to light the potential risks associated with XAMPP's default configuration. The responsible disclosure of this flaw emphasizes the importance of cooperation between the security community and software developers to bolster the security of digital environments.

## Recommended Solution:
To mitigate this security vulnerability, it is advised to follow these steps:
[Recommended Solution Video](https://kdgwebsolutions.com/assets/img/videos/how-to-fix-xampp-windows-service-running-with-elevated-privilege.mp4)

### Create a Dedicated User:

Create a new user with strong credentials, e.g., "web-manager.":
1. **Open User Accounts:**
   - Open the Start menu and type "Control Panel," then press Enter.
   - In the Control Panel, search for "User Accounts" and click on "User Accounts."

2. **Manage User Accounts:**
   - Click on "Manage another account."

3. **Add a New User:**
   - Click on the "Add a new user in PC settings" link.

4. **Settings App - Family & other users:**
   - Click on "Add someone else to this PC."

5. **Microsoft Account or Local Account:**
   - Select "I don't have this person's sign-in information."

6. **Add a User Without a Microsoft Account:**
   - Click on "Add a user without a Microsoft account."

7. **Create a Username:**
   - Enter the username "web-manager" in the "Username" field.
   - Leave the password fields blank for now.

8. **Finish Creating Account:**
   - Click on "Next."

9. **Set Password for the New User:**
   - Back in "Manage Accounts," click on the "web-manager" account.
   - Click on "Change the password."

10. **Enter and Confirm Password:**
    - Enter a strong password in the "New password" and "Reenter password" fields.
    - You can also add a password hint.

11. **Change Password:**
    - Click on "Change password."

### Assign Ownership:
- Open File Explorer:
Open the File Explorer by pressing the Win + E keyboard shortcut or by clicking the File Explorer icon on your taskbar.

- Navigate to XAMPP Directory:
Navigate to the directory where XAMPP is installed. This is typically located in the C:\xampp folder. Adjust the path if you installed XAMPP in a different location.
- Right-Click the XAMPP Directory:
Right-click on the XAMPP directory to bring up the context menu.
- Select "Properties":
From the context menu, select "Properties" at the bottom. This will open the Properties window for the XAMPP directory.
- Go to "Security" Tab:
In the Properties window, go to the "Security" tab.
- Click "Advanced":
In the Security tab, click the "Advanced" button at the bottom.
- Click "Change" next to Owner:
In the Advanced Security Settings window, next to "Owner," you'll see the current owner of the directory. Click the "Change" link next to it.
- Enter "web-manager" and Check Names:
In the Select User or Group window, click the "Advanced" button. In the subsequent window, click the "Find Now" button. Scroll through the list and select the "web-manager" user if you see it. If not, type "web-manager" into the "Enter the object name to select" field and click "Check Names." If the name is recognized, it will be underlined.
- Click "OK":
After confirming the correct user, click the "OK" button on each window to close them.
- Back in Advanced Security Settings:
Back in the Advanced Security Settings window, you should now see "web-manager" as the owner. Check the box that says "Replace owner on subcontainers and objects."
- Click "Apply" and "OK":
Click "Apply" and then "OK" in the Advanced Security Settings window.
- Confirm Permissions:
Back in the Properties window, make sure that "web-manager" now appears in the list of users and groups with permissions. If not, click "Edit" to modify permissions.
- Adjust Permissions (If Necessary):
In the Permissions window, adjust permissions as needed to ensure that "web-manager" has the appropriate access. This might include Full Control or Modify permissions, depending on your requirements.
- Apply Changes:
Apply the changes to both the Permissions window and the Properties window.
- Close Windows:
Close all open windows to save the changes.

By following these steps, you will have assigned ownership of the XAMPP installation directory to the "web-manager" user, ensuring that the user has proper control and access to the directory. Please note that these instructions may vary slightly depending on your Windows version.
### Modify Apache Service Configuration:
- Press Win + R, type services.msc, and press Enter.
- Locate the "Apache2" service in the list.
- Right-click the service and select "Properties."
- In the "Log On" tab, select "This account."
- Click "Browse" to find the "web-manager" user, enter its credentials, and confirm.
- Click OK to apply the changes.
- Restart the Apache2 service by right-clicking it and selecting "Restart."

Additional Notes:
It's important to emphasize that this security vulnerability is specific to XAMPP instances running on Windows. On Linux systems, the Apache service adheres to Linux's least privilege standards, making this issue Windows-specific.

It's crucial to emphasize the severity of this flaw, which exposes XAMPP installations on Windows systems to significant security risks. By following the recommended steps to create a dedicated user, assign ownership, and modify the Apache service configuration, users can substantially mitigate the security risks associated with the default configuration.
