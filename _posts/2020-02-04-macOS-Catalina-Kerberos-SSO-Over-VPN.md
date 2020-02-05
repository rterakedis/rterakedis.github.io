---
layout: post
title: Testing macOS Catalina Kerberos SSO Extension Over VPN
subtitle: Enabling the off-network Kerberos Single Sign-On Experience.
tags: [macOS]
comments: true
---

Working at VMware, I'm surrounded by great technology and super-smart folks!  In our portfolio of technologies, the folks in our R&D have recently been putting quite a bit of effort into building out macOS capabilities for our Workspace ONE Tunnel client for macOS.  Workspace ONE admins can leverage the same VMware technology they used to enable per-app VPN for iOS and Android, but now on macOS!  There's a bit of nuance to configuring the VPN client if you're previously familiar with iOS (look for my Operational Tutorial soon to hit [TechZone](https://techzone.vmware.com)).  That said, the premise is the same -- by configuring the appropriate rules, the Tunnel app redirects traffic from whitelisted applications back into your network through the Unified Access Gateway.  

Sounds simple enough, right?  So a few of us wondered if we could leverage some of this new technology with new Kerberos SSO Extension in Catalina.  

## Use-Cases

Sitting down to plan out testing, there were four main use cases I hoped to prove out:

1) What if the new Kerberos SSO Extension in macOS Catalina was just another one of those applications that you redirected over a VPN?
2) Could you, in theory, get Kerberos Tickets to an unbound Mac to use for authenticating to Workspace ONE Access and/or internal websites (over Per-App VPN)?
3) Could you also leverage the Kerberos SSO Extension to sync your local (non-mobile) macOS User account's password with the on-prem AD password over Per-App VPN?
4) Could you change your on-prem AD password remotely over Per-App VPN when it neared the expiration date?

It seemed pretty straightforward... but NOPE!  Some items that helped me troubleshoot this and discover what was going on under the hood:

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

I used Apple's Feedback Assistant app to provide feedback to Apple and provided them with an environment to reproduce the issue.  I'm hopeful that they can find a fix or provide some guidance to enabling the entirety of the Kerberos SSO Extension's functionality over a VPN.

More to come...
