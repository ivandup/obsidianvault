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
