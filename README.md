# Docker for Bolt CMS - Headless version

This is a Docker image for Bolt CMS.

## Setup

1. Clone this repository and cd into it.
2. Extract `boltcms.tar.gz` into `docker/boltcms/` directory:

```bash
tar -xvzf docker/boltcms.tar.gz -C docker/boltcms/
```

3. Rename `docker/.env.example` to `docker/.env` and set the environment variables.
4. Edit `docker/boltcms/.env` and set the environment variables for `APP_SECRET` and `DATABASE_URL`.
5. Change into the `docker` directory and run `docker compose build` to build the docker image.
6. Run `docker compose up -d` to start the containers.
7. Get the container name of the `boltcms-app` container by running `docker compose ps`.
8. Enter the `boltcms-app` container by running the following command:

```bash
docker exec -it <CONTAINER-NAME> bash
```
9. Run the following command in the container to setup Bolt CMS:

```bash
bin/console bolt:setup
```
Follow the instructions.

10. Set file system permissions. Make sure the Apache i.e. `www-data` user has write permissions to bolt directories with e.g.:

```bash
chown -R www-data:www-data /var/www/html/var
chown -R www-data:www-data /var/www/html/config
chown -R www-data:www-data /var/www/html/public/files
chown -R www-data:www-data /var/www/html/public/theme
chown -R www-data:www-data /var/www/html/public/thumbs
```

11. Visit `http://<YOUR_IP>:<BOLTCMS_PORT>/bolt` in your browser and login with the credentials you set in step 9.
12. You may want to proxy the `boltcms-app` and `PHP-MyAdmin` containers with a web server like Nginx or Apache.
For Apache e.g.:

```apache
ProxyPass /phpmyadmin/  http://localhost:8080/
ProxyPassReverse /phpmyadmin/ http://localhost:8080/

ProxyPass /  http://localhost:8000/
ProxyPassReverse / http://localhost:8000/
```

13. Update Bolt CMS with composer by running the following command in the `boltcms-app` container:

```bash
composer update
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.