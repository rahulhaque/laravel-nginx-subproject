# Laravel Nginx Subproject

Run multiple Laravel application with multiple PHP versions under one domain with this nginx subproject/subdirectory configuration.

## Requirements

- PHP
- Nginx

## Instructions

### 1. Copy Projects

Copy the project directories inside `/var/www` directory. Let's assume there is a project with the directory name of `app-one`. We will configure the server for `/var/www/app-one`.

### 2. Set Base Configuration (Once)

Copy the `default.conf` file to `/etc/nginx/sites-available` directory and rename it as same as your domain name. The below command will do both for you.

```bash
sudo curl https://raw.githubusercontent.com/rahulhaque/laravel-nginx-subproject/master/default.conf -o /etc/nginx/sites-available/<your-domain-name>
```

Open the configuration file and change the `server_name` to your domain name (required for setting up SSL/HTTPS) and PHP version to your desired one.

### 3. Set Subproject Configuration (For each subproject)

Create the subproject configuration directory inside `/etc/nginx/sites-available`.

```bash
sudo mkdir -p "/etc/nginx/sites-available/subprojects"
```

Copy the `subproject.conf` file to the newly created directory for each subproject. Rename the file as the subproject name you wish to serve. Subproject directory name should match project name inside `/var/www`.

```bash
sudo curl https://raw.githubusercontent.com/rahulhaque/laravel-nginx-subproject/master/subproject.conf -o /etc/nginx/sites-available/subprojects/app-one

# Replace the namespace with your subproject name
sudo sed -i 's/:subproject_name/app-one/' /etc/nginx/sites-available/subprojects/app-one
```

Open the newly generated configuration file `app-one` and change the PHP version if required.

### 4. Go Live

```bash
# Enable new configuration
sudo ln -s /etc/nginx/sites-available/<your-domain-name> /etc/nginx/sites-enabled/<your-domain-name>

# Check if nginx configuration is correct
sudo nginx -t

# Restart nginx
sudo systemctl restart nginx
```

Now, go to `http://<your-domain-name>/app-one` to see your app running.

## References

- [PHP Apps in a Subdirectory in Nginx](https://serversforhackers.com/c/nginx-php-in-subdirectory)
