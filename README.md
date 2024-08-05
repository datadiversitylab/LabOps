# LabOps :microscope:

![version](https://img.shields.io/badge/version-1.0-blue) ![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg) ![MIT license](https://img.shields.io/badge/License-MIT-blue.svg)

# Project Intro/Objective 

The purpose of this project is _______.

# Remarks

This repo has the following basic structure.

```
├── docs                <- Space for documentation. Can also included conceptualization and literature review.
├── README.md           <- The top-level README for developers.
│
├──  Mattermost_initialization.YAML    
│
├──  Portainer_install.YAML          
│
├──  Web_editor_content.YAML              
```


Wildcat and HTTPS compression:
Wildcard certificates: a wildcard certificate allows for the management and reach of a secure HTTPS enabled docker container by securing subdomains under a main domain, which ultimately facilitates certificate renewal time and efficiency. Since both Mattermost and ownCloud were installed as docker containers, enabling wildcat certificates was necessary: In order to do this, we first had to navigate within the Synology Disk Station Manager (DSM) which is the interactive and management equivalent of a GUI based OS. Once here, the “cerficate” tab setting was selected by first accessing “security” under the connectivity column,  in the control panel. Within the certificate tab, we clicked on “add” and selected “replace an existing certificate” then selected the Synology DDNS certificate from the list (previously set up). Next selected get a certificate from let’s encrypt and set it as the default certificate by checking the box under it. Then click next. Under the “Get a Certificate from Let’s Encrypt” menu type in your DDNS, its format should look similar to: X.synology.me. On the email area type in your email On the Subject Alternative Name, type in an asterisk symbol followed by a period (*.) and then type in your DDNS. Final result look something like *.X.synology.me.Click on done. 

HTTP/2+HTTP compression: Multiple settings within the synology networking system can be easily activated to increase security and speed. HTTP/2 is one of these settings. It is reccomended to use the following 2 settings to improve the overall functionality of your setup. By enabling HTTP/2 website connections will become optimized by reducing overall latency without changing web app settings. In order to enable, go to control panel, select network, navigate to the connectivity tab and click on enable HTTP/2 and click apply. HTTP compression, another useful setting to use with these setups, enhances bandwidth efficiency and increases transfer speeds between web servers and browsers when connecting through HTTPS. HTTP compression mitigates classical bottlenecking issues by minimizing the size of data transferred between servers and clients, and opmizises data needed to perform at full capacity. In order to enable,go to control panel, select security, and under the ad



### Acknowledgment

Initial project structure was created following the structure in [this repo](https://github.com/malill/research-template).
