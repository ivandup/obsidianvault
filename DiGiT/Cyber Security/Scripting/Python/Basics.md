## TXT files in directory
Lets say you can to find all txt files in a directory
```
vi test.py
```

```
import glob, os
os.chdir("/root/Desktop/Files")
for file in glob.glob("*.txt"):
	print (file)
```

Make executable:
```
chmod +x test.py
```

Run script:
```
python test.py
```

## find all txt from the root folder
```
vi test2.ph
```

```
import os
for root, dirs, files in os.walk("/"):
	for file in files:
		if file.endswith("*.txt"):
			print(os.path.join(root, file))
```
 
 Get more info, example, display all files that contain the word password:
 ```
 import os
for root, dirs, files in os.walk("/"):
	for file in files:
		if file.endswith("*.txt"):
			print(os.path.join(root, file)) as f:
				if 'password' in f.read():
					print(os.path.join(root, file))
 ```




