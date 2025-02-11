# Systemd Autorun

[Home](../README.md)

### 1. Create systemd file

Create file on ```/etc/systemd/system/example.service```

```
[Unit]
Description=My Binary Service
After=network.target

[Service]
ExecStart=/path/to/your/binary
WorkingDirectory=/path/to/your/binary_directory
Restart=always
User=myuser
Group=mygroup
StandardOutput=append:/var/log/mybinary.log
StandardError=append:/var/log/mybinary.log

[Install]
WantedBy=multi-user.target
```

### 2. Run these following command
```bash
systemctl daemon-reload
systemctl enable example.service
systemctl start example.service
```