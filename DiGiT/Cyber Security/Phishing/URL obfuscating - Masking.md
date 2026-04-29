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



