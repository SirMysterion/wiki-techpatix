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
