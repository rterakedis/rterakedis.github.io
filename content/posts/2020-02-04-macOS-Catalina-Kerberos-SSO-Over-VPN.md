---
title: "Testing macOS Catalina Kerberos SSO Extension Over VPN"
subtitle: Enabling the off-network Kerberos Single Sign-On Experience.
date: 2020-02-04
draft: false
tags: ["macOS", "SSO", "VPN"]
url: /2020-02-04-macOS-Catalina-Kerberos-SSO-Over-VPN/
---

Working at VMware, I'm surrounded by great technology and super-smart folks!  In our portfolio of technologies, the folks in our R&D have recently been putting quite a bit of effort into building out macOS capabilities for our Workspace ONE Tunnel client for macOS.  Workspace ONE admins can leverage the same VMware technology they used to enable per-app VPN for iOS and Android, but now on macOS!  There's a bit of nuance to configuring the VPN client if you're previously familiar with iOS (look for my Operational Tutorial soon to hit [TechZone](https://techzone.vmware.com)).  That said, the premise is the same -- by configuring the appropriate rules, the Tunnel app redirects traffic from whitelisted applications back into your network through the Unified Access Gateway.  

Sounds simple enough, right?  So a few of us wondered if we could leverage some of this new technology with new [Kerberos SSO Extension](https://www.apple.com/business/docs/site/Kerberos_Single_Sign_on_Extension_User_Guide.pdf) in macOS Catalina.  

## Use-Cases

Sitting down to plan out testing, there were four main use cases I hoped to prove out:

1. What if the new Kerberos SSO Extension in macOS Catalina was just another one of those applications that you redirected over a VPN?
2. Could you, in theory, get Kerberos Tickets to an unbound Mac to use for authenticating to Workspace ONE Access and/or internal websites (over Per-App VPN)?
3. Could you also leverage the Kerberos SSO Extension to sync your local (non-mobile) macOS User account's password with the on-prem AD password over Per-App VPN?
4. Could you change your on-prem AD password remotely over Per-App VPN when it neared the expiration date?

It seemed pretty straightforward... but NOPE!  I ran into quite a bit of unexpected behavior!  Luckily, one of my coworkers, [Adam](https://blog.eucse.com/contact/), had a slightly different configuration than I and was able to test all of this as functioning for macOS that is on-network with on-premise Active Directory.   

## Test Environment Configuration

Here's how I have things configured in my *test* environment:

* Isolated Network with the following VMs:
  * Two Windows Server 2016 Domain Controllers, one of which runs DNS
  * Airwatch Cloud Connector and Workspace ONE Access Connnectors configured
  * One IIS web server running two sites -- one configured for Kerberos Auth and one configured for Anonymous access.
  * One ADCS server (not used for this testing)
* Unified Access Gateway Appliance v3.8 (the only VM with inbound access from the Internet)
  * Tunnel/Edge service is enabled/configured
* SaaS-based Workspace ONE UEM and Workspace ONE Access.
* Workspace ONE Tunnel for macOS configured as an auto-deployed Volume Purchase app (from Apple Business Manager)
* The DNS name for my AD domain set up in Device Traffic Rules for tunneling.
* Google Chrome, Firefox, and Safari all configured to allow kerberos authentication to the website.

### Commands

```bash
defaults write -g GSSDebugLevel 20

defaults write -g KerberosDebugLevel 20

log stream --debug --predicate '(subsystem == "com.apple.Heimdal") OR (subsystem == "com.apple.AppSSO") OR (subsystem == "org.h5l.gss") OR (subsystem == "com.apple.network") OR (process == "VMware Tunnel") '
```

## Results

As it turns out, the Kerberos SSO Extension in Catalina appears designed for situations where macOS is on-network with an on-premise Active Directory.   I went through some testing using our Per-App Tunnel (and a full-device Global Protect VPN), and ran into the following testing results:

| Testing Item | Per-App VPN (Tunnel) | GlobalProtect (VPN) |
|--------------|----------------------|---------------------|
| Kerberos Ticket Obtained over VPN | Yes! | Yes! |
| Password Expiration Date Correct | No! | No! |
| Extension Detects local PW different from AD | No! | Yes! |
| User Change AD Password via Extension over VPN | No! | No! |

I was able to confirm that a kerberos ticket is obtained using `klist` in Terminal.  I could also confirm that I could authenticate to the Kerberos-enabled IIS website without having to provide a username/password in the browser.   Kerberos functionality appeared to be working, just not any other functionality.

## Looking Forward
I used Apple's Feedback Assistant app to provide feedback to Apple and provided them with an environment to reproduce the issue.  I'm hopeful that they can find a fix or provide some guidance to enabling the entirety of the Kerberos SSO Extension's functionality over a VPN.  

| Please reach out if you've done testing as well - I'd love to hear your experience!

More to come...
