# Network

## Network Layer

https://www.electronicdesign.com/unused/article/21800810/whats-the-difference-between-the-osi-sevenlayer-network-model-and-tcpip

https://www.techopedia.com/definition/6006/application-layer

https://www.techopedia.com/definition/8955/presentation-layer

https://www.techopedia.com/definition/9322/session-layer

https://www.techopedia.com/definition/9760/transport-layer

https://www.techopedia.com/definition/24204/network-layer

https://www.techopedia.com/definition/18698/data-link-layer

https://www.techopedia.com/definition/8866/physical-layer

https://www.totolink.vn/article/136-mo-hinh-osi-la-gi-chuc-nang-cua-cac-tang-giao-thuc-trong-mo-hinh-osi.html

| OSI Layer                         | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| Appl­ication Layer                | Approaches protocols for network applic­ations (web browsers or email programs) interaction with the network. Examples: HTTP, HTTPS, SMTP, FTP |
| Presentation Layer (Syntax Layer) | With a focus on end-user services, the application layer helps to facilitate process-to-process connections over Internet protocol. The presentation layer is responsible for the following:<br/>Data encryption/decryption<br/>Character/string conversion<br/>Data compression<br/>Graphic handling<br />The presentation layer is responsible for integrating all formats into a standard format for efficient and effective communication. |
| Session Layer                     | Controls the connections between multiple computers. The session layer tracks the dialogs between computers, which are also called sessions. This layer establishes, controls and ends the sessions between local and remote applications. |
| Tran­sport Layer                  | Be responsible for end-to-end communication over a network,deliver and receive data without errors. (TCP/UDP)<br />The send side breaks application messages into segments (packets) and passes them on to the network layer<br />The receiving side then reassembles segments into messages and passes them to the application layer |
| Network Layer                     | Addresses and packages data for transm­ission. Routes the packets across the network. (Internet Protocol) |
| Data Link Layer                   | The data link layer is used for the encoding, decoding and logical organization of data bits. Data packets are framed and addressed by this layer, which has two sublayers.(media access control - MAC, logical link control, ethernet, WIFI) |
| Physical Layer                    | This layer plays with most of the network’s physical connections—wireless transmission, cabling, cabling standards and types, connectors and types, network interface cards |

![Kết quả hình ảnh cho TCP/IP model](assets/Big_words_web/imgDownload)

![network-2022-01-26-21-33-17](/assets/network/network-2022-01-26-21-33-17.png)


https://en.wikipedia.org/wiki/Lists_of_network_protocols

A **network protocol** is a set of rules/­con­ven­tions that dictate how a network operates.

| Protocol              | A family of protocols that dictate how devices on the same network segment format and transmit data. | Port   |
| --------------------- | ------------------------------------------------------------ | ------ |
| **Wi-Fi** or **WLAN** | A family of protocols that deal with wireless transm­ission. |        |
| **TCP**               | **T**r­ans­mission **C**o­ntrol **P**r­otocol: splits (and later reasse­mbles) data into packets. Also involves error checking, as expects an acknow­led­gement transm­ission within a set time frame. |        |
| **UDP**               | **U**ser **D**a­tagram **P**r­otocol: splits (and no reasse­mbles) data into packets. No error checking |        |
| **IP**                | **I**n­ternet **P**r­otocol: each device has an IP address. Packets are 'addre­ssed' to ensure they reach the correct user. |        |
| **HTTP**              | **H**y­pertext **T**r­ansfer **P**r­otocol: used to access a web-page from a web server. | 80     |
| **HTTPS**             | **H**y­pertext **T**r­ansfer **P**r­otocol **S**e­cure: uses encryption to protect data. | 443    |
| **FTP**               | **F**ile **T**r­ansfer **P**r­otocol: handles file uploads and downloads, transfers data and programs. | 20, 21 |
| **SMTP**              | **S**imple **M**ail **T**r­ansfer **P**r­otocol: handles outbound email. SMTP servers have databases of user's email addresses. | 25     |
| **IMAP**              | **I**n­ternet **M**e­ssage **A**ccess **P**r­otocol: handles inbound emails. |        |
| **SSH**               | **Secured Shell (SSH)** protocol uses encryption to secure the connection between a client and a server. All user authentication, commands, output, and file transfers are encrypted to protect against attacks in the network. | 22     |
| DNS                   | Domain Name System                                           | 53     |
| DHCP                  | Dynamic Host Configuration Protocol                          | 67, 68 |

## Transport

### TCP/UDP

![network-2022-01-26-15-02-04](/assets/network/network-2022-01-26-15-02-04.png)

 HTTP uses port 80 and HTTPS uses **port 443**

## Application

### DNS

The Domain Name System (DNS) is the phonebook of the Internet.

key/value pair of domain and IP
