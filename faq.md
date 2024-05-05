---
layout: default
---

# How was this found?
We researched VPN threat models for a project and produced methods to attack VPNs. The team's idea proved to be a severe issue and we suspected it affected it affected the entirety of the VPN industry. That's when we began research in earnest.

# Has this been exploited in the wild?
We have not seen evidence as of 5/6/2024 of exploitation in the wild. However, we believe this was possible as far back as 2002 so it's possible someone besides us found TunnelVision but didn't widely disclose or realize the severity.

In order to detect this, you would need to be reviewing DHCP network traffic and DHCP client logs. This might be possible retroactively.

# Haven't routing table leaks in VPNs been known for years? How is this novel?
Correct, routing table issues have been known for years. This is why the privacy and anonymity communities discourage VPNs and have been doing so for years. Most issues found have been small leaks and patchable.

We have not seen any prior research using DHCP option 121 to deliver routes, which is the novel part of our research. It also not patchable in the same way smaller leaks have been (besides Linux).

# Wouldn't exploiting this be obvious?
Not really. The VPN user shows as being connected to the VPN. Attackers can control any IPs they wish to decloak, so theoretically they could choose to not decloak leak checking IPs. They could also leak DNS IP's while forwarding the traffic so the VPN connection is intact but they obtain information about the traffic in the tunnel.

In no circumstances did we observe a VPN Server disconnecting us with kill switches or other features.

# Isn't most web traffic encrypted with HTTPS anyways?
This is true. If HTTPS traffic is decloaked it is not possible to view the encrypted contents of the packet. For other protocols that are unencrypted, the packet's payload is also readable so in this way an attacker might get lucky.

# Why are you saying the mitigation's side-channel is so impactful?
Since most web traffic is encrypted with HTTPS, attackers can only determine the destination and protocol of the traffic. 

When a mitigation is in place, an attacker can obtain the destination you are talking to and the amount of traffic you are sending to that destination. For most web traffic, this is nearly the same impact as decloaking.

The side-channel is also flexible in use:
1. The traffic can be checked against a predefined list of IP addresses.
1. The traffic can be selectively denied as a censorship mechanism.
1. The attacker can use IP space denial with binary search to determine all current connections in logarithmic time.

# Why is this so bad compared to other leaks?
In a threat model from a VPN providers, there is often a claim that no one, not even a network administrator, is able to remove your VPN protections. This is why VPN providers have been claiming they can "secure untrusted traffic". 

TunnelVision shows that a network administrator can completely remove VPN protections. To make matters worse, we've demonstrated that an attacker who is on the same network has ways become a DHCP administrator, this is demonstrated in our POC video.

# Don't most networks have widespread adoption of DHCP snooping or other protections?
Most enterprise networks do. However, most people using VPNs are not connecting from an enterprise network. They are connecting from public networks or their home networks.

In addition, part of the VPN provider threat model is they can secure *any* untrusted network, including those who do not have these protections.

# Can you change the configuration of the DHCP server in the middle of a VPN session?
Yes. By setting a short lease length, an attacker can modify the configurations in the middle of a VPN session.