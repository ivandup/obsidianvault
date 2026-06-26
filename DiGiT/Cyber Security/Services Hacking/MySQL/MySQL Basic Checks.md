If MySQL is running, try connecting remotely using default details
```
mysql -u root -p -h 10.50.10.67
```
Try the password root

If SSL is enforced:
```
mysql -u root -p -h 10.50.10.73 --ssl=false