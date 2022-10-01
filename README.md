# Laravel Nginx Subproject

Run multiple Laravel application with multiple PHP versions under one domain with this nginx subproject/subdirectory configuration.

## Requirements

- PHP
- Nginx

## Instructions

### 1. Copy Projects

Copy the project directory to `/var/www`.

### 2. Base Configuration (Once)

Run the following commands by replacing `<domain-name>` to your domain name and `<php-version>` to installed PHP version. If domain is not available, use `_` instead. However, domain name is required for setting up HTTPS/SSL.

```bash
sudo curl https://raw.githubusercontent.com/rahulhaque/laravel-nginx-subproject/master/default.conf -o /etc/nginx/sites-available/<domain-name>

sudo sed -i 's/:server_name/<domain-name>/g;s/:php_version/<php-version>/g' /etc/nginx/sites-available/<domain-name>
```

Create `subproject` directory in `/etc/nginx/sites-available`. This is where all the subproject configuration will live.

```bash
sudo mkdir -p "/etc/nginx/sites-available/subprojects"
```

### 3. Subproject Configuration

For each subproject, run the following commands by replacing `<app-name>` to subproject name and `<php-version>` to installed PHP version.

```bash
sudo curl https://raw.githubusercontent.com/rahulhaque/laravel-nginx-subproject/master/subproject.conf -o /etc/nginx/sites-available/subprojects/<app-name>

sudo sed -i 's/:subproject_name/<app-name>/g;s/:php_version/<php-version>/g' /etc/nginx/sites-available/subprojects/<app-name>
```

### 4. Go Live

```bash
# Enable new configuration
sudo ln -s /etc/nginx/sites-available/<domain-name> /etc/nginx/sites-enabled/<domain-name>

# Verify nginx configuration
sudo nginx -t

# Restart nginx
sudo systemctl restart nginx
```

Now, go to `http://<domain-name>/<app-name>` to see your app running.

## References

- [PHP Apps in a Subdirectory in Nginx](https://serversforhackers.com/c/nginx-php-in-subdirectory)
