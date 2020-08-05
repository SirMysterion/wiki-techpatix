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
    * ACCT 
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