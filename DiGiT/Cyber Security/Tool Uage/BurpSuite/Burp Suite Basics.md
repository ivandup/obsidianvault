bWAPP to test brupsuite on - It can be downlaoded as a VM

In BurpSuite, check where it's listning on:
![[Pasted image 20260416064341.png]]

In your browser, set the proxy to the settings as per your config

To capture traffic, ensure that intercept is on in BurpSuite:
![[Pasted image 20260416064519.png]]

Once' it's on you can capture traffic.

# Set target Scope
To ensure that BurpSuite only captures traffic for your target, you will need to define the scope:

Enter the scope here:
![[Pasted image 20260416065121.png]]

Narrow down scope items, but clicking on the bar (pink) and selecting the options you want:
![[Pasted image 20260416065327.png]]

## Spider Settings

![[Pasted image 20260416065507.png]]

Here you can set some settings for the web spider.
Example, **Application Log**, Promt for guidance, will ask you for loging details and will not try to login

![[Pasted image 20260416065647.png]]

## Payloads
If you want to do any payloads:
![[Pasted image 20260416065826.png]]

You can specify payloads here, or change details, such as username/password:

![[Pasted image 20260416065925.png]]


You can do brute force for passwords, example here:
![[Pasted image 20260416070027.png]]

## Repeater
The repeater is to see what the response from the website is, example, changing the password and hit the "Go" button:
![[Pasted image 20260416070154.png]]