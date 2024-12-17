# Responsible Disclosure

## Report

```eml
Date: Fri, 21 Jun 2024 13:38:28 +0900
To: security@gl-inet.com
From: Keiichiro KIMURA <233t225t@cloud.kobe-u.jp>
Subject: [Potential Security Vulnerability Report] Vulnerabilities in the
 post-authentication redirection behavior of captive portals

Dear GL.iNet

We, Keiichiro Kimura, Hiroki Kuzuno, Yoshiaki Shiraishi, and Masakatu Morii (ES3 Lab., Graduate School of Engineering, Kobe University), would like to bring your attention potential security vulnerabilities that we have confirmed. As security researchers, we believe it is crucial to report such findings to ensure the continued security and integrity of the Wi-Fi ecosystem.

0. Executive Summary

- This report indicates a vulnerability in the post-authentication redirection behavior of captive portals.
- This report also provides a proof-of-concept for a man-in-the-middle attack against HTTP/HTTPS that abuses the reported vulnerability.
- Our request is to notify you of our vulnerabilities and to assign CVE numbers to these vulnerabilities.
- We plan to publish a paper in December 2024 about this vulnerability and the attacks that abuse it. Therefore, we would like to apply the 90-day policy for vulnerability disclosure.

1. Vulnerability type

Potential man-in-the-middle (MitM) attacks against HTTP/HTTPS.

2. Vulnerability details

We report a vulnerability in captive portals. Specifically, we report that the mechanism itself, which allows redirection to arbitrary websites after authentication in captive portals, is a vulnerability.

Captive portals define two redirect requests. The first redirect request is for unauthenticated users to perform the authentication necessary to grant access to the Internet. The second redirect request is a request to redirect the user to a website specified by the hotspot administrator after authentication. We consider this second redirect request in the captive portal protocol to be a critical flaw and the potential security risks of MitM attacks.

After authentication, the URLs of websites designated by the hotspot administrator vary depending on the hotspot and its administrator. There are no restrictions on the types of these websites. Consequently, if the website to which users are redirected after authentication is not a corporate website or one designed for advertising, but rather a website facilitating internet browsing, such as search engines or social networking services (SNS), users may engage with the redirected website. According to Muhammad Sangeen et al., it has been empirically demonstrated that users of public hotspots primarily connect to these hotspots to access SNS and search engines (※2-1). Given the simplicity of impersonating websites like search engines and SNS, the post-authentication redirect protocol of captive portals is deemed to potentially offer attackers a straightforward opportunity for launching MitM attacks (※2-2).

※2-1: Blind-trust: Raising awareness of the dangers of using unsecured public Wi-Fi networks, DOI: https://doi.org/10.1016/j.comcom.2023.07.011

※2-2: Phishing and Spoofing Websites: Detection and Countermeasures, DOI: https://doi.org/10.18517/ijaseit.13.5.19037

3. Affected firmware versions

All firmware versions that allow specifying the redirect destination after Captive Portal authentication.

4. Attacks abusing the captive portal vulnerability.

In this section, we briefly present attacks abusing the captive portal vulnerability described in Section 2.

4.1. Threat model

Before presenting the attack, we define the system model and the attacker model.

4.1.1. System model

We consider a victim using the Internet in public. The victim carries a device, such as a mobile phone or a laptop, capable of communicating with an external server via HTTP/HTTPS and using a web browser on the device. The device, commercially available, does not implement the rogue hotspot detector.  We assume the victim uses a hotspot located in a public place, for example, to access an e-commerce site.

We consider a victim using the Internet in public. The victim communicates with external servers via HTTP/HTTPS using a smartphone or a laptop. The device, commercially available, does not implement the rogue hotspot detector. We assume the victim uses a hotspot located in a public place, for example, to access e-commerce sites.

We assume that the websites visited by victims are highly encrypted with SSL/TLS, and HSTS enforces SSL/TLS-enabled communications. Victims do not check the URLs of the websites they browse. Additionally, we assume that the web browser available on the victim device requires SSL certificate verification for SSL/TLS-enabled websites, and even the root certificate is verified.

4.1.2. Attacker Model

We consider a scenario involving an attacker equipped with a laptop and a hotspot device. The attacker uses the hotspot as a rogue hotspot, impersonating a public hotspot, targeting victims who connect to the rogue hotspot. The attacker aims to eavesdrop on and tamper with the victim's communications without triggering any alerts or errors on the victim device.

The attacker does not necessarily need to be on the same network as the victims who access the rogue hotspot. Furthermore, in order to masquerade the rogue hotspot as the legitimate public hotspot, the attacker already know the Service Set Identifier (SSID) of the legitimate public hotspot. To lure victims to the network of the rogue hotspot, the attacker operate the rogue AP, which has a forged SSID, near the legitimate public hotspot for a certain period of time. Victim devices need to be physically present within the connection range of the rogue hotspot.

4.2. Attack strategy

The objective of the attack is to intercept and tamper with the SSL/TLS-encrypted communication between the victim device and the web server. To achieve this, the attacker must position themselves as a MitM between the victim device and the server. Our attack consists of the following steps, Step 1 to 3, to become the MitM.

4.2.1. Step 1: Hotspot Spoofing

To lure the victim device to connect to the rogue hotspot controlled by the attacker, the attacker spoofs their hotspot's SSID to mimic that of a legitimate one, thus impersonating a legitimate hotspot.

4.2.2. Step 2: Redirection to a Fake Search Engine

Exploiting the characteristics of captive portals which allow redirection to any website after authentication, the attacker lures the victim device to a fake search engine.

4.2.3. Step 3: MitM Attacks Disabling SSL/TLS

A MitM attack that initiates the victim's Internet surfing from a fake search engine and effectively disables SSL/TLS on the destination web server.

5. Proof-of-concept (PoC)

We describe PoC devices and its setup.

5.1. PoC devices

We describe devices used our PoC.

Please note that the devices used in the PoC are only a subset, and there is a significant possibility that the attack may succeed on devices other than these.

5.1.1. Device as an attacker

+--------------+------------------+------------------+------------+
| Manufacturer | Model            | Operation System | OS Version |
+--------------+------------------+------------------+------------+
| Microsoft    | Surface Laptop 4 | Windows 11 Home  | 23H2 |
+--------------+------------------+------------------+------------+

5.1.2. Device as a rogue hotspot

+--------------+-------------------+---------+------------------+------------+----------------+
| Manufacturer | Model             | Version | Operation System | OS Version | Kernel Version |
+--------------+-------------------+---------+------------------+------------+----------------+
| GL.iNet      | GL-AR300M(shadow) | v1.4.0  | OpenWrt          | 19.07.7    | 4.14.221       |
+--------------+-------------------+---------+------------------+------------+----------------+

5.1.3. Device as victims

+--------------+------------------+------------------+---------------+
| Manufacturer | Model            | Operation System | OS Version    |
+--------------+------------------+------------------+---------------+
| Microsoft    | Surface Laptop 3 | Windows 11 Home  | 22H2          |
| Lenovo       | V730-13          | Windows 10 Home  | 22H2          |
| Google       | Pixel 3XL        | Android OS       | 12            |
| Samsumg      | Galaxy S8        | Android OS       | 9             |
| Apple Inc.   | iPhone 13 Pro    | iOS              | 17.3.1        |
| Apple Inc.   | MacBook Air      | macOS            | Sonoma 14.3.1 |
+--------------+------------------+------------------+---------------+

5.2. PoC setup

We consider one device and one GL.iNet's hotspot for the attacker's rogue hotspot, along with one victim device.

The device used by the attacker has some malicious attack tool installed that mediates HTTP/HTTPS requests posing as a search engine. The rogue hotspot is set up with the attacker's address as the redirection destination after the captive portal authentication. The SSID of the rogue hotspot is also modified in advance. The attacker attempts to eavesdrop and tamper with the communications of victim terminals accessing the rogue hotspot.

The victim attempts to access the network provided by the rogue hotspot, mistaking it for a legitimate public hotspot.

5.3. PoC results

In the PoC, we use three e-commerce sites (EC-1, EC-2, and EC-3) that are in actual operation for the attack evaluation. To prevent disadvantages to third-party organizations, we will withhold the details of these three e-commerce sites.

The results are shown below. SC indicates that the attack was successful and that eavesdropping and tampering of HTTP/HTTPS communication is possible. PS indicates that the attack is successful only when the victim's device opens the captive portal authentication screen in a browser instead of the captive portal browser. NS indicates failure to redirect to the site specified as the post-authentication redirect destination of the captive portal.

+------------------+------+------+------+
| Model            | EC-1 | EC-2 | EC-3 |
+------------------+------+------+------+
| Surface Laptop 3 | SC   | SC   | SC   |
| V730-13          | SC   | SC   | SC   |
| Pixel 3XL        | NS   | NS   | NS   |
| Galaxy S8        | PS   | PS   | PS   |
| iPhone 13 Pro    | SC   | SC   | SC   |
| MacBook Air      | SC   | SC   | SC   |
+------------------+------+------+------+

5.4. PoC demonstrations

The URL of the PoC demo video is provided below. In the demo video, we have disclosed the targeted e-commerce site without any mosaics only to GL.iNet in order to demonstrate the threat of the vulnerability and the transparency of our PoC. To prevent any disadvantage to third-party organizations, we kindly request that you **refrain** from unauthorized posting of this video and **do not disclose** the URL of this video under any circumstances. In addition, we cannot disclose the MitM attack tool used in the PoC demo video due to our confidentiality obligations. We hope for your understanding in this matter.

https[:]//drive[.]google[.]com/file/d/17kjZXtCCHqIrBf6ySHbM9_RGm_Lsqp7m/view?usp=sharing

6. Our requests

- Please notify our vulnerabilities and assigning of the vulnerabilities of CVE numbers.
- We plan to publish a paper in December 2024 about this vulnerability and the attacks that abuse it. Therefore, we would like to apply the 90-day policy for vulnerability disclosure.

That's all.
Thank you for your attention to this report. We look forward to your prompt response and collaboration in addressing this potential security risk. We recognize that this vulnerability does not depend solely on GL.iNet products. We will report this vulnerability to a number of organizations, including the IETF.

Sincerely,
Keiichiro Kimura (ES3 Lab., Graduate School of Engineering, Kobe University)

****************************************
Keiichiro Kimura
ES3 Lab.
Graduate School of Engineering, Kobe University
1-1, Rokkodai-cho, Nada-ku, Kobe, 657-8501, JAPAN
E-mail (Keiichiro Kimura) : 233t225t@cloud.kobe-u.jp
****************************************
```

## Response

```eml
From: <alzhao@gl-inet.com>
To: "'Keiichiro KIMURA'" <233t225t@cloud.kobe-u.jp>,
	<security@gl-inet.com>
Subject: RE: [Potential Security Vulnerability Report] Vulnerabilities in the post-authentication redirection behavior of captive portals
Date: Fri, 21 Jun 2024 14:36:14 +0800

Hi Keiichiro,

This is how Captive portal works.

This happens in all devices that can create a portal. It is not a vulnerability of the router. It is a function of the router.
It is a vulnerability of the Internet.

To solve this, operating system, browser developers and antivirus companies should address this.

BTW, if you check the router further, you will find that the router is created to solve this problem by using encrypted DNS and VPN.

Best,
Alfie
```
