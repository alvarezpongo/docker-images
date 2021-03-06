Magento 2 MySQL images
======================
![MySQL for Magento 2 status](https://img.shields.io/github/workflow/status/ajardin/docker-images/MySQL%20for%20Magento%202%20images?style=for-the-badge)
![MySQL for Magento 2 pulls](https://img.shields.io/docker/pulls/ajardin/magento2-mysql?style=for-the-badge)

✨ Features
-----------
* Custom configuration (`max_allowed_packet` at `256M`)

Available versions: `ajardin/magento2-mysql:5.7` and `ajardin/magento2-mysql:8.0`.

🚀 Usage
--------
```yaml
services:
# [...]
  mysql:
    image: ajardin/magento2-mysql:latest
    env_file: ${PROJECT_LOCATION}/var/docker/.env
    ports:
      - 3306:3306
    volumes:
      - mysql:/var/lib/mysql
    tty: true
# [...]
volumes:
  mysql: {}
```
