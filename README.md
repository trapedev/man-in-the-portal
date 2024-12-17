# [Man-in-the-Portal](https://man-in-the-portal.github.io/): Breaking SSL/TLS Silently Abusing Captive Portal

<p align="center">
<img src="https://github.com/user-attachments/assets/24914c40-313d-4e6e-97fa-817e0326cb3b" alt="image"/>
</p>


## Abstract

The proliferation of public hotspots has led to the use of captive portals to protect hotspots and ensure their appropriate use. Captive portals control external sessions until user authentication at the hotspot is complete. One feature of captive portals is that these can redirect the authenticated user to an arbitrary website. However, cyber-attacks have been reported that exploit captive portals and there is an urgent need to improve the protocol of captive portals. In this paper, we reveal a critical flaw in the captive portal protocol and propose a man-in-the-middle attack that exploits the flaw to disable SSL/TLS. We name this attack Man-in-the-Portal (MITP). The attack is the first to exploit the post-authentication redirection of a captive portal as a starting point to disable SSL/TLS. The attacker can easily eavesdrop on and tamper with a victim device's communications. Our attack is also feasible without requiring any special privileges or tools. To demonstrate the effectiveness and practicality, we evaluate the MITP attack on five commercially available wireless devices as our proof-of-concept, and show that the attack poses significant threats. Furthermore, we analyze the root causes of the MITP attack and present protocol-level countermeasures to improve the security of wireless communications.

## Paper

Special issue of "Information Security and Trust to Support Social and Ethical Digital Activities"

DOI: [10.2197/ipsjjip.32.1066](https://doi.org/10.2197/ipsjjip.32.1066)

## Related Publications

### Peer-Reviewed International Conference Papers

- [A New Approach to Disabling SSL/TLS: Man-in-the-Middle Attacks are still Effective](https://ieeexplore.ieee.org/document/10406178) (CANDAR2023)

### Non Peer-Reviewd Papers

- [New Proposals and Threats of Man-in-the-Middle Attacks to Evade SSL/TLS: Evaluation and Consideration by Implementation](https://jglobal.jst.go.jp/detail?JGLOBAL_ID=202202236004551288) (FIT2022)

- [Implementation of Man-in-the-Middle Attacks that bypass SSL/TLS and allow eavesdropping/falsification: Public Wireless LAN Vulnerabilities and Threats](https://cir.nii.ac.jp/crid/1050013087466839168) (CSS2022)

## Appendix

### 1. [MITP Attack Demo](./video/mitp_attacks_demo_en.mp4)

### 2. [Responsible Disclosure](./disclosure/README.md)
