# SteemMention
Another mention tool for steemit is coming.

### Why To Develop It
I tried serveral tools about getting replies before.
But NO ONE is stable. So I make it again and give a 
sample way to deploy it by yourself.

### How To Deploy It
#### Requirement
* Nginx
* PHP7.1+
* Composer
* Mysql
* Python3
* [SteemPython](https://github.com/Netherdrake/steem-python)

#### Steps
1. Get the code to your host.
```
$ git pull https://github.com/ety001/steem-mention.git
```
2. Install the requirements by composer
```
$ cd steem-mention
$ composer install
```
3. Edit `Mysql connection information` and `SMTP configuration` in the `.env` file
```
DATABASE_URL=mysql://YOUR_DB_USERNAME:YOUR_DB_USER_PASS@YOUR_DB_HOST:3306/steem_mention
MAILER_URL=smtp://YOUR_MAIL_HOST:25?encryption=&auth_mode=login&username=YOUR_USERNAME&password=PASSWORD
SYS_EMAIL=notice@steem-mention.com # Set the system email
```
4. Update database schemata
```
$ php bin/console doctrine:schema:update --force
```
5. Add nginx configuration
```
server
{
  listen 80;
  server_name steem-mention.test; # Replace your domain
  root  /Users/ety001/Workspace/wwwroot/steem-mention/public; # Replace your WEB public folder

  location / {
    try_files $uri /index.php$is_args$args;
  }

  location ~ ^/index\.php(/|$)
  {
    fastcgi_pass  172.20.0.3:9000;  # Replace your php-fpm path
    fastcgi_split_path_info ^(.+\.php)(/.*)$;
    include fastcgi.conf;
    fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
    fastcgi_param DOCUMENT_ROOT $realpath_root;
  }

  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
  {
    expires      30d;
  }

  location ~ .*\.(js|css)?$
  {
    expires      12h;
  }

  access_log off;
}
```
6. Run the block watcher
```
$ export API_URL=YOUR_HOST_URL/api/block/watcher  # set the api_url in watcher
$ python3 watcher/bots.py # run the watcher
```

**Congratuations!! 📣❤️**

Now you can visit it in your browser.

### Support me
1. I'm a witness. Please vote me by steemconnect. [https://v2.steemconnect.com/sign/account-witness-vote?witness=ety001&approve=1](https://v2.steemconnect.com/sign/account-witness-vote?witness=ety001&approve=1)
2. Send me a tip to my steemit account. My steemit account is [ety001](https://steemit.com/@ety001).
