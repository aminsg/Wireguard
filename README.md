
![33399-icq-flower-logo-icon-vector-icon-vector-eps](https://github.com/Azumi67/wire/assets/119934376/9d03f964-2c64-49c9-a2bb-57fe5343c2c0) **Project Overview: Wireguard Panel**   
--------------------------------------------------------------------------------------------------------------------------------


![R](https://github.com/Azumi67/wire/assets/119934376/0aeb213c-9b5e-4f9c-a1b6-485562fd53b8) **languages :**

[Persian](https://github.com/Azumi67/Wireguard/blob/main/README.md#%D9%BE%D9%86%D9%84-%D9%88%D8%A7%DB%8C%D8%B1%DA%AF%D8%A7%D8%B1%D8%AF)

[English](https://github.com/Azumi67/Wireguard/tree/main#-project-overview-wireguard-panel)

-----------------------------------------------------------------------------------------------
![R (1)](https://github.com/Azumi67/wire/assets/119934376/c7fa5d9e-ce33-4574-b8d2-570b9a7be8fc)       

- This project is a Wireguard panel that incorporates various codes collected from friends and modified by me. Since i know basic stuff in python language,i had to rely on chatgpt and my friend's code which i have studied and modified some of it and i have learned a lot.
- It is intended for educational use and i changed some codes in css,html and javascript as well. However, please note that using it is at your own risk but I would be happy if others find it useful.
- I also helped with the Wireguard tunnel script, and with the help of Opiran, who helped me a lot, we managed to create a well-rounded UDP2Raw tunnel script for this purpose.

--------------------------------------------------------------------------------------------------------
  
![R (4)](https://github.com/Azumi67/wire/assets/119934376/b62260ed-f8fc-4600-9eb2-c58eb6d38d4b) **FEATURES**
- This is a modified version of the original WGDashboard.
- There is a limit function for the volume of traffic based internet. [Based on Total Sent data]
- There is date and time [ I couldn't find a workable way to make it working with date only just yet, i can seperate them but it is not necessary ].
- There is a Wireguard tunnel script based on [udp2raw](https://github.com/wangyu-/udp2raw) [ipv4/ipv6] written specifically for this project.
- There are other features that are also available in the original Wireguard panel.
- There are better versions of this panel made by experienced programmers for sure, but you should spend money [ Please support them if you want a better and smoother experience].

---------------------------------------------------------------------------------------------------------------------------------------------
![R (2)](https://github.com/Azumi67/wire/assets/119934376/483c2f67-e3e9-4723-9e0a-08fe48476d02)**1 :Installation:** 
- Before proceeding with the Wireguard panel installation, while it's optional please make sure to install the VPS-Optimizer from the [opiran](https://github.com/opiran-club) GitHub repository : 
```
apt install curl -y && bash <(curl -s https://raw.githubusercontent.com/opiran-club/VPS-Optimizer/main/optimizer.sh --ipv4)
```
-----------------------------------------------------------------------------------------------------------
![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/wire/assets/119934376/d9c844dc-725c-4c71-8214-422e4aa93c63) **Note: Ensure that you have opened the necessary ports for Wireguard, SSH, and Iran on your UFW firewall if you intend to create a tunnel and not losing access to your server.**

**For DIGITALOCEAN ![R (1)](https://github.com/Azumi67/wire/assets/119934376/cb3aef3e-665f-4d73-ab42-e715dd001f7b)
 Servers, be sure to remove your private ips otherwise you might have a problem** [ there is workaround for this, but it is easier to remove your private ip]

 - Make sure to change Peer Remote Endpoint to your VPS ipv4 address and in case of tunnel, change it to your tunnel ipv4 address.

----------------------------------------------------------------------------------------------------------

![3022470](https://github.com/Azumi67/wire/assets/119934376/16adbfa2-4dc6-41e6-9408-040c1e0119da) **Installation Steps:** [Tested on: Ubuntu 20]

- Manually create a Wireguard config file, for example, at the default location: /etc/wireguard/wg0.conf. If you need additional ports, add them to the same directory (e.g., /etc/wireguard/wg1.conf).

- Run the command: 
```
$ apt install update
$ apt install wireguard
```

- Generate a private key for your Wireguard configuration:
Run the following commands:
```
wg genkey | sudo tee /etc/wireguard/server_private.key
```
- View it with :
```
cat /etc/wireguard/server_private.key
```
- Save the server_private.key to a notepad for future use.

-------------------------------------------------------------------------------------------------------------

![3022470](https://github.com/Azumi67/wire/assets/119934376/088f5d95-7795-4e5c-93cc-137da2f34f1b) **Create a sample Wireguard config file:**

Replace the placeholders with your desired configurations and save it to /etc/wireguard/wg0.conf.
Remember to replace "eth0" and "wg0" with the correct interface names.
- Command : [This is the default location]
```
nano /etc/wireguard/wg0.conf
```

```
[Interface]
Address = 10.66.66.1/24, fd42:42:42::1/64
PostUp = iptables -I INPUT -p udp --dport 50820 -j ACCEPT
PostUp = iptables -I FORWARD -i eth0 -o wg0 -j ACCEPT
PostUp = iptables -I FORWARD -i wg0 -j ACCEPT
PostUp = iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostUp = ip6tables -I FORWARD -i wg0 -j ACCEPT
PostUp = ip6tables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D INPUT -p udp --dport 50820 -j ACCEPT
PostDown = iptables -D FORWARD -i eth0 -o wg0 -j ACCEPT
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT
PostDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
PostDown = ip6tables -D FORWARD -i wg0 -j ACCEPT
PostDown = ip6tables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
ListenPort = 50820
PrivateKey = YOUR_GENERATED_PRIVATE_KEY
SaveConfig = true
```

![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/wire/assets/119934376/2f4c8256-ffee-4a56-80ac-c540e8ecf229) - You can make different ports, just add another wireguard interface eg : wg1 and so on.
- When adding config in wireguard panel, based on the IPs above... you will use 10.66.66.2/32, fd42:42:42::2/128 in allowed IPs section.
- You can use different IPs if you wish.
- Wireguard port here is 50820/udp [ Be sure to change it if you need another prot]
---------------------------------------------------------------------------------------------------------------------

![OIP](https://github.com/Azumi67/wire/assets/119934376/cb09e9ad-6414-4e18-bebc-c06530a1be8d) **DOWNLOAD SECTION :** 
- Download the necessary files from my GitHub repository to your operating system:

Run the following commands:

- English version
  
 ```
$ apt update
$ apt install git
$ apt-get -y install python3-pip
$ apt install gunicorn -y
$ cd Wireguard/WireguardEnglish/src
$ sudo chmod u+x wgd.sh
$ pip install -r requirements.txt
$ sudo ./wgd.sh install
$ sudo chmod -R 755 /etc/wireguard
$ ./wgd.sh start or ./wgd.sh restart
 ```

- Persian version
  
```
$ apt update
$ apt install git
$ apt-get -y install python3-pip
$ apt install gunicorn -y
$ cd Wireguard/WireguardPersian/src
$ sudo chmod u+x wgd.sh
$ pip install -r requirements.txt
$ sudo ./wgd.sh install
$ sudo chmod -R 755 /etc/wireguard
$ ./wgd.sh start or ./wgd.sh restart
```
- Accessing the Wireguard panel:

Open your web browser and enter the following link, replacing yourserverip with the actual IP of your server: [ yourserverip:8080 ]

---------------------------------------------------------------------------------------------------------------

![OIP (1)](https://github.com/Azumi67/wire/assets/119934376/8c889def-0944-4923-825a-c92bebc54ff2) **Dashboard Server Configuration:**

![3022470](https://github.com/Azumi67/wire/assets/119934376/d7422a38-7a97-4fe8-a444-1c826247f5b8) - Create a system service file for the Wireguard dashboard:

Run the command: 
```
$ nano /etc/systemd/system/wg-dashboard.service
```
Paste the following content into the file:
- For English version :
  
```
  [Unit]
After=network.service

[Service]
WorkingDirectory=/root/Wireguard/WireguardEnglish/src
ExecStart=/usr/bin/python3 /root/Wireguard/WireguardEnglish/src/dashboard.py
Restart=always

[Install]
WantedBy=default.target
```

- For Persian version :
  
```
[Unit]
After=network.service

[Service]
WorkingDirectory=/root/Wireguard/WireguardPersian/src
ExecStart=/usr/bin/python3 /root/Wireguard/WireguardPersian/src/dashboard.py
Restart=always

[Install]
WantedBy=default.target
```

- Enable and start the Wireguard dashboard service:

Run the following commands:
```
$ sudo chmod 664 /etc/systemd/system/wg-dashboard.service
$ sudo systemctl daemon-reload
$ sudo systemctl enable wg-dashboard.service
$ sudo systemctl start wg-dashboard.service
$ sudo systemctl status wg-dashboard.service
```

-------------------------------------------------------

![R (3)](https://github.com/Azumi67/wire/assets/119934376/5786bb38-e2fa-47dd-809d-d0246c63c55d)   **Script for Tunnel**

- To establish a tunnel for your Wireguard server, follow the steps below:
```
 bash <(curl -Ls https://raw.githubusercontent.com/opiran-club/wgtunnel/main/udp2raw.sh --ipv4)
```
  ![3022470](https://github.com/Azumi67/wire/assets/119934376/2a412b79-cdeb-4905-9cd5-805e6113ff52)  There are different method to establish a tunnel and additional methods for establishing a tunnel will be added to the script when time permits. **[ only for Wireguard ]**
  - **Thanks to opiran admin for helping and giving me a hand in creating the script.**
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
![R (5)](https://github.com/Azumi67/wire/assets/119934376/62540974-d741-475e-807a-4ce4e1579142) **SEPCIAL THANKS & LINKS :**
------------------------------------------------------------

- Thanks to OPIRAN for helping me in creating the script and answering my questions. I've put his github link down below:
  
![circle-clipart-chain-link-9](https://github.com/Azumi67/wire/assets/119934376/0d31bb2b-7d91-48d4-8057-d9ef7fd85aee) [OPIRAN Github link](https://github.com/opiran-club)

![R (6)](https://github.com/Azumi67/wire/assets/119934376/ff18689c-9c8e-4c28-9161-7defd53c4109) [Original Author](https://github.com/donaldzou/WGDashboard)

- Thanks to Joshua for answering my questions and helping me in my endeavour and some codes.
- Thanks to OPIRAN telegram channel.
--------------------------------------------------------------------------------------------------------------------------

![youtube-131994968075841675](https://github.com/Azumi67/wire/assets/119934376/099484c7-f153-449e-b277-b5308d7dba9e) **Video Guide for the Tunnel Script**

[![Alt Text](https://img.youtube.com/vi/VIDEO_ID/0.jpg)](https://www.youtube.com/watch?v=VIDEO_ID)


--------------------------------------------------------------------

![R (7)](https://github.com/Azumi67/wire/assets/119934376/72057751-2417-4bc0-8761-eb14a2d1549e) **Telegram Channel**

![R (6)](https://github.com/Azumi67/wire/assets/119934376/50d5bd58-60e7-4c5b-a61d-1600ab0d70c0)  [OPIRAN](https://github.com/opiran-club)



-------------------------------------------------------------------------
![OIP](https://github.com/Azumi67/wire/assets/119934376/33e39025-bbec-48c1-83da-aa152b227acc) **Known Bugs and workarounds**

- If you had problems deleting peers, try turning Wireguard off and back on one more time and then delete it. If that didn't work, you can manually delete the private key and click the Save button. Now try to delete it.
- If you are having problems with the dashboard panel, try using this command in the right directoy. for example for Persian version :
  ```
  $ cd Wireguard/WireguardPersian/src/
  $ ./wgd.sh restart
   ```
- I think there is a problem with the bulk feature, but i think it can be fixed.
- Make sure to install wireguard only with the above commands since it can cause the connection not sending packets.

-------------------------------------------------------------------------------------------------------------------------------------



**پنل وایرگارد**
------------------------------------------------------------

![R (1)](https://github.com/Azumi67/wire/assets/119934376/6ffd99fa-094d-4a04-ae7c-6d22c2d2dc04) **مقدمه :**

این پروژه در مورد یک پنل وایرگارد است که نسخه اصلی ان به نام WGDashboard میباشد . این پروژه که صرفا برای اموزش بوده و چون اطلاعات من در پایتون بالا نیست، دوستانم در این مسیر به من کمک کردند و بعضی از کدها توسط دوستانم تهیه شده و توسط بنده تغییر و بهینه شده است و تغییراتی در html - javascript -css داده ام. در اینده اگر ایده ای هم داشته باشم و در حد توانم باشه به این پنل اضافه خواهم کرد.


------------------------------------------------------------------------------------
![R (2)](https://github.com/Azumi67/wire/assets/119934376/239a5319-8708-40cc-abe2-13ec4e0843be)
**امکانات**
- داشتن حجم [بر اساس Total Data sent ]
 - داشتن تاریخ و زمان ( تلاش کردم که این دو Input از هم جدا باشند اما در حال حاضر موفق به پیاده سازی آن نشده ام.
  - تمام امکاناتی که نسخه اصلی دارد
  - پشتیبانی از زبان فارسی
  - داشتن یک اسکریپت که بر پایه [udp2raw ipv4/ipv6](https://github.com/wangyu-/udp2raw) هست و شما میتوانید به راحتی با این اسکریپت تانل را برقرار کنید و در آینده مدل های دیگر تانل را درصورت امکان به این اسکریپت اضافه خواهم کرد. با سپاس از [opiran](https://github.com/opiran-club)
  -  اگر دنبال پنل بهتر هستید حتما از برنامه نویسانی که حرفه ای اینکار را انجام میدهند و پنل بهتری را درست میکنند، حمایت کنید که مشکلی هم نداشته باشید.

-------------------------------------------------------------
    
![R (4)](https://github.com/Azumi67/wire/assets/119934376/180ef896-8bb5-4c95-aa52-3e507113215a)
**روش نصب قدم به قدم**

- نخست VPS optimizer گیت هاب [اپیران](https://github.com/opiran-club) را نصب کنید : 

```
apt install curl -y && bash <(curl -s https://raw.githubusercontent.com/opiran-club/VPS-Optimizer/main/optimizer.sh --ipv4)
```

- سپس وایرگارد را نصب کنید.

```
$ apt update
$ apt install wireguard
```
- پرایوت کی بسازید و در یک جا یادداشتش کنید . با دستور زیر میتوانید بسازید
  
```
wg genkey | sudo tee /etc/wireguard/server_private.key
```
- و با دستور زیر میتوانید کلیدی که ساختید را مشاهده کنید

```
cat /etc/wireguard/server_private.key
```

![3022470](https://github.com/Azumi67/wire/assets/119934376/028829e8-e70c-47e7-8848-03451c80a902)
**نمونه کانفیگ وایرگارد**


![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/wire/assets/119934376/8e1fd38b-4c5e-48b0-889f-e3babd1009cb)  پورت و wg0 و eth0 در صورت لزوم تغییر دهید

- با دستور زیر وارد مسیر کانفیگ وایرگارد بشوید. [مسیر پیش فرض است]
```
nano /etc/wireguard/wg0.conf
```

- داخلش متن زیر را کپی کنید
```
[Interface]
Address = 10.66.66.1/24, fd42:42:42::1/64
PostUp = iptables -I INPUT -p udp --dport 50820 -j ACCEPT
PostUp = iptables -I FORWARD -i eth0 -o wg0 -j ACCEPT
PostUp = iptables -I FORWARD -i wg0 -j ACCEPT
PostUp = iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostUp = ip6tables -I FORWARD -i wg0 -j ACCEPT
PostUp = ip6tables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D INPUT -p udp --dport 50820 -j ACCEPT
PostDown = iptables -D FORWARD -i eth0 -o wg0 -j ACCEPT
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT
PostDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
PostDown = ip6tables -D FORWARD -i wg0 -j ACCEPT
PostDown = ip6tables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
ListenPort = 50820
PrivateKey = YOUR_GENERATED_PRIVATE_KEY
SaveConfig = true
```
- میتوانید از ایپی های دیگری استفاده کنید.
- پورت وایرگارد در اینجا 50820 است . میتوانید پورت دیگری انتخاب کنید.
----------------------------------------------------------------
**توجه**![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/wire/assets/119934376/0789cd1d-834c-4ee4-9731-7262b7dd3d4e)

- برای ساختن اینترفیس های بیشتر و با پورت های مختلف با همین روش بالا انجام بدید و فقط نام و پورت و ایپی رو عوض کنید
- دقت کنید برای سرور های دیجیتال اوشن ![R (1)](https://github.com/Azumi67/wire/assets/119934376/4e73c1d0-9d9b-4686-94a8-d32e8eab690d) قبل از نصب، ایپی پرایوت خود را حذف کنید [ راه دیگر هم هست اما این روش هم شدنی هست]
- به صورت پیش فرض Peer Remote Endpoint بر روی یک عدد بی ربط است. حتما از داخل تنظیمات این مقدار را به ایپی 4 خارج یا سرور ایران در صورت تانل تغییر بدهید.
  - در پنل وایرگارد داخل Allowed IPs برای کاربر بر اساس ایپی انتخابی بالا ، از 10.66.66.2/32, fd42:42:42::2/128 و برای کاربر دوم از 10.66.66.3/32, fd42:42:42::3/128 استفاده میکنید.

-----------------------------------------------------------------




![OIP](https://github.com/Azumi67/wire/assets/119934376/aa2c755e-e6a8-46c1-8acf-1d061ae7587b)
**دانلود**

- پس از اینکه فایل را از گیت هاب در سیستم عامل خودتون دانلود کردید با دستورات زیر پیش نیازها را نصب کنید و پنل را اجرا کنید

```
$ apt update
$ apt install git
$ apt-get -y install python3-pip
$ apt install gunicorn -y
$ cd Wireguard/WireguardPersian/src
$ sudo chmod u+x wgd.sh
$ pip install -r requirements.txt
$ sudo ./wgd.sh install
$ sudo chmod -R 755 /etc/wireguard
$ ./wgd.sh start or ./wgd.sh restart
```

-به پنل خودتون با [serverip:8080] وارد شوید. نام کاربری و رمز عبور پنل به صورت پیش فرض admin میباشد.

  ------------------------------------------------------------



![OIP (1)](https://github.com/Azumi67/wire/assets/119934376/5a63a09c-2e3d-4a63-824e-0ab48e29b1ab)
**کانفیگ داشبورد**
  

- با دستور زیر یک سرویس درست کنید

```
$ nano /etc/systemd/system/wg-dashboard.service
```
و محتویات پایین را داخل این سرویس کپی نمایید.
```
[Unit]
After=network.service

[Service]
WorkingDirectory=/root/Wireguard/WireguardPersian/src
ExecStart=/usr/bin/python3 /root/Wireguard/WireguardPersian/src/dashboard.py
Restart=always

[Install]
WantedBy=default.target
```

- با دستورات زیر سرویس را فعال و اجرا نمایید
```
$ sudo chmod 664 /etc/systemd/system/wg-dashboard.service
$ sudo systemctl daemon-reload
$ sudo systemctl enable wg-dashboard.service
$ sudo systemctl start wg-dashboard.service
$ sudo systemctl status wg-dashboard.service
```

-----------------------------------------------------------

![R (3)](https://github.com/Azumi67/wire/assets/119934376/7063ea65-e563-4f0c-82b6-6b6f6092ea6f)
**اسکریپت تانل**  

 - با این [اسکریپت](https://github.com/opiran-club) به راحتی تانل وایرگارد را برقرار کنید. این تانل [udp2raw ipv4/ipv6](https://github.com/wangyu-/udp2raw) است و بعدا اگر امکانش باشد، مدل های دیگه هم اضافه خواهد شد 

  

  ```
  bash <(curl -Ls https://raw.githubusercontent.com/opiran-club/wgtunnel/main/udp2raw.sh --ipv4)
  ```

  --------------------------------------------------------------------

  
![R (5)](https://github.com/Azumi67/wire/assets/119934376/bb3e8e7c-7f82-4ab9-bfd9-4396bfc36982)
**قدردانی و لینک ها**
  
- از ادمین اپیران به خاط همکاری در اسکریپت و پاسخ دادن به سوالاتم سپاسگزارم
  
![circle-clipart-chain-link-9](https://github.com/Azumi67/wire/assets/119934376/47b6f314-dba6-45a5-85de-b335be2d4fa0)
[گیت هاب اپیرن](https://github.com/opiran-club)

![R (6)](https://github.com/Azumi67/wire/assets/119934376/7ee32849-5f99-41fa-b932-1d40a6c4170c)
[نسخه اصلی پنل](https://github.com/donaldzou/WGDashboard)

- از joshua دوست خوبم به خاطر پاسخ به سوالاتم و کمک کردن در بعضی کدها سپاسگزارم
- از گروه تلگرام opiranclub هم سپاسگزارم

--------------------------------------------------------------------

![youtube-131994968075841675](https://github.com/Azumi67/wire/assets/119934376/ab660d58-2458-4a56-af6e-34ec4b78fb75)
**یوتیوب**

 آموزش اسکریپت :

 [![Alt Text](https://img.youtube.com/vi/videoid/0.jpg)](https://www.youtube.com/watch?v=videoid)
  
  

  -------------------------------------------------------------

  
![R (7)](https://github.com/Azumi67/wire/assets/119934376/f8b6528b-fbbb-4308-a496-7d7fb78a70fa)
**تلگرام**

![R (6)](https://github.com/Azumi67/wire/assets/119934376/54a03b41-d63a-495d-90ef-3c10a5a52092)
[کانال تلگرام اپیران](https://t.me/opiranv2rayproxy)


---------------------------------------------------
![OIP](https://github.com/Azumi67/wire/assets/119934376/5f5e7f6d-b25f-4292-b135-474a826eb318)
**باگ ها و رفع اشکال**

- اگر نتوانستید کانفیگ ها یا Peers رو پاک کنید ، وایرگارد را روشن و خاموش کنید و دوباره تلاش کنید. اگر بازم موفق نشدید، پرایوت کی هر Peer رو پاک و ذخیره کنید و دوباره حذف کنید.
- ممکن است تانل شما پکت ها رو جا به جا نکند ، حتما به همون روش بالا، وایرگارد رو نصب کنید تا این مشکل برای شما پیش نیاید.

