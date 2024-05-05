---
layout: default
---
# Introduction 
VPNs are marketed as a security service that protects users even on untrusted networks (e.g. public Wi-Fi). However, it has been known within the security industry that these claims are questionable, and small leaks have been discovered over the years. Most research has been focused on VPN servers rather than leaking client traffic on a local network.

Recently, a technique known as [TunnelCrack](https://tunnelcrack.mathyvanhoef.com/) allowed attackers to leak data from a VPN. Simultaneously, we have been working on a more general technique we call "TunnelVision."

TunnelVision leaks VPN traffic more simply and powerfully. We have demonstrated an attacker can leak all traffic just by being on the same local network as a VPN user. 

From the user's perspective, they appear as if they are connected to the VPN.

# How TunnelVision Works
Computers may be connected to multiple networks at once. A VPN is also a network you're connected to. Computers decide which network they should send traffic through using a set of rules called "routing tables". 

An attacker on the same local network can manipulate these rules and force traffic over the wrong network.

From there, they can redirect traffic meant for the VPN to the local network, *completely bypassing the VPN.* Because this technique is not dependent on exploiting VPN technologies or underlying protocols, it works completely independently of the VPN provider or implementation. 

We call this total bypass, **decloaking.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/ajsLmZia6UU?si=tjakCcpuNF-KnGd9" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# Impact 
VPN users who expect VPNs to protect them on untrusted networks are as susceptible to the very same attacks as if they weren't using a VPN. This is particularly dangerous for people who rely on VPNs to keep them safe such as journalists and political dissidents.

Luckily, most users who use commercial VPNs are sending web traffic which is mostly HTTPS ([about 85%, actually](https://w3techs.com/technologies/details/ce-httpsdefault)). HTTPS traffic looks like gibberish to attackers using Tunnelvision, but they know who you are sending that gibberish to which can be an issue. If a website is using HTTP then it becomes possible to view everything you are saying as well as who you are saying it to.

Our research challenges previous understanding of VPN technology. It also raises questions about what other technologies are impacted by routing table attacks and how this could go unnoticed for 20+ years.

# Why We Are Publishing Now
We aim to raise awareness about this technique in the security and privacy communities. Since it could have been exploitable as early as 2002, it might already be in use without widespread knowledge. 

We initially disclosed this to several VPN providers but quickly realized that it wasn't scalable and there were too many affected parties. We contacted the EFF and CISA and through them, we've made over 50+ vendors aware of this before public release.

In addition, to help raise awareness, we decided to name the CVE, in the hopes that a more human-readable name would extend the awareness to outside the security and privacy communities and into the general public. 

# How Do We Fix This
Well, that's a difficult question. VPNs were not made to secure every connection on a computer. Their purpose is to connect two local networks that normally are not connected and to secure data going over that bridge. It was not meant to protect traffic on the local network, just to protect traffic after exiting to the WAN.

In that regard, VPNs are working as intended, local network protocols are working as intended, and the routing tables in your operating system are working as intended. Fundamentally, the public understanding of VPNs is what's broken. This is largely due to marketing campaigns by VPN providers. 

There is a fix on Linux operating systems through a feature called "network namespaces". Network namespaces can segment interfaces and routing tables away from the local network’s control.

Other operating systems do not have a full fix available. For those operating systems, the best that a VPN provider can do is mitigate the leak. However, all mitigations we've observed still expose a serious issue for users who rely on total privacy of their connection and the issue can also be abused for censorship.

Additionally, there might be bypasses that we have not discovered for the mitigations, so inevitably relying on using mitigations will create a "cat and mouse" game between attackers and defenders.

Groups primarily affected by the shortcomings of mitigations are those who commonly are the targets of surveillance or spyware, journalists, whistleblowers, and those needing to bypass censorship. 

One could argue that the OS maintainers should implement this feature and one could also argue that VPNs shouldn't be advertising a feature that is impossible on many devices. 

Ultimately, we feel that it's a shared responsibility and the people who suffer from this are the VPN users.

# Call to Action
If you require total privacy of your connection:
1.  Do not use untrusted networks (Public WiFi)
1.  Consider using a hotspot with your VPN
1.  Consider using a VPN inside a virtual machine that does not have a bridged network adapter
1.  Use AdBlock and privacy browsers that reject tracking cookies

If you are a VPN provider:
1.  Review and update your marketing: do not claim untrusted networks can be secured by you
1.  Where possible, use network namespaces features in your product
1.  Consider host-based firewall protections to partially mitigate local network attacks

If you are an operating system maintainer:
1.  Consider implementing network namespaces if your operating system doesn't support it.

# Prior Research
We found research related to DHCP leaking default routes over an incorrect interface [as far back as 2015](https://petsymposium.org/popets/2015/popets-2015-0006.pdf).
However, it was implied that DHCP only pushed default routes and the research did not take into account DHCP option 121.

[This research from Anvil Secure](https://www.anvilsecure.com/blog/dhcp-games-with-smart-router-devices.html) did mention option 121, and they used this technique against attacking
smart router devices with multiple physical network interfaces. However, this was not applied to network
client devices that use a VPN which creates a virtual interface.

In August 2023, [TunnelCrack was published](https://tunnelcrack.mathyvanhoef.com/#summary) demonstrating that routing rules can be attacked in different ways
to leak VPN traffic. The paper details two methods of leaking VPN traffic:
- Abusing non-RFC1918 IP ranges to leak traffic.
- DNS spoofing the VPN server’s domain name to trick the VPN client into adding a routing rule
exception for an IP address.

However, neither technique described in TunnelCrack leveraged DHCP option 121 to push
routes. Pushing routes through DHCP has a significantly higher impact from the same attacker vantage
point (the ability to hand out IP leases for a non-RFC1918 range or spoofing DNS replies).

In short, researchers came very close to discovering TunnelVision earlier.