# API developing guide lines 

Api to communicate and using Spamurai Analysis system

## Enviroment Setup

- Install Python 3.x

 `~/$ sudo  apt install python`
- Install venv

 `~/$ sudo  apt install python3.9-venv`
- Clone repo

 `~/$ sudo git clone https://github.com/AhmedKAwwad/Spamurai.git`
- Go to repo
 
 `~/$ cd Spamurai/spamurai.api`

Now we will creat our venv inside spamurai.api directory
- create python3 venv (for example `api_env`)

 `~/Spamurai/spamurai.api$ python3 -m api_env .`
- Activate your venv

 `~/Spamurai/spamurai.api$ . .api_env/bin/activate `
- Install Required Packages 

 `(api_env)~/Spamurai/spamurai.api$ pip install -r requirements.txt `

- Start the local server 

 `python manage.py runserver 127.0.0.1:8050`

## Resources

Following is a list of currently available resources and a brief description of each one. For details click on the resource name.

Resource | Description |
-------- | ----------- | 
`POST` [/api/signup](###/api/signup)  | Regsister a new account to can use analysis process through the API.|
`POST` [/api/analysis](###/api/analysis)  | Send Email Content TEXT and URLs to Machine learning model to be processed and analyzed.|
`POST`[/api/analysis/attachment](###/api/analysis/attachment)  | Send Email Content attachment to offline cuckoo sandbox to be processed and analyzed.|

### /api/signup

**POST /api/signup**

Register a new account through API dashboard to have access to the API by API KEY .

**URL:**
localhost:8080/api/signup
**Request format in json**
```json
{
	"first_name": "Your Name",
	"last_name": "Your last Name",
	"user_name": "NickName",
	"email": "test.email@example.com",
	"password": "Use Strong Password"

}
```
Example response.

```json
{
	"Account with 'NickName' has successfully created"
}

```

### /api/analysis

**POST /api/analysis**

Request analysis task for Email content Text and URLs.

**URL:**
localhost:8080/api/analysis
**Request format in json**
```json
{
	"api_key": "Y0UR4P1K3Y ",
	"urls": ["www.example.com","bengin.domain/first_directory","www.malcious.site/dir/2nd_dir"],
	"text": "Subject; Hello Dear.. Hope this email find you well I send you \"analysis report \" to be checked and whatever content"

}

```
Example response.

```json
{
        "malicous_urls": {
        "number": 4, 
        "phishing": 1,
        "malware":2,
        "defacement":1 
        }, 
        "is_text_spam": 5,
        "is_attachment_spam":8.5  
        }
```

### /api/analysis/attachment

**POST /api/analysis**
**Request format in json**
```json
{
        "attachment":[
                {
                        "mimetype":"application/pdf",
                        "base64":"BLA BLA"
                },
                {
                        "mimetype":"application/pdf",
                        "base64":"BLA BLA"
                }
        ]

}

```
Example response.

```json
{
        "task_id":1
        }
```

Request analysis task for Email attachment content.
Wait for it 