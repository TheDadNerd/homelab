# Enable FTP on Ubuntu

Install FTP
```
sudo apt install vsftpd
```

<br>Edit the conf file
```
sudo /etc/vsftpd.conf
```
<br>Uncomment the follow:
```
write_enable=YES
```

<br>Restart the service
```
sudo systemctl restart vsftpd.service
```