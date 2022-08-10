# Spamurai
### Email Antispammer AI solution


![AI](https://img.shields.io/badge/AI-Machine--Learning-blueviolet)
![Department](https://img.shields.io/badge/Department-Security--Engineering-yellow)
![team](https://img.shields.io/badge/Category-Blue--TEAM-blue)
![Project](https://img.shields.io/badge/Project-R&D-yellow)
![Aim](https://img.shields.io/badge/Aim-Graduation--Project-yellow)
![Offline](https://img.shields.io/badge/Offline-Sandbox-red)
![API](https://img.shields.io/badge/API-Django-success)

[![Author](https://img.shields.io/badge/Author-K-blue)](https://ahmedkawwad.github.io/)


![](assets/carbon.png?raw=true)

<!-- ```
/ _\_ __   __ _ _ __ ___  _   _ _ __ __ _(_)
\ \| '_ \ / _` | '_ ` _ \| | | | '__/ _` | |
_\ \ |_) | (_| | | | | | | |_| | | | (_| | |
\__/ .__/ \__,_|_| |_| |_|\__,_|_|  \__,_|_|
   |_|                                      
``` -->
<!-- ```
 _______  _______  _______  _______           _______  _______ _________
(  ____ \(  ____ )(  ___  )(       )|\     /|(  ____ )(  ___  )\__   __/
| (    \/| (    )|| (   ) || () () || )   ( || (    )|| (   ) |   ) (   
| (_____ | (____)|| (___) || || || || |   | || (____)|| (___) |   | |   
(_____  )|  _____)|  ___  || |(_)| || |   | ||     __)|  ___  |   | |   
      ) || (      | (   ) || |   | || |   | || (\ (   | (   ) |   | |   
/\____) || )      | )   ( || )   ( || (___) || ) \ \__| )   ( |___) (___
\_______)|/       |/     \||/     \|(_______)|/   \__/|/     \|\_______/
```
 -->

This is Research and development (R&D) project aim to find solution to prevent or less the interaction of users with any harmful emails.


In order to full understand it - basic knowledge required:

- `Python` more than basic level.

- `Machine-Learing` A good understanding with basics.

- `Web-development` How to build a system and make integrations.

- `Malware Analysis` How to analysis malware behavoir and malicious actions.

- `Open-source` How to build and deploy an open source.

One of the most important risks which faces the victims of phishing is the spam and this is our main problem that to reduce and detect it and protect user from unconscious interaction with these spam emails, we will design a system to make a layer of filtration between the user and the incoming emails.

As the Spamurai project:
• Helps to decrease the victim interaction with spam emails.
• Email spam detection and filtration.
• Build a system based on machine learning that can improve with time.
• Reach to the maximum accuracy as much as possible.

## API 

See [API](spamurai.api) for the API client
documentation.

## Development

- We had been using Email client devloped by PHP
also for external analysis API integrations. 
- API for separated analysis stage like TEXT analysis, URL Analysis by Python Django Rest Framwork
- Cuckoo opensource for attachment analysis used sperated Python Flask API

## Machine Learning
 
We used two models for classifications in two stages of text and urls.

### Natural language processing NLP

Model Conecept Illustration

[NPL Model](models/text/)

### RainForest Classifier

Model Conecept Illustration

[Random Forest Model](models/url/)

## Usage

First things first, to have machine with all the requirments as following:
- Cuckoo v2 
    - Suricata
    - Internet routing
Then Configure your cuckoo guests IP , enable MongoDB and enable all services needed .

- Email Client cloned 
Install Project requirments to work with PHP

- Django API cloned
Get in API dir and install all requirments in requirments.txt

- RUN in Terminal:
    - Run VirtualBox 

    `VBoxManage startvm "192.168.56.1011" --type headless`

    - Activate Cuckoo venv

    `workon cuckoo-test` (cuckoo-test = path/to/cuckoo/venv/bin/activate)

    - Run Cuckoo Sandbox

        - 1st terminal had rooter running : `cuckoo rooter --sudo --group cuckoo`
        - Then 2nd  terminal run cuckoo    : `cuckoo`
        - 3rd terminal run web interface  : `cuckoo web --host 192.168.xxx.xxx --port 8080` 
        
        (192.168.xxx.xxx is your VM ip address over the network )
        And Boom we are done with the host.
    - Activate API venv

    `. $api` ($api = path/to/venv/bin/activate)
    - Run Django API
     `python manage.py runserver 192.168.xxx.xxx:8050`

     (192.168.xxx.xxx is your VM ip address over the network )

    - Start Apache and MySql on the host machine
    - Run Email Project
    
        - clone email client repo (this private for developers mode only )

        - get in project directory
        `php artisan serve --host=192.168.xxx.xxx --port=8000`

        (192.168.xxx.xxx is your host ip address over the network )
    
Notes About the project:
PHP Web interface implmenting is so bad , need to rebuilt or replaced by Django web app (prefferable)
not dynamic , full of static variable due to IP changes
VT API not to be called out of python API , pereferable to save files in same place to avoid duplicated and consume storage.
Change in Scoring system funcitonality with a proper one to evaluate each item inside each stage. (avoid skip items )

<p align="center">
Copyright©️ 2022 Spamurai Project </br>
Communication and Electronics Department - Faculty of Engineering - Helwan University</br>
Contributers: Ahmed Khaled Awwad , Ahmed Akram Mahmoud , Mahmoud Ahmed , Amr Sayed , Gamal Eldin Magdy
</p>

