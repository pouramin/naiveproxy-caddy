###### Video : لینک ویدیو یوتیوب
```

```

###### خرید دامنه از نیم چیپ: 
```
https://namecheap.pxf.io/BX7m6W
```
###### خرید دامنه سایت ایرانی: 
```
https://www.hub.shatelhost.com/aff.php?aff=290
```
###### خرید سرور از دیجیتال اوشن : 
```
https://m.do.co/c/0fb522deafa4
```
###### خرید سرور از سایت ایرانی : 
```
https://dashboard.azaronline.com/order/?aff=790
```

**If you think this project is helpful to you, you may wish to give a** 🌟

**Feel Free To Donation :** ❤️

>TRC20: ```TGTyqv2MH7dZztMvaP5PKuS9Bma8RY5Pk8```

>ETH: ```0x5b5202a54e5ce4fb25f0d886254eeb07bb088614```


#### Naive server configuration
#### Update and Upgrade Server 
```
apt-get update -y && apt-get upgrade -y
```

###### Compile and install caddy+naive
###### Install Go
```
wget https://go.dev/dl/go1.19.4.linux-amd64.tar.gz
```

###### Extract the archive file
```
sudo tar -xvf go1.19.4.linux-amd64.tar.gz
```
###### Move Dir of GO
```
sudo mv go /usr/local
```

###### Add /usr/local/go/bin to your executable PATH
```
nano ~/.profile
```
###### paste the followong script in the end of file
```
export GOPATH=$HOME/go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
```
###### load settings
```
source ~/.profile
```

###### Install caddy
```
go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest
~/go/bin/xcaddy build --with github.com/caddyserver/forwardproxy@caddy2=github.com/klzgrad/forwardproxy@naive
```

###### Caddyfile config
###### به کوچیک بزرگی حروف دقت کنید. حتما کدی فایل رو با سی بزرگ بنویسید.
###### Delete comment before save the file.
###### کامنتارو قبل اینکه فایل رو سیو کنید حذف کنید، هر کامنت شامل متن فارسی و هشتگ هستش
```
:443, nvp.pouraminam.it #آدرس دومین ثبت شده روی کلودفلر
tls example@example.com #ایمیل
route {
 forward_proxy {
   basic_auth user pass #یوزرنیم و پسورد
   hide_ip
   hide_via
   probe_resistance
  }
 #ساخت یوزر دوم سوم و... برای هر یوزر لازمه که ازاینجا تا کامنت بعدی رو کپی پیست کنید. 
 forward_proxy { 
   basic_auth user2 pass2 #یوزرنیم و پسورد
   hide_ip
   hide_via
   probe_resistance
  }
  # تا اینجا رو باید کپی پیست کنید
 reverse_proxy  shorturl.at/lBDI5  { #آدرس فیک
   header_up  Host  {upstream_hostport}
   header_up  X-Forwarded-Host  {host}
  }
}
```
###### Caddy common instructions:
###### Running caddy in the foreground
```
./caddy run
```
###### background
```
./caddy start
```
###### stopping caddy
```
./caddy stop
```

###### reloading configuration
```
./caddy reload
```

#### Custom port
###### If naive wants to use a custom port, you need to use the json configuration 
```
//قبل از سیو کردن فایل این کامنتای فارسی رو پاک کنید، هر کامنت شامل متن فارسی و دوتا اسلش قبلش هست
{
 "apps": {
   "http": {
     "servers": {
       "srv0": {
         "listen": [
           ":4431"   //پورت
         ],
         "routes": [
           {
             "handle": [
               {
                 "auth_user_deprecated": "user",   //یوزر
                 "auth_pass_deprecated": "pass",  //پسورد
                 "handler": "forward_proxy",
                 "hide_ip": true,
                 "hide_via": true,
                 "probe_resistance": {}
               }
             ]
           },
           {
             "handle": [
               {
                 "handler": "reverse_proxy",
                 "headers": {
                   "request": {
                     "set": {
                       "Host": [
                         "{http.reverse_proxy.upstream.hostport}"
                       ],
                       "X-Forwarded-Host": [
                         "{http.request.host}"
                       ]
                     }
                   }
                 },
                 "transport": {
                   "protocol": "http",
                   "tls": {}
                 },
                 "upstreams": [
                   {
                     "dial": "shorturl.at/lBDI5:443"  //آدرس فیک
                   }
                 ]
               }
             ]
           }
         ],
         "tls_connection_policies": [
           {
             "match": {
               "sni": [
                 "nvp.pouraminam.it"  //آدرس دومین ثبت شده روی کلود فلر
               ]
             },
             "certificate_selection": {
               "any_tag": [
                 "cert0"
               ]
             }
           }
         ],
         "automatic_https": {
           "disable": true
         }
       }
     }
   },
   "tls": {
     "certificates": {
       "load_files": [
         {
           "certificate": "/root/a.crt",  //پابلیک کی
           "key": "/root/a.key",   //پرایویت کی 
           "tags": [
             "cert0"
           ]
         }
       ]
     }
   }
 }
}
```
#### client configuration
```
{
  "listen": "socks://127.0.0.1:1080",
  "proxy": "https://user:pass@example.com"
}
```
