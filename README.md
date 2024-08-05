# LabOps :microscope:

![version](https://img.shields.io/badge/version-1.0-blue) ![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg) ![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)

# Objective

Below you can find a set of necessary steps for setting up Mattermost in a self hosted environent in reference to: PAPER TITLE

# Remarks

This repo has the following basic structure.

```
├── docs                <- Space for documentation
├── README.md           <- Top-level README. Includes a description for Wildcard and HTTPS compression proccedures for increasing overall speed and security of self hosted deployments. 
│
├──  Mattermost.YAML    <-
├──  Owncloud.YAML      <-
├──  Radicale.YAML      <-
├──  OnlyOffice.YAML    <-  
```


# Wildcard Certificate and HTTP2/HTTP compression:

To do before setting up selfhosted software. 

Wildcard certificates: a wildcard certificate allows for the management and reach of a secure HTTPS enabled docker container by securing subdomains under a main domain, which ultimately facilitates certificate renewal time and efficiency. Since both Mattermost and ownCloud need to be installed as docker containers, enabling wildcat certificates is necessary. In order to do this:

1. First navigate within the Synology Disk Station Manager (DSM) which is the interactive and management equivalent of a GUI based OS. Once here, the “cerficate” tab setting was selected by first accessing “security” under the connectivity column,  in the control panel.
2. Within the certificate tab, click on “add” and selected “replace an existing certificate” then selected the Synology DDNS certificate from the list (previously set up). Next selected get a certificate from let’s encrypt and set it as the default certificate by checking the box under it. Click next.
3. Under the “Get a Certificate from Let’s Encrypt” menu type in your DDNS, its format should look similar to: "X.synology.me". On the email area type in your email On the Subject Alternative Name, type in an asterisk symbol followed by a period (*.) and then type in your DDNS. Final result look something like *.X.synology.me. Click on done. 

HTTP/2+HTTPS compression: Multiple settings within the synology networking system can be easily activated to increase security and speed. HTTP/2 is one of these settings. It is reccomended to use the following 2 settings to improve the overall functionality of your setup. By enabling HTTP/2 website connections will become optimized by reducing overall latency without changing web app settings. In order to enable:
1. Go to control panel, select network, navigate to the connectivity tab and click on enable HTTP/2 and click apply.
2. HTTP compression, another useful setting to use with these setups, enhances bandwidth efficiency and increases transfer speeds between web servers and browsers when connecting through HTTPS. HTTP compression mitigates classical bottlenecking issues by minimizing the size of data transferred between servers and clients, and opmizises data needed to perform at full capacity. In order to enable,go to control panel, select security, and under the advanced tab check enable HTTP compression then click on apply. 


# Installing Portainer and Docker
1. Open the Synology Package Center and within it, search for, and then install Docker inc.’s “Container Manager”. 
2. Go back to the main interface and found “File Station”, Within it chose the folder named “docker”,
3. Head to the “Create” tab and established a new folder named “portainer” in lowercase letters. 
4. Navigate to Control  Panel, and under its menu select “Task Scheduler”, 
5. Next go into the “Create” tab and select “Scheduled Task” then “User-defiend script”.
6. Once this is done, a new window called “Create task” will open. Within it there are multiple empty fields that have to be filled as follows:

Task: Install Portainer
User: root
Uncheck the “enabled” box.

7. Move on to the next tab in the window called “Schedule” and select “Run on the following date” with today’s date and select “Do not repeat” underneath it. 
8. Continue by moving to the next tab called “Task Settings” and select the “Send run details by email” box, and input your email. 
9. Next input the following script in the “User-defined script” field and select “OK”: 

docker run -d --name=portainer \
-p 8000:8000 \
-p 9000:9000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /volume1/docker/portainer:/data \
--restart=always \
portainer/portainer-ce


10. Within the portainer menu,click on “Get Started” under the Environment Wizard’s quick setup. This will take you to the Environments home menu. 
11. Click on the ‘pencil’ shaped logo to edit the local environment. A new page will open where the environment details can be further filled out.
12. Under the “Public IP” field input the NAS’s local IP and click on the “Update environment” button to confirm the changes. 
13. After this the environment should be updated and confirmed with a message saying so. 
14. Next click on “Registries” under the main menu  and select “+Add registry”. From the options, shown select “Custom registry” and type in the registry’s details as follows:

Name: GHCR
Registry URL: ghcr.io

Repeat the process to add an additional 2 registries with the following details:

1.	Name: CODEBERG
2.	Registry URL: codeberg.org

1.	Name: Qauy.io
2.	Registry URL: quay.io

15. Click on “Add registry”:

# Reverse proxy and addition of YAML files

1. Once portainer is installed, navigate towards the control panel and click on “login portal”.
2. Once in here, go into the “Advanced” tab and click on reverse Proxy.
3. Select the “Create” tab within the Reverse Proxy menu.
4. Reverse Proxy Rules window will open and should be filled out as follows:

Source:
Protocol: HTTPS
Hostname: chat.yourname.synology.me 
Port: 443

Check Enable HSTS

Destination:
Protocol: HTTP
Hostname: localhost
Port: 8401

5. After inputting this information, switch to the custom header tab, then click on “Create” and choose the “WebSocket” option.
6. After this is selected, two input boxes for “Header Name” and “Value” will appear, leave as is and click on Save.
7. Go back to the main interface and find and select “File Station” and open the docker folder, which should have become available after the installation of docker done previously.
8. Within this folder, click on the create tab and select “folder” this will allow for the creation of a new folder.
9. Name this new folder “mattermost”.
10. Within this new folder, create 7 new folders with the following names in lowercase: “client”, “config”, “data”, “db”, “indexes”, “logs”, “plugins”.
11. Next, log in to portainer by visiting: https://localhost:9443 or the previously set up http://synology-ip-address:9000   and then your synology username and password.
12. Within the portainer UI, click on “Stacks” and then added a new stack by clicking on the “+Add stack” button. Name the stack “mattermost” and paste the contents of Mattermost.YAML file found in this repository in the Portainer Stacks Web editor section.


### Acknowledgment

Initial project structure was created following the structure in [this repo](https://github.com/malill/research-template).
