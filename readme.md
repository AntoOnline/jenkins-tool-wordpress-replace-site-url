# Pipeline for WordPress URL Replacement

This pipeline script replaces the URLs of a WordPress site from an old URL to a new URL. It uses MySQL to perform the replacement and requires the following parameters:

- `OLD_URL`: The old site URL to be replaced
- `NEW_URL`: The new site URL to replace the old URL
- `MYSQL_USER`: The MySQL user to authenticate with
- `MYSQL_PASSWORD`: The password for the MySQL user
- `MYSQL_DATABASE`: The name of the MySQL database to perform the replacement on
- `MYSQL_HOST`: The hostname of the MySQL server
- `TABLE_PREFIX`: The WordPress table prefix (default: `wp_`)

## Pipeline Stages

### Check MySQL client

This stage checks if the MySQL client is installed on the machine and installs it if it's not available. It currently supports only Linux operating systems.

### Replace URL

This stage replaces the old URL with the new URL in the WordPress database using MySQL queries. It replaces the URLs in the following tables:

- `wp_options`
- `wp_posts`
- `wp_postmeta`

It uses the parameters provided in the pipeline to connect to the MySQL server and authenticate the user.
