---
layout: default
---
## What You Need to Know About VPNs and TunnelVision
VPNs are often sold as a way to keep you safe on the internet, especially on Wi-Fi that isn't secure like in coffee shops, airports, or venues. But, VPNs were never really designed to do that. 

Eventually, companies selling VPNs started promising VPNs could do something they weren't designed for and developers used clever workarounds to fix issues. This approach appeared to work well until our discovery. 

We discovered a security and privacy problem in VPNs and we're calling it TunnelVision. This problem lets someone see what you're doing online, even if you think you're safely using a VPN. They can do this if they are on the same Wi-Fi or network as you. TunnelVision has been possible since 2002. It also doesn't have a complete fix for most computers.


## How TunnelVision Works

Imagine your computer is like a traveler at a train station, and each network (like public Wi-Fi or a VPN) is a different train. The VPN train is intended to be safer to use. 

Your computer uses a map (called "routing tables") to decide which train to take. But if someone changes the map, your computer can take the wrong train. That's what happens with TunnelVision. 

The malicious person tricks your computer into sending your private info over the local network instead of the VPN, totally avoiding the VPN’s protection.

Meanwhile, VPN marketing claims say they can protect you from someone changing the maps in the train station but this is impossible in most cases.

{% include youtube.html id='ajsLmZia6UU' %}

## Why This Matters

People who use VPNs and think they are completely protected on unsafe networks are wrong. This includes people like journalists or activists who really need to keep their information safe. 

TunnelVision shows that just using a VPN isn't enough. It also calls into question whether VPNs should make such promises at all.

## Why We Are Telling You This Now

We want everyone to understand this issue because it has been possible to do this since 2002, and maybe some people already know and use this trick. It would be hard to tell if they did.

We tried to tell VPN companies one-by-one about it, but there are too many for just our small team to contact. So we got help from bigger organizations, like the EFF and CISA to tell more people and companies. 

We told over 50+ companies but this is an international problem and is bigger than even they can assist with.

Now we're publishing more broadly with the hopes we can make people aware and keep more people safe.

## Fixing the Problem

Fixing this isn't easy. 

VPNs were originally created to connect two separate networks securely, not to protect all the information on your computer all the time in any environment.

The different systems are doing what they were meant to do. The misunderstanding comes from VPN companies saying they can keep you safe in ways they can't prove.

On Linux computers, there’s something called "network namespaces" that makes it possible to have only a VPN train in the station so there's no other choice. Other systems like Windows and MacOS don’t have a full solution yet, so VPNs can only use band-aid fixes, but can’t fix it completely. 

This means there is still a chance for someone to sneak a peek at your info or know who you are talking to when your using public Wi-Fi or untrusted networks.

## What You Can Do

If you need to keep your information very private:
1.  Avoid using Wi-Fi or networks you don’t trust, such public Wi-Fi.
1.  Connect to a personal hotspot WiFi instead, in most cases you can trust that more than public Wi-Fi.
1.  Use AdBlock and browsers that protect your privacy by blocking trackers.

Ask those who sell VPNs to:
1.  Be honest in their advertising.
1.  Think about adding extra security features to help protect users.
1.  Update their marketing and be specific about their promises.
