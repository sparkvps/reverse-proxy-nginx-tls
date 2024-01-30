<h1 align="center"/>Welcome to sparkvps</h1>

<p align="center">
Easy to sell with <a href="https://github.com/sparkvps/reverse-proxy-nginx-tls/">spark vps proxy</a> easy install with few clicks
</p>


### دستورات:


نکته= برین به کلودفلر و پروکسی رو خاموش کنید و SSL/TLS رو Flexible بزارین.

# Cloudflare proxy = off + SSL/TLS = Flexible


1: سیستم را آپدیت کنید:
@ apt-get update -y && apt-get upgrade -y


2: بسته های مورد نیاز را نصب کنید:
* sudo apt install nginx certbot python3-certbot-nginx -y


3: فایل HTML را کپی کنید:
```` cp /etc/nginx/sites-available/default /etc/nginx/sites-available/YOURSITE.COM ````


4: لینک کردن:
& ln -s /etc/nginx/sites-available/YOURSITE.COM /etc/nginx/sites-enabled/


5: تغییر دایرکتوری:
cd /etc/nginx/sites-enabled && ls -la


5.1: فایل زیر را باز کنید و در قسمتی که نوشته شده listen 80 & listen [::]:80 بیاین جمله default_server رو پاک کنید.
nano /etc/nginx/sites-available/YOURSITE.COM


5.2: در قسمتی که نوشته شده server_name باید ادرس سایت خودتون رو به شکل زیر وارد کنید.
server_name YOURSITE.COM;


5.3: کد زیر رو در جای مشخص شده که در ویدیو گفته شده جایگذاری کنید یعنی در زیر location
location /ports {
        if ($http_upgrade != "websocket") {
            return 404;
        }
        location ~ /ports/\d\d\d\d\d$ {
            if ($request_uri ~* "([^/]*$)" ) {
                set $port $1;
            }
            proxy_redirect off;
            proxy_pass http://127.0.0.1:$port/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        return 404;
    }


5.4: با دستور cd برگردین به دایرکتوری روت
cd


6: حالا nginx رو ری استارت کنید.
systemctl restart nginx


7: گرفتن ssl برای دامنه
certbot --nginx -d YOURSITE.COM --register-unsafely-without-email


8: در این مرحله برین به کلودفلر و پروکسی رو روشن کنید و SSL/TLS رو روی فول بزارین.
CloudFlare = proxy on + SSL/TLS = Full 


9: در این مرحله پنل مورد نیاز خودتون رو نصب میکنید مثل پنل چینی و یا سنایی و… میتونین از ویدیو زیر که چندتا خوب هاش رو معرفی کردم هم استفاده کنید.
https://youtu.be/Om3fgIJoCh4
 و یا پنل چینی رو نصب کنید.
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)


10: و در آخر اگه فایروال دارین و فعال هستش:


برای فهمیدن فعال بودن فایروال:
ufw status
برای نصب فایروال:
apt install ufw -y
برای آزاد کردن پورت های مورد نیاز:
ufw allow PORT/tcp
برای مثال:
ufw allow 22/tcp
