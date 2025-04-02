# H1 - Sniff

## Read and summarize

### Karvinen 2025: Wireshark - Getting Started

A guide to getting started with Wireshark, how to install it and some basic commands.

	sudo apt-get install wireshark

To install wireshark on Debian based distros.

	sudo adduser (username) wireshark

To add you to the Wireshark usergroup, to allow you to launch the program

	wireshark

To launch the program(Wireshark).

Then you can select the device you want to record.

### Karvinen 2025: Network Interface Names on Linux

	ip a

	ip route

To show your own interfaces, en connections are wired connections, wl are wireless and lo loopback for localhost connections.

## Install Debian/Kali on a VM

No tests required.

## Show that you can cut the internet connection and restore it inside of the VM

	ping 8.8.8.8

![h1/ping8.png](h1/ping8.png)

## Install Wireshark and capture traffic via Wireshark

I installed Wireshark through the package manager.

	sudo apt-get install wireshark

I also have it installed on my non-vm machine which is Arch based and the installation is similar.

	sudo pacman -S wireshark

![h1/capture-data.png](h1/capture-data.png)

## Demonstrate the TCP/IP-model's four layers from a single captured packet

Link Layer: Ethernet II

Internet Layer: Internet Protocol Version 4

Transport Layer: Transmission Control Protocol(TCP)

Application Layer: Transport Layer Security(TCP)

![h1/four-layers.png](h1/four-layers.png)

## Open surfing-secure.pcap, summarize what was captured e.g., how many packets, what kind of packets, which protocols.

![h1/wireshark-summarize.png](h1/wireshark-summarize.png)

Time: 7,5 seconds

Packets: 283

Which protocols: ARP, DNS, QUIC, TCP, TLS 1.2, TLS 1.3

### Which browser did the user use in surfing-secure.pcap?

I started by searching(CTRL+F) for "user" hoping to find user-agent in the strings somewhere, it didn't lead anywhere.

![h1/find-user.png](h1/find-user.png)

After which I started to look at the different protocols, ARP & DNS didn't have anything of note for me. QUIC Seemed to be promising, right click expand all and it had something regarding identity and below that fingerprinting JA4, JA3, it lead me to googling and finding an article regarding TLS fingerprinting(Hazeez 2022, TLS fingerprinting: What it is and how to implement it). JA4 nor the JA3 fingerprints in QUIC packets did not lead to anything but I saw the "Client Hello" packets that were mentioned in the afformentioned article which also had JA3 fingerprints which led to me finding out that Firefox was most likely used.

![h1/JA3-firefox.png](h1/JA3-firefox.png)

I also ended up checking out Giang's homework afterwards if it differed much from mine and it didn't, thank you! 

### What type of network interface did the user have in surfing-secure.pcap?

I could not determine the specific network interface from the pcap logs and depending on the OS it showed different Interfaces so I will conclude that it it unknown but with an educated guess it would be the one from the VM(RealTEk)?

![h1/non-vm-vs-vm-interface.png](h1/non-vm-vs-vm-interface.png)

### Which web-servers did the user visit on in surfing-secure.pcap?

By sorting by the DNS queries I could see that the user possibly visited: google.com, terokarvinen.com, gz.gc.at, commentero.terokarvinen.com, goatcounter.netflify.com,  goatcounter.terokarvinen.com.

![h1/DNS-queries.png](h1/DNS-queries.png)

I decided to check which sites loaded by capturing data with Wireshark when visiting Terokarvinen.com and all of these showed up: terokarvinen.com, gz.gc.at, commentero.terokarvinen.com, goatcounter.netflify.com,  goatcounter.terokarvinen.com.

![h1/terokarvinen-com.png](h1/terokarvinen-com.png)

After that I decided to visit Google.com and capture data, it showed significantly more DNS queries than were in the surfing-secure.pcap, so the DNS queries to google in the surfing-secure.pcap probably aren't because the user visited google.com but instead possibly some analytics or opening a new tab in Firefox(firefox defaults to google.com as it's search engine)?

![h1/google-com.png](h1/google-com.png)
 
## Capture and analyze traffic using Wireshark.

![h1/new-tab-firefox.png](h1/new-tab-firefox.png)

I captured data from opening a new tab in firefox.

It captured 36 packets, capture length was around 1 second, protocols include: TLSv1.2, TCP, DNS and plain HTTP. Source is the where data is coming from and Destination the where it is going, if it is local ip address it will show the network interfaces ip so in my case 10.0.2.15. Info tab shows information regarding the packet, clicking a packet open also shows the data in the packet and if it is unencrypted it might even be human readable.

## Sources:

Karvinen, T. 2025. Network Attacks and Reconnaissance. Available at: ![https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/)

Karvinen, T. 2025. Wireshark - Getting Started. Available at: ![https://terokarvinen.com/wireshark-getting-started/](https://terokarvinen.com/wireshark-getting-started/)

Karvinen, T. 2025. Network Interface Names on Linux. Available at: ![https://terokarvinen.com/network-interface-linux/](https://terokarvinen.com/network-interface-linux/)

Karvinen, T. 2025. surfing-secure.pcap file. Available at: ![https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/surfing-secure.pcap](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/surfing-secure.pcap)

JA3 DB. Available at: ![https://ja3.zone/](https://ja3.zone/)

Hazeez. 2022. TLS fingerprinting: What it is and how to implement it. Available at: ![https://fingerprint.com/blog/what-is-tls-fingerprinting-transport-layer-security/](https://fingerprint.com/blog/what-is-tls-fingerprinting-transport-layer-security/)

Giang. 2025. H1 Sniff. ![https://github.com/gianglex/Courses/blob/6e74794c2e27752698f9bff0455a927843d9f934/Verkkoon-Tunkeutuminen-ja-Tiedustelu/h1-sniff.md#f-mit%C3%A4-selainta-k%C3%A4ytt%C3%A4j%C3%A4-k%C3%A4ytt%C3%A4%C3%A4](https://github.com/gianglex/Courses/blob/6e74794c2e27752698f9bff0455a927843d9f934/Verkkoon-Tunkeutuminen-ja-Tiedustelu/h1-sniff.md#f-mit%C3%A4-selainta-k%C3%A4ytt%C3%A4j%C3%A4-k%C3%A4ytt%C3%A4%C3%A4)
