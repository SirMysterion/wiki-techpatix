# AD DFFS shares and policies  
## Active Directory Setup 
1. Create domain and promoted HQ-DC-01 to Domain Controller. 
    * Using the at the time temporary IP addresses we had used to get initial connectivity we promoted our DC and prepared the Techpatix.com domain  
2. Joined HQ-FS-01 to the domain and gave it File Server roles and attributes 
   * This included installing the DFS role 
3. Joined BR-DC-01 to the domain as a secondary domain controller. 
4. Joined BR-FS-01 to the domain 
    * Installed file server role and DFS roles 
5. Added proper subnets to the Sites and Services.  
    * This was applied once the proper IP addresses were applied: 
    * HQ: 
        * 172.16.110/24 
        * 172.16.120/24 
        * 172.16.150/24 
    * BR: 
        * 172.16.210/24 
        * 172.16.220/24 
        * 172.16.250/24 
    * IPV6 was handled by Thomas and are as Follows 
    * HQ: 
        * 2600:70ff:b856:8110::/64 
        * 2600:70ff:b856:8120::/64 
        * 2600:70ff:b856:8150::/64 
    * BR 
        * 2600:70ff:b856:8210::/64 
        * 2600:70ff:b856:8220::/64 
        * 2600:70ff:b856:8250::/64 
6. Created HQ and BR OU structure 
    * HQ 
        * Departmental OUs 
            * ACCT 
                * This OU is for the accounting department. 
                * This OU contains the ACCT group. 
            * MGMT 
                * This OU is for the management teams. 
                * This OU contains the MGMT group. 
            * MRKT 
                * This OU is for the Marketing department. 
                * This OU contains the MRKT group. 
            * SALES 
                * This OU is for the Sales department. 
                * This OU contains the SALES group. 
            * SHPP 
                * This OU is for the Shipping Department 
                * This OU contains the SHPP group. 
        * Administrator Group 
            * This group will hold the administrators that are assigned to headquarters. 
            * They have permissions that are appropriate to their position as administrators. 
    * BR 
        * Departmental OUs 
            * These are designed to mirror the HQ departmental OUs and thusly have the same structure: 
            * ACCT 
                * Accounting department OU and Group. 
            * MGMT 
                * Management department OU and Group. 
            * MRKT 
                * Marketing OU and Group. 
            * SALES 
                * Sales OU and Group. 
            * SHPP 
                * Shipping OU and Group. 
            * BR Administrator Group 
                * Like the HQ admin group, this group will hold the administrators for the Branch Location 
    * Domain Admins 
            * This group is held above the OUs and represents our permissions to build and configure the Domain. For future use the administrators that are assigned to maintain and control the domain in its entirety will be placed in this group. 
7. Users were imported and placed in groups appropriate to their position 
   * For testing purposes there should be one or two for each department 

## DFS Share Set up 

This is the process used to install the DFS share for Techpatix.com allowing for file access for departments across the Domain:  

1. Install DFS on both HQ-FS-01 and BR-FS-01 
2. Create folders to be linked to on both HQ-FS-01 and BR-FS-01  
    * ACCT 
    * MGMT 
    * MRKT 
    * SALES 
    * SHPP 
3. Create DFS Folders and Link them to the appropriate folders  
4. Ensure that linked folders are properly shared out with appropriate access  
    * Administrators  
        * Full Control 
    * Departments 
        * Read 
5. Ensure that departmental folders have the correct permission ACLs 
    *ACCT 
        * HQ-GRP-ACCT 
            * Read Write Execute 
        * BR-GRP-ACCT 
            * Read Write Execute 
        * HQ-GRP-ADMIN 
            * Full Control 
        * BR-GRP-ADMIN 
            * Full Control 
        * Domain Admins 
            * Full Control 
    * MGMT 
        * HQ-GRP-MGMT 
            * Read Write Execute 
        * BR-GRP-MGMT 
            * Read Write Execute 
        * HQ-GRP-ADMIN 
            * Full Control 
        * BR-GRP-ADMIN 
            * Full Control 
        * Domain Admins 
            * Full Control 
    * MRKT 
        * HQ-GRP-MRKT 
            * Read Write Execute 
        * BR-GRP-MRKT 
            * Read Write Execute 
        * HQ-GRP-ADMIN 
            * Full Control 
        * BR-GRP-ADMIN 
            * Full Control 
        * Domain Admins 
            * Full Control 
    * SALES 
        * HQ-GRP-SALES 
            * Read Write Execute 
        * BR-GRP-SALES 
            * Read Write Execute 
        * HQ-GRP-ADMIN 
            * Full Control 
        * BR-GRP-ADMIN 
            * Full Control 
        * Domain Admins 
            * Full Control 
    *SHPP 
        * HQ-GRP-SHPP 
            * Read Write Execute 
        * BR-GRP-SHPP 
            * Read Write Execute 
        * HQ-GRP-ADMIN 
            * Full Control 
        * BR-GRP-ADMIN 
            * Full Control 
        * Domain Admins 
            * Full Control 

6. Ensure that all servers that are participating in the DFS shares are set as namespace servers. This includes the two DCs as the shares would not show up in the \\\techpatix.com location without the DCs set as Namespace servers. 
7. Implement replication between HQ-FS-01 and BR-FS-01 
8. Replication allows for files created on one FS to be created and present on the other. This also allows changes made to files to update both instances providing redundancy in order to prevent data loss 

## Group Policies (GPO) 

1. Login Banner 
    * This Policy is applied at the domain level. 
    * This is the Login Banner that would indicate policy for proper use of systems and legal notice against trespassing. 
    * Title : TECHPATIX SOLUTIONS 
    * Message: 
```
“~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

---------------------------WARNING------------------------------ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 

This computer is Property of Techpatix.com. 

Unauthorized Access is strictly prohibited, Violators will be 

prosecuted. 

Please adhere to your departmental policy with regards to the use of this computer.” 
```
2. Password regulations 
    * This policy is applied at the domain level. 
    * These are the regulation on password complexity for security purposes: 
    * Password History 
        * 10 passwords remembered 
    * Maximum Password Age 
        * 90 days 
    * Minimum Password Age 
        * 30 days 
    * Minimum Password Length 
        * 8 characters 
    * Complexity 
        * All three of the character types 

3. CMD Access denial 
    * Non-administrator accounts should not have CMD access 
    * This was applied directly to the departmental OUs 

4. Drive Map 
    * This is applied at the domain level 
    * Drive map to the DFS share as drive letter: T (for Techpatix) 
    * This allows for direct access to the share for the departments. 