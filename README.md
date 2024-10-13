# Docker Compose Services for fresh installation of WP with MariaDB & PhpMyAdmin

```yaml
services:
  mariadb:
    image: bitnami/mariadb:latest
    container_name: mariadb
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=bn_wordpress
      - MARIADB_DATABASE=bitnami_wordpress
    volumes:
      - mariadb_data:/bitnami/mariadb
    networks:
      - wordpress-network

  wordpress:
    image: bitnami/wordpress:latest
    container_name: wordpress
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
      - WORDPRESS_DATABASE_USER=bn_wordpress
      - WORDPRESS_DATABASE_NAME=bitnami_wordpress
    ports:
      - "8081:8080"
      - "8444:8443"
    volumes:
      - ./wp_root:/bitnami/wordpress  # Local directory for WordPress code
    networks:
      - wordpress-network

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    container_name: phpmyadmin
    environment:
      - PMA_HOST=mariadb
      - PMA_PORT=3306
    ports:
      - "8082:80"
    depends_on:
      - mariadb
    networks:
      - wordpress-network

volumes:
  mariadb_data:
  wordpress_data:

networks:
  wordpress-network:
    driver: bridge


## Afterwards you can easily import the backup file by installing this plugin: https://github.com/devHardik71/all-in-one-wp-migration-unlimited

