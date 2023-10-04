
![33399-icq-flower-logo-icon-vector-icon-vector-eps](https://github.com/Azumi67/Wireguard/assets/119934376/1b3f5158-69e5-48b5-952f-3042c14d0de9) **Project Overview: Wireguard Panel**   
--------------------------------------------------------------------------------------------------------------------------------


![R](https://github.com/Azumi67/Wireguard/assets/119934376/0ab61a19-de0b-492c-99f7-e3d1ad492cb0) **languages :**

[Persian](https://github.com/Azumi67/Wireguard/blob/main/README.md#%D9%BE%D9%86%D9%84-%D9%88%D8%A7%DB%8C%D8%B1%DA%AF%D8%A7%D8%B1%D8%AF)

[English](https://github.com/Azumi67/Wireguard/tree/main#-project-overview-wireguard-panel)

-----------------------------------------------------------------------------------------------
![R (1s)](https://github.com/Azumi67/Wireguard/assets/119934376/3974c679-a8a9-4046-a8a2-7f83848167c7)

- This project is a Wireguard panel that incorporates various codes collected from friends and modified by me. Since i know basic stuff in python language,i had to rely on chatgpt and my friend's code which i have studied and modified some of it and i have learned a lot.
- It is intended for educational use and i changed some codes in css,html and javascript as well. However, please note that using it is at your own risk but I would be happy if others find it useful.
- I also helped with the Wireguard tunnel script, and with the help of Opiran, who helped me a lot, we managed to create a well-rounded UDP2Raw tunnel script for this purpose.

--------------------------------------------------------------------------------------------------------
  
![R (4)](https://github.com/Azumi67/Wireguard/assets/119934376/ec77e178-ae28-4e17-bb83-61be890e330f) **FEATURES**
- This is a modified version of the original [WGDashboard](https://github.com/donaldzou/WGDashboard)
- There is a limit function for the volume of traffic based internet. [Based on Total Sent data]
- There is date and time [ I couldn't find a workable way to make it working with date only just yet, i can seperate them but it is not necessary ].
- There is a Wireguard tunnel script based on [udp2raw](https://github.com/wangyu-/udp2raw) [ipv4/ipv6] written specifically for this project.
- There are other features that are also available in the original Wireguard panel.
- There are better versions of this panel made by experienced programmers for sure, but you should spend money [ Please support them if you want a better and smoother experience].

---------------------------------------------------------------------------------------------------------------------------------------------
![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/463cb33e-2155-4846-9ff0-0e3ddc8cccc3) **1 :Installation:** 
- Before proceeding with the Wireguard panel installation, while it's optional please make sure to install the VPS-Optimizer from the [opiran](https://github.com/opiran-club) GitHub repository : 
```
apt install curl -y && bash <(curl -s https://raw.githubusercontent.com/opiran-club/VPS-Optimizer/main/optimizer.sh --ipv4)
```
-----------------------------------------------------------------------------------------------------------
![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/6e559581-834b-47e0-b754-f1a954b1d2cf) **Note: Ensure that you have opened the necessary ports for Wireguard, SSH, and Iran on your UFW firewall if you intend to create a tunnel and not losing access to your server.**

**For DIGITALOCEAN ![R (1)](https://github.com/Azumi67/Wireguard/assets/119934376/5a3f59cd-744f-4cb7-941d-90b932e18b10)
Servers, be sure to remove your private ips otherwise you might have a problem** [ there is workaround for this, but it is easier to remove your private ip]

- Make sure to change Peer Remote Endpoint to your VPS ipv4 address and in case of tunnel, change it to your tunnel ipv4 address.

----------------------------------------------------------------------------------------------------------

![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/2b1d72af-22e0-446e-a2f3-cd6b326c17c1) **Installation Steps:** [Tested on: Ubuntu 20]

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

![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/036090c7-ce99-42e9-a760-4ccc51ec20c0) **Create a sample Wireguard config file:**

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

![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/4829b09e-6978-40ad-9693-d4e3c04f2e6f)- You can make different ports, just add another wireguard interface eg : wg1 and so on.
- When adding config in wireguard panel, based on the IPs above... you will use 10.66.66.2/32, fd42:42:42::2/128 in allowed IPs section.
- You can use different IPs if you wish.
- Wireguard port here is 50820/udp [ Be sure to change it if you need another prot]
---------------------------------------------------------------------------------------------------------------------

![OIP](https://github.com/Azumi67/Wireguard/assets/119934376/8b7db842-3e4a-484c-a730-c6da4e479b62) **DOWNLOAD SECTION :** 
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

![OIP (1)](https://github.com/Azumi67/Wireguard/assets/119934376/0b63b0ea-d2b4-4b8d-9828-11bb69351946) **Dashboard Server Configuration:**

![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/338c8918-d6a8-46c9-a8de-a6e23e279dba)Create a system service file for the Wireguard dashboard:

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

![R (3)](https://github.com/Azumi67/Wireguard/assets/119934376/677d95b1-3e6e-455c-914a-36c981473c1e)  **Script for Tunnel**

- To establish a tunnel for your Wireguard server, follow the steps below:
```
 bash <(curl -Ls https://raw.githubusercontent.com/opiran-club/wgtunnel/main/udp2raw.sh --ipv4)
```
![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/74dfa7a7-6bab-4cf3-8b7b-a8202466e8ca) There are different method to establish a tunnel and additional methods for establishing a tunnel will be added to the script when time permits. **[ only for Wireguard ]**
  - **Thanks to opiran admin for helping and giving me a hand in creating the script.**
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
![R (5)](https://github.com/Azumi67/Wireguard/assets/119934376/476ef2ab-19b3-4952-b755-d05b1535ca09) **SEPCIAL THANKS & LINKS :**
------------------------------------------------------------

- Thanks to OPIRAN for helping me in creating the script and answering my questions. I've put his github link down below:
  
![circle-clipart-chain-link-9](https://github.com/Azumi67/Wireguard/assets/119934376/8fe3c454-ab0a-4b98-a4fe-21a7590a2bee)[OPIRAN Github link](https://github.com/opiran-club)

![R (6)](https://github.com/Azumi67/Wireguard/assets/119934376/f79f26bc-3c53-46aa-aa76-7b779646e9a8)[Original Author](https://github.com/donaldzou/WGDashboard)

- Thanks to Joshua for answering my questions and helping me in my endeavour and some codes.
- Thanks to OPIRAN telegram channel.
--------------------------------------------------------------------------------------------------------------------------

![youtube-131994968075841675](https://github.com/Azumi67/Wireguard/assets/119934376/58437ca2-a7b5-4e1c-8c74-30c178aa71d6)  **Video Guide for the Tunnel Script**

[![Alt Text](https://img.youtube.com/vi/VIDEO_ID/0.jpg)](https://www.youtube.com/watch?v=VIDEO_ID)


--------------------------------------------------------------------

![R (7)](https://github.com/Azumi67/Wireguard/assets/119934376/6c7f4539-30ae-4020-b498-e8be5640bf42) **Telegram Channel**

![R (6)](https://github.com/Azumi67/Wireguard/assets/119934376/7226c310-a1cd-4eb0-af60-9757b488ed38)[OPIRAN](https://github.com/opiran-club)



-------------------------------------------------------------------------
![OIP](https://github.com/Azumi67/Wireguard/assets/119934376/337e645f-36cc-4357-bbbf-7d980ad39668) **Known Bugs and workarounds**

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

![R (1s)](https://github.com/Azumi67/Wireguard/assets/119934376/19c9d840-d442-41d6-a577-ff7050fc7103)
 **مقدمه :**

این پروژه در مورد یک پنل وایرگارد است که نسخه اصلی ان به نام [WGDashboard](https://github.com/donaldzou/WGDashboard) میباشد . این پروژه که صرفا برای اموزش بوده و چون اطلاعات من در پایتون بالا نیست، دوستانم در این مسیر به من کمک کردند و بعضی از کدها توسط دوستانم تهیه شده و توسط بنده تغییر و بهینه شده است (از chatgpt هم کمک گرفته شده است) و تغییراتی در html - javascript -css داده ام. در اینده اگر ایده ای هم داشته باشم و در حد توانم باشه به این پنل اضافه خواهم کرد.


------------------------------------------------------------------------------------
![R (4)](https://github.com/Azumi67/Wireguard/assets/119934376/a9a2bca3-d6f6-47d2-8d2e-7e11f57d68aa)
**امکانات**
- داشتن حجم [بر اساس Total Data sent ]
 - داشتن تاریخ و زمان ( تلاش کردم که این دو Input از هم جدا باشند اما در حال حاضر موفق به پیاده سازی آن نشده ام.
  - تمام امکاناتی که نسخه اصلی دارد
  - پشتیبانی از زبان فارسی
  - داشتن یک اسکریپت که بر پایه [udp2raw ipv4/ipv6](https://github.com/wangyu-/udp2raw) هست و شما میتوانید به راحتی با این اسکریپت تانل را برقرار کنید و در آینده مدل های دیگر تانل را درصورت امکان به این اسکریپت اضافه خواهم کرد. با سپاس از [opiran](https://github.com/opiran-club)
  -  اگر دنبال پنل بهتر هستید حتما از برنامه نویسانی که حرفه ای اینکار را انجام میدهند و پنل بهتری را درست میکنند، حمایت کنید که مشکلی هم نداشته باشید.

-------------------------------------------------------------
    
![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/0329067d-8e9a-4842-89bf-8f6da5a7533a)
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

![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/bd7244f5-7e23-4d32-a3b0-fda0325d3a49)
**نمونه کانفیگ وایرگارد**


![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/59fbff4c-5a13-4385-8829-122025fab001)
  پورت و wg0 و eth0 در صورت لزوم تغییر دهید

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
**توجه**![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/b639ec45-65b0-4f0b-bf78-ebe821f31988)



- برای ساختن اینترفیس های بیشتر و با پورت های مختلف با همین روش بالا انجام بدید و فقط نام و پورت و ایپی رو عوض کنید
- دقت کنید برای سرور های دیجیتال اوشن ![R (1)](https://github.com/Azumi67/Wireguard/assets/119934376/348379cb-55b3-4fff-980d-6e2d31072c81) قبل از نصب، ایپی پرایوت خود را حذف کنید [ راه دیگر هم هست اما این روش هم شدنی هست]
- به صورت پیش فرض Peer Remote Endpoint بر روی یک عدد بی ربط است. حتما از داخل تنظیمات این مقدار را به ایپی 4 خارج یا سرور ایران در صورت تانل تغییر بدهید.
  - در پنل وایرگارد داخل Allowed IPs برای کاربر بر اساس ایپی انتخابی بالا ، از 10.66.66.2/32, fd42:42:42::2/128 و برای کاربر دوم از 10.66.66.3/32, fd42:42:42::3/128 استفاده میکنید.

-----------------------------------------------------------------




![OIP](https://github.com/Azumi67/Wireguard/assets/119934376/e45e553c-23c7-43e2-8856-9aec78020ec2)
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



![OIP (1)](https://github.com/Azumi67/Wireguard/assets/119934376/5455440c-200e-4f14-b979-438e44940351)
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

![R (3)](https://github.com/Azumi67/Wireguard/assets/119934376/f854fe2b-ad58-4239-a9d4-a7b3c2d11513)
**اسکریپت تانل**  

 - با این [اسکریپت](https://github.com/opiran-club) به راحتی تانل وایرگارد را برقرار کنید. این تانل [udp2raw ipv4/ipv6](https://github.com/wangyu-/udp2raw) است و بعدا اگر امکانش باشد، مدل های دیگه هم اضافه خواهد شد 

  

  ```
  bash <(curl -Ls https://raw.githubusercontent.com/opiran-club/wgtunnel/main/udp2raw.sh --ipv4)
  ```

  --------------------------------------------------------------------

  
![R (5)](https://github.com/Azumi67/Wireguard/assets/119934376/a5835d36-6496-4c2b-b134-a2a07fb22aae)
**قدردانی و لینک ها**
  
- از ادمین اپیران به خاط همکاری در اسکریپت و پاسخ دادن به سوالاتم سپاسگزارم
  
![circle-clipart-chain-link-9](https://github.com/Azumi67/Wireguard/assets/119934376/85220da8-f2f9-483f-9f06-a63af9db4070)
[گیت هاب اپیرن](https://github.com/opiran-club)

![R (6)](https://github.com/Azumi67/Wireguard/assets/119934376/9e9f3443-c884-4905-9505-0f31643bfc49)
[نسخه اصلی پنل](https://github.com/donaldzou/WGDashboard)

- از joshua دوست خوبم به خاطر پاسخ به سوالاتم و کمک کردن در بعضی کدها سپاسگزارم
- از گروه تلگرام opiranclub هم سپاسگزارم

--------------------------------------------------------------------

![youtube-131994968075841675](https://github.com/Azumi67/Wireguard/assets/119934376/9e4f9dd5-6e27-4ab0-9ac9-0089671f70ac)
**یوتیوب**

 آموزش اسکریپت :

 [![Alt Text](https://img.youtube.com/vi/videoid/0.jpg)](https://www.youtube.com/watch?v=videoid)
  
  

  -------------------------------------------------------------

  
![R (7)](https://github.com/Azumi67/Wireguard/assets/119934376/4af86570-c8dd-4877-bcee-c311d0ec2b0d)
**تلگرام**

![R (7)](https://github.com/Azumi67/Wireguard/assets/119934376/7430935f-af65-49bc-9b18-407aa06f8256)
[کانال تلگرام اپیران](https://t.me/opiranv2rayproxy)


---------------------------------------------------
![OIP](https://github.com/Azumi67/Wireguard/assets/119934376/87074fc6-ccb8-4d7d-b768-2f7de3479354)
**باگ ها و رفع اشکال**

- اگر نتوانستید کانفیگ ها یا Peers رو پاک کنید ، وایرگارد را روشن و خاموش کنید و دوباره تلاش کنید. اگر بازم موفق نشدید، پرایوت کی هر Peer رو پاک و ذخیره کنید و دوباره حذف کنید.
- ممکن است تانل شما پکت ها رو جا به جا نکند ، حتما به همون روش بالا، وایرگارد رو نصب کنید تا این مشکل برای شما پیش نیاید.

