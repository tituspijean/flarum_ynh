# Flarum for YunoHost

[![Install Hubzilla with YunoHost](https://install-app.yunohost.org/install-with-yunohost.png)](https://install-app.yunohost.org/?app=flarum) [![Integration level](https://dash.yunohost.org/integration/flarum.svg)](https://dash.yunohost.org/appci/app/flarum)

[Flarum](http://flarum.org/), an open-source forum software, packaged for [YunoHost](https://yunohost.org/), a self-hosting server operating server.

**Shipped version:**  0.1.0-beta.7

![](http://flarum.org/img/screenshot_2x.png)

## Features

- All Flarum features, see its [documentation](http://flarum.org/docs/)
- SSOwat integration through a [dedicated extension](https://github.com/tituspijean/flarum-ext-auth-ssowat).

## Installation
This Flarum package can be installed
- through YunoHost's webadmin, in the Community listing
- through YunoHost's CLI: `yunohost app install https://github.com/YunoHost-Apps/flarum_ynh`.

Required parameters are :
- `domain`
- `path`
- `admin`, among the YunoHosts users
- `public`, *true* by default, for guests to read the forum
- `title` of the forum
- `language` can be English (by default), French, and German. Other languages installable after installation as any other extensions
- `bazaar_extension` to install the extension marketplace (*false* by default), to avoid using the command line to add new extensions.

After installation, simply open your browser to Flarum's page. First loading may be a bit longer as assets are generated.

## Troubleshooting

### `Low memory` errors
A swapfile will enable your system to extend its limited memory through its disk capacity. The following commands will create a 1 GB swapfile.
```
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1024000
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

Then add this line in `/etc/fstab`:
```
/swapfile none swap sw 0 0
```

Reboot the system and try the installation again.

### `Timeout` errors
Some users have reported a successful installation, but get a blank page due to a `timeout` on a PHP script that prepares the forum assests (`Minify.php`, notably).
In `/etc/php/7.0/fpm/conf.d/20-{APPID}.ini`, you can increase the `max_execution_time` and `max_input_time` limits (both values are in seconds if nothing is specified).
Reload PHP-FPM with `sudo service php7.0-fpm reload`.

### Upload limit
If you are facing an error while uploading large files into the forum, PHP may be limiting file upload.
In `/etc/php/7.0/fpm/conf.d/20-{APPID}.ini`, you can uncomment (remove `;` at the beginning of the line) and increase the values of `upload_max_filesize` and `post_max_size` (both values are in bytes).
Reload PHP-FPM with `sudo service php7.0-fpm reload`.
