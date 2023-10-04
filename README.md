
![33399-icq-flower-logo-icon-vector-icon-vector-eps](https://github.com/Azumi67/Wireguard/assets/119934376/8f6dfee6-041a-4602-b8c8-e7c9208cbfbb) **Project Overview: Wireguard Panel**   
--------------------------------------------------------------------------------------------------------------------------------


![R](https://github.com/Azumi67/Wireguard/assets/119934376/4fc5260c-551b-4a3c-9fcb-f7ab021db010) **languages :**

[Persian](https://github.com/Azumi67/Wireguard/blob/main/README.md#%D9%BE%D9%86%D9%84-%D9%88%D8%A7%DB%8C%D8%B1%DA%AF%D8%A7%D8%B1%D8%AF)

[English](https://github.com/Azumi67/Wireguard/tree/main#-project-overview-wireguard-panel)

-----------------------------------------------------------------------------------------------
![R (1s)](https://github.com/Azumi67/Wireguard/assets/119934376/932d8876-79bd-4e63-a543-e125aae4314d)
    
- This project is a Wireguard panel that incorporates various codes collected from friends and modified by me. Since i know basic stuff in python language,i had to rely on chatgpt and my friend's code which i have studied and modified some of it and i have learned a lot.
- It is intended for educational use and i changed some codes in css,html and javascript as well. However, please note that using it is at your own risk but I would be happy if others find it useful.
- I also helped with the Wireguard tunnel script, and with the help of Opiran, who helped me a lot, we managed to create a well-rounded UDP2Raw tunnel script for this purpose.

--------------------------------------------------------------------------------------------------------
  
![R (4)](https://github.com/Azumi67/Wireguard/assets/119934376/77f22327-7f56-481a-a74b-e8a2aaceab95) **FEATURES**
- This is a modified version of the original WGDashboard.
- There is a limit function for the volume of traffic based internet. [Based on Total Sent data]
- There is date and time [ I couldn't find a workable way to make it working with date only just yet, i can seperate them but it is not necessary ].
- There is a Wireguard tunnel script based on [udp2raw](https://github.com/wangyu-/udp2raw) [ipv4/ipv6] written specifically for this project.
- There are other features that are also available in the original Wireguard panel.
- There are better versions of this panel made by experienced programmers for sure, but you should spend money [ Please support them if you want a better and smoother experience].

---------------------------------------------------------------------------------------------------------------------------------------------
![R (2)](https://github.com/Azumi67/Wireguard/assets/119934376/1ae84236-0fe7-47f6-afb9-5b4d126db772) **1 :Installation:** 
- Before proceeding with the Wireguard panel installation, while it's optional please make sure to install the VPS-Optimizer from the [opiran](https://github.com/opiran-club) GitHub repository : 
```
apt install curl -y && bash <(curl -s https://raw.githubusercontent.com/opiran-club/VPS-Optimizer/main/optimizer.sh --ipv4)
```
-----------------------------------------------------------------------------------------------------------
![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/1a79b9cb-0d86-4edf-9d17-b091b4015fc3)**Note: Ensure that you have opened the necessary ports for Wireguard, SSH, and Iran on your UFW firewall if you intend to create a tunnel and not losing access to your server.**

**For DIGITALOCEAN ![R (1)](https://github.com/Azumi67/Wireguard/assets/119934376/2a3fc92d-5011-4ba0-b06c-d555ce875f41)
Servers, be sure to remove your private ips otherwise you might have a problem** [ there is workaround for this, but it is easier to remove your private ip]

- Make sure to change Peer Remote Endpoint to your VPS ipv4 address and in case of tunnel, change it to your tunnel ipv4 address.

----------------------------------------------------------------------------------------------------------

![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/00d3ea3a-46e1-4777-82e5-f68cdcc45603)**Installation Steps:** [Tested on: Ubuntu 20]

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

![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/11755ebe-cdbf-4e77-973e-703a4f3476ef)**Create a sample Wireguard config file:**

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

![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/51495c15-7aba-457f-865b-d6560d21464f)- You can make different ports, just add another wireguard interface eg : wg1 and so on.
- When adding config in wireguard panel, based on the IPs above... you will use 10.66.66.2/32, fd42:42:42::2/128 in allowed IPs section.
- You can use different IPs if you wish.
- Wireguard port here is 50820/udp [ Be sure to change it if you need another prot]
---------------------------------------------------------------------------------------------------------------------

![OIP](https://github.com/Azumi67/Wireguard/assets/119934376/cd663bdd-c27e-4cce-9f55-8b9b9778b5ac)**DOWNLOAD SECTION :** 
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

![OIP (1)](https://github.com/Azumi67/Wireguard/assets/119934376/5af063d8-7c47-43e6-bc2e-6661b7061137) **Dashboard Server Configuration:**

![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/22f876cc-d88c-4a03-af7a-4caf7caac620) Create a system service file for the Wireguard dashboard:

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

![R (3)](https://github.com/Azumi67/Wireguard/assets/119934376/67d0d159-a749-492e-94eb-3ffe76c51134)  **Script for Tunnel**

- To establish a tunnel for your Wireguard server, follow the steps below:
```
 bash <(curl -Ls https://raw.githubusercontent.com/opiran-club/wgtunnel/main/udp2raw.sh --ipv4)
```
  ![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/86f18cd9-64dc-4217-90fb-df2b21c6029b) There are different method to establish a tunnel and additional methods for establishing a tunnel will be added to the script when time permits. **[ only for Wireguard ]**
  - **Thanks to opiran admin for helping and giving me a hand in creating the script.**
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
![R (5)](https://github.com/Azumi67/Wireguard/assets/119934376/dd265542-5bcb-4d8a-b7ff-4f3100627635) **SEPCIAL THANKS & LINKS :**
------------------------------------------------------------

- Thanks to OPIRAN for helping me in creating the script and answering my questions. I've put his github link down below:
  
![circle-clipart-chain-link-9](https://github.com/Azumi67/Wireguard/assets/119934376/1bc19d24-86eb-46f9-b384-b05b3d5657f9) [OPIRAN Github link](https://github.com/opiran-club)

![R (6)](https://github.com/Azumi67/Wireguard/assets/119934376/72fbc10c-81fa-4cf3-b623-22b1759a6da6) [Original Author](https://github.com/donaldzou/WGDashboard)

- Thanks to Joshua for answering my questions and helping me in my endeavour and some codes.
- Thanks to OPIRAN telegram channel.
--------------------------------------------------------------------------------------------------------------------------

![youtube-131994968075841675](https://github.com/Azumi67/Wireguard/assets/119934376/d42ab263-7351-4493-b0f6-342bacc4b827) **Video Guide for the Tunnel Script**

[![Alt Text](https://img.youtube.com/vi/VIDEO_ID/0.jpg)](https://www.youtube.com/watch?v=VIDEO_ID)


--------------------------------------------------------------------

![R (7)](https://github.com/Azumi67/Wireguard/assets/119934376/ceb5daca-b4e4-49f9-befa-fbebc2454d8d) **Telegram Channel**

![R (6)](https://github.com/Azumi67/Wireguard/assets/119934376/5e96c932-b1f5-44ff-8594-d1c5c1e1d49b) [OPIRAN](https://github.com/opiran-club)



-------------------------------------------------------------------------
![OIP](https://github.com/Azumi67/Wireguard/assets/119934376/7ec0549f-cb11-4ee9-bfcb-d75a6d698e14) **Known Bugs and workarounds**

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

![R (1s)](https://github.com/Azumi67/Wireguard/assets/119934376/66880701-806d-4d74-aefc-c27b3ec3a29b)
 **مقدمه :**

این پروژه در مورد یک پنل وایرگارد است که نسخه اصلی ان به نام WGDashboard میباشد . این پروژه که صرفا برای اموزش بوده و چون اطلاعات من در پایتون بالا نیست، دوستانم در این مسیر به من کمک کردند و بعضی از کدها توسط دوستانم تهیه شده و توسط بنده تغییر و بهینه شده است و تغییراتی در html - javascript -css داده ام. در اینده اگر ایده ای هم داشته باشم و در حد توانم باشه به این پنل اضافه خواهم کرد.


------------------------------------------------------------------------------------
![R (4)](https://github.com/Azumi67/Wireguard/assets/119934376/6fae1cfc-5e5c-4b78-96b2-89ba8566e953)
**امکانات**
- داشتن حجم [بر اساس Total Data sent ]
 - داشتن تاریخ و زمان ( تلاش کردم که این دو Input از هم جدا باشند اما در حال حاضر موفق به پیاده سازی آن نشده ام.
  - تمام امکاناتی که نسخه اصلی دارد
  - پشتیبانی از زبان فارسی
  - داشتن یک اسکریپت که بر پایه [udp2raw ipv4/ipv6](https://github.com/wangyu-/udp2raw) هست و شما میتوانید به راحتی با این اسکریپت تانل را برقرار کنید و در آینده مدل های دیگر تانل را درصورت امکان به این اسکریپت اضافه خواهم کرد. با سپاس از [opiran](https://github.com/opiran-club)
  -  اگر دنبال پنل بهتر هستید حتما از برنامه نویسانی که حرفه ای اینکار را انجام میدهند و پنل بهتری را درست میکنند، حمایت کنید که مشکلی هم نداشته باشید.

-------------------------------------------------------------
    
![R (2)](https://github.com/Azumi67/Wireguard/assets/119934376/413c9792-4213-4c72-b45a-1c1eefc342b9)
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

![3022470](https://github.com/Azumi67/Wireguard/assets/119934376/03a25d27-d2ab-4c74-a638-8421778594e5)
**نمونه کانفیگ وایرگارد**


![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/5437f6d5-89c0-4419-9a25-2d03f26441b7)
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
**توجه**![Exclamation-Mark-PNG-Clipart](https://github.com/Azumi67/Wireguard/assets/119934376/4f36da97-6670-4af7-bc10-2ceffe414692)


- برای ساختن اینترفیس های بیشتر و با پورت های مختلف با همین روش بالا انجام بدید و فقط نام و پورت و ایپی رو عوض کنید
- دقت کنید برای سرور های دیجیتال اوشن ![R (1)](https://github.com/Azumi67/Wireguard/assets/119934376/7eb7f6f1-ebef-4b0e-928c-58c5ac2d50a3) قبل از نصب، ایپی پرایوت خود را حذف کنید [ راه دیگر هم هست اما این روش هم شدنی هست]
- به صورت پیش فرض Peer Remote Endpoint بر روی یک عدد بی ربط است. حتما از داخل تنظیمات این مقدار را به ایپی 4 خارج یا سرور ایران در صورت تانل تغییر بدهید.
  - در پنل وایرگارد داخل Allowed IPs برای کاربر بر اساس ایپی انتخابی بالا ، از 10.66.66.2/32, fd42:42:42::2/128 و برای کاربر دوم از 10.66.66.3/32, fd42:42:42::3/128 استفاده میکنید.

-----------------------------------------------------------------




![OIP](https://github.com/Azumi67/Wireguard/assets/119934376/31f28960-6617-4d4c-b917-9f32e7f2fd0c)
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



![OIP (1)](https://github.com/Azumi67/Wireguard/assets/119934376/3b6630c2-f93d-4ca8-b629-050739ac7794)
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

![R (3)](https://github.com/Azumi67/Wireguard/assets/119934376/141e7698-b173-498a-a26e-109c11643b7c)
**اسکریپت تانل**  

 - با این [اسکریپت](https://github.com/opiran-club) به راحتی تانل وایرگارد را برقرار کنید. این تانل [udp2raw ipv4/ipv6](https://github.com/wangyu-/udp2raw) است و بعدا اگر امکانش باشد، مدل های دیگه هم اضافه خواهد شد 

  

  ```
  bash <(curl -Ls https://raw.githubusercontent.com/opiran-club/wgtunnel/main/udp2raw.sh --ipv4)
  ```

  --------------------------------------------------------------------

  
![R (5)](https://github.com/Azumi67/Wireguard/assets/119934376/571cbd38-6d8d-4c14-8e3e-94d2b3ad093d)
**قدردانی و لینک ها**
  
- از ادمین اپیران به خاط همکاری در اسکریپت و پاسخ دادن به سوالاتم سپاسگزارم
  
![circle-clipart-chain-link-9](https://github.com/Azumi67/Wireguard/assets/119934376/a5206b06-2abc-44bc-bbd0-cca8861eb3aa)
[گیت هاب اپیرن](https://github.com/opiran-club)

![R (6)](https://github.com/Azumi67/Wireguard/assets/119934376/2ef26df6-7164-4f1f-bd84-715c1b47b387)
[نسخه اصلی پنل](https://github.com/donaldzou/WGDashboard)

- از joshua دوست خوبم به خاطر پاسخ به سوالاتم و کمک کردن در بعضی کدها سپاسگزارم
- از گروه تلگرام opiranclub هم سپاسگزارم

--------------------------------------------------------------------

![youtube-131994968075841675](https://github.com/Azumi67/Wireguard/assets/119934376/cff6f4a9-9252-4125-a62d-2d30009bea59)
**یوتیوب**

 آموزش اسکریپت :

 [![Alt Text](https://img.youtube.com/vi/videoid/0.jpg)](https://www.youtube.com/watch?v=videoid)
  
  

  -------------------------------------------------------------

  
![R (7)](https://github.com/Azumi67/Wireguard/assets/119934376/60c95881-2a5d-4fa7-ac40-a897bcb3c2f5)
**تلگرام**

![R (6)](https://github.com/Azumi67/Wireguard/assets/119934376/d0f68a86-f60e-4ff9-bab4-649a09b49c0c)
[کانال تلگرام اپیران](https://t.me/opiranv2rayproxy)


---------------------------------------------------
![OIP](https://github.com/Azumi67/Wireguard/assets/119934376/0f6a18fd-5255-4376-9b96-dcb60065ae8d)
**باگ ها و رفع اشکال**

- اگر نتوانستید کانفیگ ها یا Peers رو پاک کنید ، وایرگارد را روشن و خاموش کنید و دوباره تلاش کنید. اگر بازم موفق نشدید، پرایوت کی هر Peer رو پاک و ذخیره کنید و دوباره حذف کنید.
- ممکن است تانل شما پکت ها رو جا به جا نکند ، حتما به همون روش بالا، وایرگارد رو نصب کنید تا این مشکل برای شما پیش نیاید.

