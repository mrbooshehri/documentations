# Nextcloud Cron Job in Docker

1. Make sure you have a cron utility installed on your host, like
	 ```cronie```

1. Make sure ```crone``` sevice is enabled and running.
```bash
sudo systemctl enable cronie
sudo systemctl start cronie
```

1. Create a Cron job
```bash
crontab -e
```

1. Add the following line to the crontab file
```
*/5 * * * * docker exec --user www-data [NEXTCOUD_INSTANCE] php -f /var/www/html/cron.php
```
in my case ```NEXTCOUD_INSTANCE``` is ```nextcloud```, you could also
use container id if you wish.
