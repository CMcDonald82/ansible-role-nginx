# ansible-role-nginx

[![Build Status](https://travis-ci.org/CMcDonald82/ansible-role-nginx.svg?branch=master)](https://travis-ci.org/CMcDonald82/ansible-role-nginx)

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
nginx_confd_directory: /etc/nginx/conf.d
```

This is the path on the server to copy the conf files for the nginx.conf into. The nginx.conf file is setup to include all files ending in \*.conf that are located in the directory at nginx_confd_dir_path.

```
nginx_sites_available_directory: /etc/nginx/sites-available
```

The path on the server to copy the serverblock files into. These files will then be symlinked to the directory at nginx_sites_enabled_directory.

```
nginx_sites_enabled_directory: /etc/nginx/sites-enabled
```

The path on the server that the serverblock files will be symlinked to and served from.

```
nginx_user: "www-data"
```

The user account used by the worker processes. This should be "www-data", the user created by the nginx install process specifically for use with nginx. That way, this user will only have access to the nginx processes so if that user 
account is compromised somehow, it is not an account that is used by other services or the entire Ubuntu system.

```
nginx_worker_processes: auto 
```

Set nginx_worker_processes to number of CPU cores, auto will try to autodetect.

```
nginx_pidfile: "/run/nginx.pid" 
```

This is the file that stores the process ID. Rarely needs changing.

```
nginx_log_path: "/var/log/nginx"
```

This is the path to where the log files will be stored. 

```
nginx_worker_connections: 768
```

Sets the maximum number of connections each worker process can open. Anything higher than this will require Unix optimizations.

```
nginx_multi_accept: on
```

Accept all new connections as they're opened.

```
nginx_confd_files: 
  - ./conf.d/http_core.conf
  - ./conf.d/http_gzip.conf
  - ./conf.d/http_log.conf
  - ./conf.d/http_proxy.conf
  - ./conf.d/http_ssl.conf
```

A list of the paths to the \*.conf files on local machine that will be copied into nginx_confd_directory on server.
See the [modules reference](https://nginx.org/en/docs/) for descriptions of all the ngx_http_*_module directives that are available for configuration.

It is recommended to make a seperate .conf file for each module you want to configure (ex. http_core.conf for configuring the directives in ngx_http_core_module)

```
nginx_sites_available_files: 
  - ./sites-available/default.conf
```

A list of the paths to the serverblock files on local machine that will be copied into nginx_sites_available_directory on server and symlinked to nginx_sites_enabled_directory. See [this guide](https://linode.com/docs/web-servers/nginx/how-to-configure-nginx/) for explanation and examples of configuring Nginx and setting up serverblock files.

It is recommended to make a separate serverblock file for each server you want to configure (ex. example.com.conf for configuring the site located at example.com)


## Example Playbook

```
- name: Setup Ubuntu server with Nginx
  hosts: all
  remote_user: "{{ remote_username }}"
  become: yes

  roles:
    - role: nginx_role
      nginx_worker_connections: 1024
      nginx_confd_files: 
        - /path/to/custom/file/http_core.conf
        - ./conf.d/http_gzip.conf
        - ./conf.d/http_log.conf
        - ./conf.d/http_proxy.conf
      nginx_sites_available_files: 
        - ./sites-available/default.conf
        - /path/to/mysite/example.com.conf
```


## Dependencies

None