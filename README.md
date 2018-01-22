# ansible-role-nginx
Ansible role for installing and configuring NGINX on an Ubuntu server


## Requirements

None

## Role Variables

The following variables with their default values are listed below.

``` 
nginx_ppa_version: stable 
```

Specifies the version of the ppa to use when installing Nginx on Ubuntu. Can be set to stable or development

```
nginx_conf_file_path: /etc/nginx/nginx.conf
```

Specifies the location on the server to copy the nginx.conf file to.

```
nginx_confd_dir_path: /etc/nginx/conf.d
```

This is the path on the server to copy the conf files for the nginx.conf into. The nginx.conf file is setup to include all files ending in \*.conf that are located in the directory at nginx_confd_dir_path.

```
nginx_sites_available_dir_path: /etc/nginx/sites-available
```

The path on the server to copy the serverblock files into. These files will then be symlinked to the directory at nginx_sites_enabled_dir_path.

```
nginx_sites_enabled_dir_path: /etc/nginx/sites-enabled
```

The path on the server that the serverblock files will be symlinked to and served from.