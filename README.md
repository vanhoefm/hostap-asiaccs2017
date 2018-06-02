Quick tests for two vulnerabilities mentioned in the paper [Discovering Logical Vulnerabilities in the Wi-Fi Handshake Using Model-Based Testing](https://lirias2repo.kuleuven.be/bitstream/handle/123456789/572634/asiaccs2017.pdf).

You can enable a specific tests by uncommenting it in the file [`research.h`](src/common/research.h). Once you selected a test, compile and run `wpa_supplicant` as usual.

# 4.3 Forging Message 4 (a.k.a. Message Confusion)

Test is performed if `RESEARCH_MESSAGE_CONFUSION` is set. It will send message 2 instead of message 4 to complete the 4-way handshake. When using WPA (not WPA2) some routers treat the message 2 as message 4, and will successfully complete the handshake. Against a vulnerable AP, the 4-way handshake will complete. Against secure APs, you should not be able to connect to the network.

When we performed this attack, it only worked when using WPA (and not when using WPA2). To force usage of WPA, you can include `proto=WPA` in your network configuration file.

# 4.2.1 Impossible TKIP Countermeasures

Test is performed if `RESEARCH_SEND_MICFAILS` is set. After executing the 4-way handshake, the client will send two MIC failure reports. If the client is not using TKIP, the AP should ignore these MIC failure reports. If the AP is vulnerable, the client will get disconnected, and cannot reconnect for 1 minute. If the AP is secure, nothing happens (and the client stays connected).

To make sure the client isn't using TKIP, you can include `pairwise=CCMP` and	`group=CCMP` in the network configuration file. This assures TKIP is not used, meaning the AP should ignore all MIC failure reports that we went.
