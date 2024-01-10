
- [Scope of Work](#scope-of-work)
- [getLandingZoneDetails subcommand details](#getlandingzonedetails-subcommand-details)
- [getElementDetails subcommand details](#getelementdetails-subcommand-details)
  
# Scope of Work

Customer has provided the below Scope of Work to SYNECTIKS during initial conversations.

Synectiks need to develop a Mobile App that allows Carecapplus users to login and authenticate and land up in some page where they sees all the payments that is pending.They can pay the same amount from the Mobile App and back end system gets updates about the payment. Finally users gets notifications.
 

Synectiks need to build software module for the following sub processes :

    1. Login and Authentication

    2. Landing Page and displaying the pending payment

    3. Pay the selected payment or enroll in payment plan
   
    4. Process payment through API with payment vendor

    5. update the backend system
   
    6. Notification and transaction log updates on customer payment

 

Few clarification on login and authentication

 

As we understand , we will have mobile number and OTP based login and authentication and we can have optional MFA based login and authentication.Does compliance etc forces us to use MFA etc? Please clarify on authentication process.
Authentication is needed for hipaa compliance and to prevent someone that have another person phone number and address to access their Patient health information. 
   

Few clarification on landing page and displaying the pending payment

 

As we understand , there is no API available at this time to get the pending payment information. We will get excel or CSV files daily through SFTP and we need to parse them and populate the payment information. If this is the business flow , then we will need to know the following information:

 

How will you furnish the regular CSV file to our App , would you like to upload them daily to any location like AWS S3 and we will automatically get the events and process that file?
[BG|| ] Step 1 – we will drop a .csv file 4x per day to a shared sFTP server – also located with synectics.  A transaction log can be shared in return.

Step 2 – an API can be set up in the webserver and connected to Pulse server (our software)

 

Is there any compliance requirement for sharing the file daily.[BG|| ]   All parts of this is are inside the Care Cap Plus firewall which is GREAT compliance. Jithendra can confirm this.
 

Few clarification on Pay the selected payment

 

1. We understand that its a stripe API integration. You should be getting the ample provision to have multiple payment options Yes.  Hopefully!

 

Few clarification on update the backend system

 

Please detail this use case , also let us know if there are any apis that is available to update the syste.[BG|| ]  no APIs exist back to Pulse as of now.  We update our system using payment reports from our payment vendor.

 

Few clarification on Notification and transaction log updates on customer payment

 

Please detail this use case and UX design[BG|| ]  A confirmation report in return would be ideal.

 


All the supported subcommands and there source code locations are as follows:

| S.No | Sub-command     |                Description                             |      Repository        |            functionalities                                                                                                         |
|------|-----------------------|----------------------------------------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------|
| 1    | getLandingZoneDetails       | Collect Information about any specific landing zone  |    Percentage(%)  | 1. Get Elements Metadata,<br /> 2. Get List of every elements with their config infos <br /> 3. Get List of products and Environments for the landing Zone<br /> 4. Get the cost of the landing zone                                                                                           |
| 2    |  getLandingZoneCompliance  | Collect Information about any specific landing zone compliances and security      |     Bytes         | 1. Get overall account Compliance ,<br /> 2.Get the elementWise Compliance <br /> 3. Run Compliance on products and environments                                                             |
| 3    |  getElementDetails | Collect Information about specific cloud elements -Run Queries |     Bytes         | 1.EC2,<br /> 2.EKS <br /> 3. ECS <br /> 4.LAMBDA <br /> 5. API Gw <br /> 6.Load Balancer<br />|
| 4    |  getCostDetails | Collect Information about account and elements specific costs |     Bytes         | 1.Total Account <br /> 2.Product and Envwise <br /> 3. Element Wise <br /> 4.Spikes and Trends <br /> 5. App/Data/Nw Service wise Costs <br />|
| 5    |  getBusinessServiceDetails | Collect Information about the business Services that is hosted on infrastructure |     Bytes         | 1.Account Wise infralist and corresponding services <br /> 2.Product and Env wise Service Lists along with hosted infrastructure <br /> |
| 6    |  getSlaDetails | Collect Information about Infra and Business Service Element Details SLA score |     Bytes     | 1.All Infra Elements <br /> 2.All Service Elements <br /> 3. Product and Env Wise Score <br /> 4.Product / Env / Business/ Common Services Score <br /> 5. App/Data/Nw Service Score <br />|
| 7    |  provision | Provision infra and App Services on AWS |     Bytes     | 1.landingZone <br /> 2.Product Enclave <br /> 3. Cluster <br /> 4.Product and Services in product enclave  <br /> |
| 8    |  diaganose | Run Diagnostics on  infra and App Services on AWS |     Bytes     | 1.Infra Elements<br /> 2.Service Elements <br />  |
| 9    |  health | Run Diagnostics on  infra and App Services on AWS |     Bytes     | 1.Infra Elements<br /> 2.Service Elements <br />  |
| 10    |  secret | secret management of infra and service elements |     Bytes     | 1.Infra Elements<br /> 2.Service Elements <br />  |


Please refer the specification of subcommands for every subcommands input/ output / algos.

# getLandingZoneDetails subcommand details

| S.No | CLI Spec|  Description                           
|------|----------------|----------------------|
| 1    | awsx --vaultURL=vault.synectiks.net getLandingZoneDetails --zoneId="1234" --getElementsMetadata  | This will get the metadata from AWS config service |
| 2    | awsx --vaultURL=vault.synectiks.net getLandingZoneDetails --zoneId="1234" --getElementsDetails  | This will get all the element details from AWS config service |
| 3    | awsx --vaultURL=vault.synectiks.net getLandingZoneDetails --zoneId="1234" --getProductDetails  | This will get all the product and environment details that is hosted in that landing zone |
| 4    | awsx --vaultURL=vault.synectiks.net getLandingZoneDetails --zoneId="1234" --getCostDetails  | This will get all the cost details for the landing zone |

# getElementDetails subcommand details

This subcommand will need to take care for all the cloud elements and for every element, we need to support the composite 
method like network_utilization_panel. So , we can keep a single repo for the subcommand and keep separate folders for the different element handlers.

| S.No | CLI Spec|  Description                           
|------|----------------|----------------------|
| 1    | awsx --vaultURL=vault.synectiks.net getElementDetails --elementId="1234" --elementType=EC2 --query="ec2-config-data"  | This will get the specific EC2 instance config data |
| 2    | awsx --vaultURL=vault.synectiks.net getElementDetails --elementId="1234" --elementType=EC2 --query="cpu_utilization_panel"  | This will get the specific EC2 instance cpu utilization panel data in hybrid structure |
| 3    | awsx --vaultURL=vault.synectiks.net getElementDetails --elementId="1234" --elementType=EC2 --query="storage_utilization_panel" | This will get the specific EC2 instance storage utilization panel data in hybrid structure|
| 4    | awsx --vaultURL=vault.synectiks.net getElementDetails --elementId="1234" --elementType=EC2 --query="network_utilization_panel"  | This will get the specific EC2 instance network utilization panel data in hybrid structure |
