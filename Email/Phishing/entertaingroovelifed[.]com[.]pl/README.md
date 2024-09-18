<div align="center">
  
# :fishing_pole_and_fish: entertaingroovelifed[.]com[.]pl :fishing_pole_and_fish:

## :page_facing_up: Summary :page_facing_up:

<div align="left">
  
The attacker is attempting to obtain Office 365 credentials for further exploitation. To avoid email security, the attacker utilizes evasion techniques such as QR codes, CAPTCHA, and geo-blocking.

<div align="center">

## :mag_right: Indicators of Compromise :mag:

| Type | Value |
| --- | --- |
| Domain | banhtrangutbinh[.]com |
| Domain | entertaingroovelifed[.]com[.]pl |
| Domain | urbantechentertainmentfe[.]ru |
| IP | 191[.]101[.]80[.]248 |
| URL | hxxps[://]banhtrangutbinh[.]com/image/catalog/vqmod/arull[.]php?7120797967704b536932307464507a53744a4c53704a7a4d784c4c3872504c30764e7955784c5464464c7a732f564b386a524c3357717a4376564277413d |
| URL | hxxps[://]entertaingroovelifed[.]com[.]pl/uBynu/#5 |
| URL | hxxps[://]urbantechentertainmentfe[.]ru/514[.]php |

## :checkered_flag: Attack Progression :racing_car:

<div align="left">
  
1) Attacker compromises a legitimate 3rd-party vendor of the victim's company.
2) Attacker crafts email to be sent to the victim. The attacker uses a QR code to prevent the scanning and rewriting of the URL by any email security services used by the victim's company.

![entertaingroovelifed_com_pl (Email)](https://github.com/user-attachments/assets/873a233b-6365-476f-9cea-3a5c59ac5814)

3) Attacker sends emails to victims in alphabetical order.
4) Victim receives email and scans the QR code with his/her phone, which is most likely not protected by the company's security software such as web proxies or AV.
5) QR code is for the following site: _hxxps[://]banhtrangutbinh[.]com/image/catalog/vqmod/arull[.]php?7120797967704b536932307464507a53744a4c53704a7a4d784c4c3872504c30764e7955784c5464464c7a732f564b386a524c3357717a4376564277413d_
6) The site checks the geolocation of the victim's IP address. If the victim is not in the United States, the site fails to load. If the victim's IP is in the United States, the attack proceeds to the next step.

> Based on later connections and previous similar attacks utilizing the same fake webpage, this appears to be a mistake by the attacker ([Triage scan of a previous attack with the same webpage](https://tria.ge/240812-yzgbwsxglm/behavioral1); Triage scans from the EU normally but offer a private US based service if desired). It appears the attacker meant for the non-United States connections to go to _hxxps[://]urbantechentertainmentfe[.]ru/514[.]php_, which is a fake car website with only one page. The attacker's hope appears to be to avoid detection for a longer period, so that non-US victims would not flag the emails as malicious, causing security tools to block the sender and URLs. However, not much effort was put into this front since the redirect is broken and none of the links on the home page take you anywhere other than the home page. In addition, the site is not related to the original request of the email, which would probably have caused suspicion if the site had been working properly.

![urbantechentertainmentfe_ru](https://github.com/user-attachments/assets/f46de7f5-4280-4dc8-b9da-1e15dc22ad5c)

7) The victim is redirected to _hxxps[://]entertaingroovelifed[.]com[.]pl/uBynu/#5_.
8) Victim is required to perform a CloudFlare CAPTCHA.

> CAPTCHA, especially CloudFlare's CAPTCHA, have been observed by me for most phishing attacks because of their protection against automated scanners such as VirusTotal and URLScan.io.

![entertaingroovelifed_com_pl (CAPTCHA)](https://github.com/user-attachments/assets/2699f7f9-3444-4406-95a1-9661792d5a54)

9) Victim is brought to a fake Office 365 login page.
> None of the other buttons on this page work if clicked. Also, the SharePoint logo and background are not shown when logging into a legitimate SharePoint site.

![entertaingroovelifed_com_pl (Login) 01](https://github.com/user-attachments/assets/77156d15-61be-4402-a66a-f9ea6e984933)

10) Victim types in their email, and then is requested to enter their password on the same page.
> The site does not check to see if the email is correct before proceeding to the password, nor does it reload the page to look like the victim's SSO login page if an SSO is used at the victim's company. It also has 2 broken words (i.e. _proceed_ and _verification_) which will cause suspicion.

![entertaingroovelifed_com_pl (Login) 02](https://github.com/user-attachments/assets/728f439c-fe2c-45c5-8a6a-290112e70948)

11) The site checks the victim's credentials by attempting to login to their account from _191[.]101[.]80[.]248_ using the following user agent: _Mozilla/5.0 (Windows NT 10.0; Win64; x64; WebView/3.0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36 Edge/18.22621_
> According to [User-Agents.net](https://user-agents.net/string/mozilla-5-0-windows-nt-10-0-win64-x64-webview-3-0-applewebkit-537-36-khtml-like-gecko-chrome-70-0-3538-102-safari-537-36-edge-18-19044), this is a Windows 10 device using Microsoft Edge. I wasn't able to find a specific release date for this version of Edge, but the Chrome version listed was released on November 9, 2018, so it appears the attacker is running a version of Windows 10 from around this time.

![entertaingroovelifed_com_pl (Login) 03](https://github.com/user-attachments/assets/a91f9533-2692-4b69-b17f-c212ecc31a84)

12) If the attacker can gain access, the attack would progress from here, and most likely end in the attacker targeting new victims using the same methods once they have achieved their goal(s) inside the victim's account/company.

<div align="center">

## :bulb: OSINT Insights :bulb:

<div align="left">
  
1) According to [IP2Proxy](https://www.ip2proxy.com/), _191[.]101[.]80[.]248_ is a data center out of the UK hosted with Hostinger International Limited.

![IP2Proxy (191_101_80_248) 01](https://github.com/user-attachments/assets/c013a878-5776-4ad3-8a3b-b3bfe8e678db)

![IP2Proxy (191_101_80_248) 02](https://github.com/user-attachments/assets/0279be96-566b-4fe7-aee3-acaaf3b28555)

2) A reverse IP lookup using [SecurityTrails](https://securitytrails.com/list/ip/191.101.80.248) shows 2 subdomains using this IP address: _0x1[.]star0x1[.]com_ and _bot[.]star0x1[.]com_

> For data centers, multiple domains with no relation may use the same IP address for their DNS A record (ex. AWS). However, this IP has only 2 subdomains which are related, most likely indicating that the attacker is the only one utilizing this IP address and the subsequent subdomains.

![SecurityTrails (191_101_80_248) 01](https://github.com/user-attachments/assets/88f4e542-3092-4158-a34e-751f8e5f1107)

3) Although this IP and these subdomains are not on [Shodan](https://www.shodan.io/), the domain _star0x1[.]com_ is used as part of the hostname for a server at _103[.]113[.]71[.]207_, which belongs to Stark Industries Solutions. The server's hostname according to its response on port 25 is _WIN-344VU98D3RU[.]star0x1[.]com_.

> Brian Krebs put out a great report on Stark Industries Solutions in May 2024, which can be viewed on his website [here](https://krebsonsecurity.com/2024/05/stark-industries-solutions-an-iron-hammer-in-the-cloud/).

![Shodan (103_113_71_207) 01](https://github.com/user-attachments/assets/33cba348-c69f-4fea-befb-af61cf7021c4)
![Shodan (103_113_71_207) 02](https://github.com/user-attachments/assets/aadfc244-a76b-4ea9-81c7-82b9f3affa95)
