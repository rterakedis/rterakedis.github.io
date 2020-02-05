---
layout: post
title: Testing macOS Catalina Kerberos SSO Extension Over VPN
subtitle: Enabling the off-network Kerberos Single Sign-On Experience.
tags: [macOS]
comments: true
---

Working at VMware, I'm surrounded by great technology and super smart folks!  In our portfolio of technologies, the folks in our R&D have recently been putting quite a bit of effort into building out macOS capabilities for our Workspace ONE Tunnel client for macOS.  Basically, Workspace ONE admins can leverage the same VMware technology they used to enable per-app VPN for iOS and Android, but now on macOS!  There's a bit of nuance to configuring the VPN client if you're previously familiar with iOS (look for my Operational Tutorial soon to hit [TechZone](https://techzone.vmware.com)).  That said, the premise is basically the same -- by configuring the appropriate rules, the Tunnel app redirects traffic from whitelisted applications back into your network through the Unified Acceess Gateway.  

Sounds simple enough, right?  That's what I thought...
