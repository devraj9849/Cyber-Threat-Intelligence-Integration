# Cyber-Threat-Intelligence-Integration LAB

## Objective

The objective of developing an OpenCTI lab at home is to gain practical experience in cybersecurity threat intelligence by analyzing and managing cyber threats through hands-on interaction with industry-standard tools and frameworks. This lab environment will allow for customized research on specific threats, integration of various data sources for comprehensive analysis, and foster collaboration with peers. Ultimately, the lab aims to enhance skills, support career development, and prepare for certifications by providing a strong foundation in threat intelligence.

### Skills Learned

- Successfully integrated OpenCTI with Splunk SIEM to enhance the correlation of threat intelligence data with security event logs.
- OpenCTI Configuration and Management: Skilled in installing, configuring, and maintaining the OpenCTI platform, including managing its various components and integrations.
- Collaboration and Reporting: Developed the ability to collaborate on and share threat intelligence with peers, and produce detailed reports and actionable intelligence for stakeholders.
- Technical Troubleshooting: Strong troubleshooting skills, with experience solving issues related to the setup, configuration, and operation of the OpenCTI platform.

### Tools Used

- Splunk Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Github
- OpenCTI
- Ubuntu OS

<h1>Cyber Threat Intelligence</h1>

![image](https://github.com/user-attachments/assets/fb1d3af4-1e72-4a66-b9b2-537c3570b827)

 
Cyber Threat Intelligence (CTI) refers to the information and insights that organizations gather and analyze about potential or existing threats to their cybersecurity. This intelligence is used to understand, prevent, and respond to cyber threats more effectively. CTI includes details about the tactics, techniques, and procedures (TTPs) used by cybercriminals, as well as indicators of compromise (IOCs) such as malicious IP addresses, domains, or malware signatures.
Key Aspects of Cyber Threat Intelligence:
•	Strategic Intelligence: High-level information about broad trends, emerging threats, and potential impacts on an organization. It helps in decision-making at the executive level.
•	Operational Intelligence: Information about specific attacks or campaigns, often providing context on the adversaries, their goals, and their methods. It supports day-to-day cybersecurity operations.
•	Tactical Intelligence: Technical details about specific threats, such as malware signatures, IP addresses, or URLs associated with an attack. It’s often used to improve defenses and detect threats.

•	Technical Intelligence: Information about how threats work, including how malware is structured or how exploits work. It’s useful for developing countermeasures and understanding vulnerabilities.
Importance of CTI:
•	Proactive Defense: Helps organizations anticipate and prevent attacks before they happen.
•	Informed Decision-Making: Enables better strategic planning and resource allocation.
•	Improved Incident Response: Enhances the ability to detect, analyze, and respond to security incidents.
•	Collaboration: CTI often involves sharing information between organizations, sectors, and governments to improve overall cybersecurity.
In essence, CTI is a crucial component of a robust cybersecurity strategy, helping organizations stay ahead of evolving threats.


<h1>PART1: Setup OpenCTI</h1>

For installing OpenCTI threat intelligence platform you should already have an Ubuntu Operating system with Splunk installed in it. For this you can visit 
https://medium.com/@khadkadevraj100/installing-splunk-into-the-ubuntu-operating-system-9fff0d1ffdbc
Install another Ubuntu OS and name that system as OpenCTI. The default settings for Ubuntu are okay for this lab. After everything is installed, opening it with MobaXterm we get
![image](https://github.com/user-attachments/assets/9e0f7f6d-ae40-46d1-9868-90ba0e0c3c55)

                Fig: IP address of the Ubuntu machine 192.168.132.130
![image](https://github.com/user-attachments/assets/d6078fd8-2973-46b4-883a-7234837a1b41)
                
                Fig: IP address of the opencti ubuntu machine 192.168.132.134
Next we want to install Docker compose as we will be using Docker to spin up OpenCTI.
                                  
                                  sudo apt install docker-compose
![image](https://github.com/user-attachments/assets/25f5d170-b3a1-4867-ba1d-cb19d99ed215)

After docker is installed let’s clone the repository of OpenCTI. Go to the URL https://docs.opencti.io/latest/deployment/installation/#using-docker
![image](https://github.com/user-attachments/assets/bc972441-bc90-49a3-a963-80faea99c115)

Copying the URL into the terminal of OpenCTI we have
![image](https://github.com/user-attachments/assets/c0616753-f737-4b10-8457-638027e68737)
 
Here inside the docker directory, there are 4 files. Among them we are more interested in the file docker-compose.dev.yml which contains all the configurations that Docker will reference. This yaml file references a environmental variable called .env.sample
![image](https://github.com/user-attachments/assets/77949267-0a24-4e5e-9563-585f5848db6a)

  
.env.sample is the hiden file that the Docker-compose.dev.yml will reference. Renaming the file to .env using mv command

    mv .env.sample .env
![image](https://github.com/user-attachments/assets/be4425c5-af3e-41c6-873b-9bd4e2bb1466)

 
Opening the .env file using the vi command 

     sudo vi .env
![image](https://github.com/user-attachments/assets/ae51f2a5-f453-485e-9922-4f2494e93565)


Here I have made some changes like OPENCTI_ADMIN_PASSWORD, OPENCTI_ADMIN_TOKEN (you can add any random token from the site https://www.uuidgenerator.net/), OPENCTI_BASE_URL where you add the opencti ip address followed by 8080, MINIIO_ROOT_PASSWORD, RABBITMQ_DEFAULT_PASS. After that close the file using the command wq!.
Now we have updated our variables, there is one last thing that we need to do which is updating the elastic. 

    sudo sysctl -w vm.max_map_count=1048575
Here we are using this command for assigning a static map count for elastic to use.
![image](https://github.com/user-attachments/assets/61aafd30-52df-4d1e-ad9d-e784aea42d21)

 
Now we can start the docker using the command 

    sudo systemctl start docker.service
    sudo docker-compose up -d
![image](https://github.com/user-attachments/assets/ba4702e0-ad19-489c-b4aa-92788c53e608)

 
This will download everything that is required within that docker -compose.yaml file. I have found that the above command doesnot work properly with all the UBUNTU versions. Be careful with that. And once this is done we can then access our OpenCTI.
To see everything is up and running as OpenCTI takes some time to load everything. we can use the command:

    sudo docker-compose ps
![image](https://github.com/user-attachments/assets/85537121-b98e-49f1-960b-bed587619cc1)

 
After the state is up, we can open openCTI in browser

    192.168.132.134:8080
![image](https://github.com/user-attachments/assets/9f48cebb-c5a6-452d-8082-6a01f781453c)

 
After entering OpenCTI credentials, the dashboard of OpenCTI is seen.
![image](https://github.com/user-attachments/assets/4efa9c76-e69c-4b07-9830-5555dd267a34)

 
Now the next thing to do is adding external connectors. And these external connectors are third party websites that can integrate with OpenCTI to provide additional capabilities. For this project we are going to use OTX AlienVault. In order to integrate with OTX AlienVault, first we must create an account with OTX AlienVault.
After creation of account Select API integration. And on the right you will be seeing OTX key which you will be using for integrating with OpenCTI.
![image](https://github.com/user-attachments/assets/579aa4be-e44f-4324-9d88-18c492184e9a)


 
OpenCTI can be connected with lots of vendors like any-run, virustotal, crowdstrike,  cuckoo and many more. To see the list of available third-party applications that can be integrated with OpenCTI, go to the website: 

    https://github.com/OpenCTI-Platform/connectors/tree/master/external-import
For this project we are using AlienVault. While looking at the OpenCTI Alienvault Connector from the link above there is an example provided docker-compose.yml file. Copying the contents of the file.
![image](https://github.com/user-attachments/assets/72c60598-0c15-4edf-be3e-f4ee4c5a023d)

 
Copying the contents of the file in notepad and making some changes as necessary. Taking .env file as a reference.
OPENCTI_Token= xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx (from .env file)
CONNECTOR_ID= 89b967e8-4812-4f85-b266-4875ae08c7a2 (from UUID)
ALIENVAULT_API_KEY = xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx(fromAlienVault OTX)
These are the things we are going to make changes on the file.
Then we are going to add the content we just made changes in the notepad and add it in docker-compose.yml file.

    sudo vi docker-compose.yml
![image](https://github.com/user-attachments/assets/8754378d-a9fe-4682-b4ac-002a33fc0d13)

 
Here you have to make sure that the files you copied are in correct order otherwise it will generate an error. Finally we stop the docker again and start it again.
![image](https://github.com/user-attachments/assets/46cb3af4-eef5-428e-af05-478463b560b9)

![image](https://github.com/user-attachments/assets/0ee20ef6-9de4-4fda-8e3b-77bfbd1e769a)
![image](https://github.com/user-attachments/assets/cc416181-fe74-4dca-ad55-45b50bcd4b79)

 
Then we can see the OpenCTI dashboard.
After this click on Data>Ingetion>Connectors>AlienVault and take a look at status. If you see status In Progress this means its fetching all of the data within AlienVault and pushing that into OpenCTI.
![image](https://github.com/user-attachments/assets/78a1e667-f8fc-431b-b3d5-336660d842de)

 
After the above process we can think of integrating Splunk into the OpenCTI.

<h1>Part2 -Integrating Splunk</h1>
Head into the system where your Splunk is installed. For mine it is installed inside the Ubuntu Operating system.

![image](https://github.com/user-attachments/assets/f1e37911-3503-4201-9b2f-76ae519fa9ce)

 
Go to Settings>Tokens>New-Token
![image](https://github.com/user-attachments/assets/acecd8a2-f619-4ed0-838e-448c964245d3)

 
Copy the token into safe place as it will not appear again as shown in the screenshot.
Go to Token Settings on the top right corner and make sure that Token Authentication is Enabled.
Similar to the Alienvault we connected, we are going to do it for splunk now. For this go to the connector github page, and copy the docker.compose.yml file contents.
![image](https://github.com/user-attachments/assets/7df9a414-33b1-42b6-8e93-875662567c01)

 
Paste it in opencti docker.compose.yml file and make changes.
![image](https://github.com/user-attachments/assets/5dffc53a-a7e8-4a2f-b974-97f8b8c04027)

 
Here make sure to change the OPENCTI_URL to http://opencti:8080.
The highlighted things are the texts that we changed in the file that we get from connector github website for splunk. Here CONNECTOR_ID is the UUID generated from UUID generator website.
After this we stop the docker connection and again start the connection using the command sudo docker-compose up -d as we did it for alienvault. And seeing whether all the docker components are up or not.
![image](https://github.com/user-attachments/assets/2b9bec05-83d7-46ea-a4ef-eea29cad496c)

 
Opening the OpenCTI and seeing both the Splunk and AlienVault are in progress.
![image](https://github.com/user-attachments/assets/48d9708d-c18d-4949-be3f-228deb7f17ab)

 
Now after the completion of above process we are going to create a KV lookup in splunk. To do that go to Settings>Knowledge>Lookups>Lookup definitions>NewLookup Definition.
![image](https://github.com/user-attachments/assets/2444fcc1-6fe9-4a4f-8944-88f480aea03e)


 
KVstore grabs data from our OpenCTI and puts into our Splunk Environment. When you look at the Splunk connector GitHub page at the bottom of the page there is written supported_fields which we are interested in this project. For this we are selecting file type. Copying all the supported fields of file type and placing into the Supported fields in Lookup definitions.
So for checking whether it is integrated to splunk or not. For that we can go to search and type

    |inputlookup devraj_openccti 
Where devraj_openccti is the lookup definitions file created earlier
![image](https://github.com/user-attachments/assets/cd3b05c2-d37f-4564-bf4e-536a051d83af)

 
<h1>PART3- Creating CSV and Observables</h1>
To create a CSV feed from OpenCTI to use in Splunk, what you want to do is head over to Data>Data sharing > CSV feeds. Then click on the + button on the bottom of the page.

![image](https://github.com/user-attachments/assets/0ccf90ca-c410-4d66-bd22-5fd31d21166c)

 
The reason we are doing this because If we are investigating internal compromise, you can then copy all of the IOCs that you have identified and put them in your own threat feed. So not only you will detect similar threats in the future, but you are also building up the library of all of the threats that you have seen in your environment. This can be an incredible knowledge base to have. And if you constantly see the same threat targeting your environment, well maybe that is something you can start working towards to strengthen your security posture so that doesn’t happen anymore which essentially will feed into reducing the risk of compromise. 
![image](https://github.com/user-attachments/assets/fa344615-0cd8-4814-b11f-209b0c4081b3)

 
Click on observables > + button on bottom > Domain name and add the following
![image](https://github.com/user-attachments/assets/064412d9-7941-4935-a1b0-269c021bb1bc)

 
When we click create we see Domain name xyz.cc added in the opencti. 
![image](https://github.com/user-attachments/assets/075497d0-0a06-44ab-ac11-015ff70819e6)

 
Here we can see xyz.cc added into the list of domains.
After this we can use this CSV feed to feed into the Splunk instance. Going to Settings>Lookups>Lookup Definition >devraj_openccti
Here when clicking on the opencti file we created, I am going to add values in supported fields as it is a field for domain name.This information can be found in the connectors for splunk github page.
![image](https://github.com/user-attachments/assets/265cd400-268b-43be-95f1-18230dad7b61)

 
So, for this project I am just going to add the value name in supported fields section in our definition file. After that click on Save.
![image](https://github.com/user-attachments/assets/1e1c9dd7-6874-489b-a2ed-cefbb59b8b4b)

 

After that going to search bar in Splunk and typing the command

    |inputlookup devraj_openccti
    |search value=xyz.cc
![image](https://github.com/user-attachments/assets/037facac-953d-4b23-bfbc-fb56464b922e)

 
<h2This is a great way in which we can detect IOCs that we are currently finding and getting information about it in the future too.</h2>


<h1>Important Links for the project</h1>

<b>Clone the repo</b>

    git clone https://github.com/OpenCTI-Platform/docker.git

  <b>OpenCTI External Connectors</b>
    
    https://github.com/OpenCTI-Platform/connectors/tree/master/external-import
    
<b>Splunk OpenCTI External Connectors</b>

    https://github.com/OpenCTI-Platform/connectors/tree/master/stream/splunk

<b>OTX Alienvault</b>
  
    https://otx.alienvault.com/
