# Identity and Access Management: Configuring and Analyzing Share Permissions
<h3>Objectives</h3>

- Apply Security Solutions for Infrastructure Management
- Analyze and Configure Appropriate Share Permissions
#
**Scenario:**
515support is in the process of being audited. To comply with the audit process, it is necessary to create temporary user and computer accounts for the audit staff. Each account exists as an object in the company's Active Directory domain, where they are given certain attributes and permissions according to the requirements of the project. Another technician has developed a script to provision the accounts automatically. You have been asked to execute the script and validate the results. You have tested that the portion of the script that creates accounts is working as intended.

This is a continuation of part 1 of this IAM lab: Performing Account and Permissions Audits. I still need to validate that the sec-glo-audit security group has read-only access to the \\MS1\AUDIT network share, mapped to the C:\LABFILES folder on the MS1 VM.
#
<h3>Configuring and Analyzing Share Permissions</h3>

To set up the lab I just need to execute this script

![image](https://github.com/user-attachments/assets/c4c7cbd8-98ea-430a-8be5-bd974285ff1d)

Now on Server Manager, I go to Manage > Add Servers, and type in ms1 in the Name (CN) field, then click Find Now. This confirms that MS1 is up.

![image](https://github.com/user-attachments/assets/d1c058b8-dd84-4037-a3bd-1036b8fb21c3)

Now to set read-only access to the audit network share on ms1

Back on Server Manager, I'll go to File and Storage Services > Shares > Audit and right-click > Properties

![image](https://github.com/user-attachments/assets/5678800e-119b-43ae-8b34-2e49b8ed01d1)

Now I'll click on Permissions > Custom Permissions. At the top, we see that sec-glo-audit has been given Full Control instead of read-only access.

![image](https://github.com/user-attachments/assets/ee2170b5-d97f-4872-9d88-adb680f89a5f)
![image](https://github.com/user-attachments/assets/7f718776-f203-46fc-a111-5be49b84add2)
![image](https://github.com/user-attachments/assets/33ca360d-58c5-4506-8c4b-87d400bbcaa3)


- sec-glo-audit has been granted Full Control using NTFS permissions. These apply locally. NTFS permissions can propagate to child objects and containers (like in this case) or they can be set separately.

Clicking the Share tab, there is only one entry which allows only authenticated users full control.
-  This permission applies when accessing the share over the network only.
![image](https://github.com/user-attachments/assets/6e44d2ab-eaac-4b00-9655-a8d1bfc5d340)
![image](https://github.com/user-attachments/assets/2ab90fb4-669a-4b3e-8d87-21cdc3768c08)

- It is standard to allocate permissive share permissions combined with restrictive NTFS permissions.

**Instead of making the necessary changes here, I'll do it from the command line and then come back for a before and after comparison.**

On PowerShell, I'll first check that the permissions match what was shown in the security settings, and they do.

![image](https://github.com/user-attachments/assets/b8109eb6-952a-4de9-b822-6b58ad73f5ca)

Now I'll run the following command which gives sec-glo-audit read access.

![image](https://github.com/user-attachments/assets/828278b3-a567-442f-8d8d-07cf0e103aad)
- The read permission was given with '/grant:r'
- The (OI)(CI) flags mean this permission is inherited by objects (files) and containers (subfolders) within the share.

Now I'll check the permissions again to verify that the new permissions were set correctly.

![image](https://github.com/user-attachments/assets/38ddb53e-b918-472f-a6e4-701f917e3c05)

Now if I check the LABFILES security settings, sec-glo-audit has read-only access, and as expected no changes were made to the share permissions, so authenticated users still have full control.

![image](https://github.com/user-attachments/assets/e36adb7c-4094-4496-87d8-014b97c6ef35)
![image](https://github.com/user-attachments/assets/40fe0c0b-16b8-498d-a945-2189fbbfe147)
![image](https://github.com/user-attachments/assets/6b4a9f17-fe92-4aab-a660-6cd31d2a0f96)
#
**Summary: Identity and Access Management is the act of provisioning and moderating access to the domain, based on security policies and the principle of least privilege. In this lab, I made sure that the security group designated for the audit team was properly configured in accordance with security policies and with the principle of least privledge in mind.**
