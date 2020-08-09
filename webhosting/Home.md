# Techpatix Website (Landing Page) and Wiki Site 

Techpatix Landing Page and Wiki Site are developed using HTML, CSS, Bootstrap, and JavaScript. Techpatix used ATOM an Open Source code editor to develop the coding part of the site-building. The landing page serves the business side needs and the Wiki site is developed for the Internal project content and details. The sites are hosted from the Cloudflare account.  

The landing page is hosted as the main domain and the Wiki page is created and hosted as sub-domain of Techpatix. URLs as listed below:  

[https://www.techpatix.com](https://www.techpatix.com)  
[https://wiki.techpatix.com/Home](https://wiki.techpatix.com/Home) 

Company Website is built as a Complete Responsive site using Responsive Web Designing (RWD), where users can access the site from any device across the world and the website will fit into the device size.  
![image](https://wiki.techpatix.com/webhosting/1.png)

And Wiki page covers mostly on project documentation like configurations and documentation of Network Components, Active Directory, DFS, NMS, Disaster Recovery Plan, etc. 
![image](https://wiki.techpatix.com/webhosting/2.png) 

The landing page coding file has been deposited in Git Hub repository and is linked to the Techpatix Azure portal under Static Web Apps. GITHUB account is merged within ATOM for continuous deployment of site changes. This Documentation cover steps to create GitHub Repository and getting azure URL for the Static App created.   

## Creating Git Hub Repository: 

Once the account is created and signed up with Git Hub, the “New Repository” should be created. There are 2 ways to create new repository, either from the top right corner of the page by clicking the dropdown menu with “+” symbol or from dashboard > Repositories > New. 

![image](https://wiki.techpatix.com/webhosting/3.png) 

Name the Repository, and fill the description as required, Choose if you want to make your repository Private or public and Create Repository.  

![image](https://wiki.techpatix.com/webhosting/4.png) 

Once repository is created, files are uploaded under <>Code > Add File (Dropdown) > Upload Files. Selected Files can be uploaded using drag and drop in Git Hub.  

![image](https://wiki.techpatix.com/webhosting/5.png) 

## Creating Static Web App using Azure Portal 

Login portal.azure.com and switch to respective Directory and from Azure Services or All Services sections, using “Static Web Apps” create an App so that we can get an azure URL.  

![image](https://wiki.techpatix.com/webhosting/6.png) 

The Source Code which is deposited in the Git Hub repository can be made highly available using this static web app. Under Static Web Apps, create a Landing page App using “Add”.  

![image](https://wiki.techpatix.com/webhosting/7.png) 

Select “Subscription” and “Resource Group” under project details. Under Static Web App details, Name the web app, Region, and SKU plan. Sign in with Git Hub from Source Control Details so that Azure will create a workflow to Git Hub and access the source code uploaded in Git Hub.  

![image](https://wiki.techpatix.com/webhosting/8.png) 

Once the Web App is created, Azure will give a Static URL. The Azure Static Web App URL is further used for hosting domain from Cloudflare. Azure Static URL is converted to the required domain name by creating CNAME in Cloudflare.   

![image](https://wiki.techpatix.com/webhosting/9.png) 

To simplify the development workflow, Git Hub is integrated into ATOM using GitHub Desktop so that the developing team needs not to have any access to Company’s GIT account or Azure Portal account. The Source code files are saved into the GitHub root folder.  

![image](https://wiki.techpatix.com/webhosting/10.png) 

Via ATOM, these files are accessed and any change to the source code can be updated to LIVE site using GitHub Desktop. Once Changes are made to source code, the changed file is listed under Untagged changes, where users need to click “stage all” and Commit to Master. The Changes will be updated to GitHub Master Repository and the azure workflow will update the same to live site.  

![image](https://wiki.techpatix.com/webhosting/11.png) 
