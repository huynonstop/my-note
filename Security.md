# What is Web Application Security?

https://www.cloudflare.com/learning/security/what-is-web-application-security/

https://restfulapi.net/security-essentials/

https://nordicapis.com/fostering-an-internal-culture-of-security/

# Secrets and Keys

https://random-ize.com/how-long-to-hack-pass/

> Authentication is the art of unlocking things with a secret. At the heart of every authentication is a shared secret!

The secret can be given to you, like a secret code to enter a night club, or you can generate the secret and store it with the system like your pin number for your bank card.

# Public and private keys

Either way, the secret has two copies. One copy is with the system and the other copy is with you. Your copy is what is called the *private* key.

The copy of the key on the server is not readable by another human being or a machine. The server only has a highly encrypted version of the secret; it doesn’t know what the decrypted value would look like.

> In the digital world, the **key** is called a **key**, and the **lock** is also called a **key**. So there are two keys- **your copy** which is the **private key**, and the **other** is the **public key** that’s accessible to anyone.
>
> This is the public/private key pair.

Your front door lock is public. Anybody can come up to your front door and try your lock with their key but only your key would open it. While your key is private, the lock of your house is public.

# Factors

> Factors authenticate you by who you are, what do you know, and what do you have.

In addition to username and password, authentication can include additional information called *factors*.

1. Single Factor — this is the username and password that we talked about
2. Two Factor — this is where, along with the username and password, you need to answer another secret question that you set up while signing up. So you have 2 secrets keys. One for who you are and the other for what you know.
3. Multi-Factor Authentication (MFA) — this is the hardest to crack. You use your secret key, and you set up a secret location for the server to send you another secret key. When you use your first secret key, the server sends you another secret key to the secret location that you mutually agreed on during the sign-up process (usually people set this as their phone number or their email address). This second secret key is usually valid for only a few minutes. Within that timeframe, you’ll need to use this second secret key to login.
4. Biometric authentication — this is the fingerprint ID that is becoming a common part of authenticating on the mobile. My Dell laptop that I’m writing this article on has biometric authentication for logging in. I have to run my index finger on the power button to be able to log in.
5. Captchas — there are two kinds. One where you have to decipher the squiggly words that you see and type those in. The other where you have to identify mountains, or cars, or storefronts, or any number of things in a grid of pictures. These are the most annoying ones, and I hope they go away soon. Sometimes the squiggly words are so convoluted that I have to ask for a second set, and then turn on the audio to hear them.

# Authorization and Authentication

https://anonystick.com/blog-developer/4-co-che-dang-nhap-bai-viet-nay-la-du-cho-dan-lap-trinh-phan-1-2020091071827696

https://medium.com/@ratrosy/authorization-and-authentication-in-api-services-9b4db295a35b

[https://medium.com/future-vision/the-great-authenticators-of-rest-738e81109331#:~:text=1.-,Basic%20Authentication,your%20computer%20to%20the%20server.](https://medium.com/future-vision/the-great-authenticators-of-rest-738e81109331#:~:text=1.-,Basic Authentication,your computer to the server.)

> Authentication gets you logged into the server. Authorization gets you access to the resources on the server.

Authentication is negotiated with usernames, passwords, and other *factors* we talked about. Authorization is negotiated based on policies on the account e.g. *administrator* or *user,* or *power user,* etc.

In a similar manner, API services cares little about authorization process; its sole responsibility is to authenticate clients, certifying that they have the clearance to access the resources via the API service.

## Authentication

Deals with determining **who you are**,

Authentication is about proving you are the correct person because you know things.

The client passes the key or the token to the API service, which verifies its validity and responds accordingly.

https://medium.com/@vivekmadurai/different-ways-to-authenticate-a-web-application-e8f3875c254a

https://www.youtube.com/watch?v=mBd-SMPp3kI

https://medium.com/datadriveninvestor/api-design-101-solving-identity-issues-with-authentication-9f98d546131f

## Authorization

 Deals with determining **what you are allowed to do**.

Authorization is asking for permission to do stuff.

The resource owner grants the client access to the API service, usually in the form of a key or a token.

*A user’s default access level to any resource in the system should be “denied” unless they’ve been granted a “permit” explicitly.*

# State-full

## Sessions

https://www.quora.com/What-is-a-session-in-a-Web-Application

https://stackoverflow.com/questions/3804209/what-are-sessions-how-do-they-work

Coming to the technical details, all web applications work through **HTTP** protocol

Because HTTP is **stateless**. That means HTTP doesn't remember or record details of any request

In order to associate a request to any other request, you need a way to store user data between HTTP requests.

This is not enough, as user identity is required for operations followed by login. So a session is created upon user login, and it can be used to hold any information/attributes required between requests until logout.

In simple terms, a session is an **information store unique for logged-in user**

Sessions can be maintained using hidden form fields, cookies, or http session.

### Cookies

https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

Cookies or URL parameters ( for ex. like http://example.com/myPage?asd=lol&boo=no ) are both suitable ways to transport data between 2 or more request. However they are not good in case you don't want that data to be readable/editable on client side.

The solution is to store that data server side, give it an "id", and let the client only know (and pass back at every http request) that id. There you go, sessions implemented. Or you can use the client as a convenient remote storage, but you would encrypt the data and keep the secret server-side.

Of course there are other aspects to consider, like you don't want people to hijack other's sessions, you want sessions to not last forever but to expire, and so on.

In your specific example, the user id (could be username or another unique ID in your user database) is stored in the session data, server-side, after successful identification. Then for every HTTP request you get from the client, the session id (given by the client) will point you to the correct session data (stored by the server) that contains the authenticated user id - that way your code will know what user it is talking to.

![image-20200812213039239](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200812213039239.png)

# Stateless

> Being stateless, the REST API can’t remember your credentials. So you have to tell it who you are every time you talk to it!

https://medium.com/better-programming/json-web-tokens-vs-session-cookies-for-authentication-55a5ddafb435

https://topdev.vn/blog/json-web-token-hay-session-cookies-dau-moi-la-chan-ai/?fbclid=IwAR0UlnfohlwT4ZcreuJWT9skndZRcqmnhfA61SWmn8stIu-I-QhLEcg4wVI

https://anonystick.com/blog-developer/authorization-framework-access-token-refresh-token-cung-giong-viec-sinh-vien-thue-nha-tro-2019061161976500?fbclid=IwAR1YHS5AGwC4bmfUoX6nEMpKXr3idkWsGl10b1KvNFxvHq7hu3m9aBdoqrk

https://nordicapis.com/3-common-methods-api-authentication-explained/

https://zapier.com/engineering/apikey-oauth-jwt/

https://cloud.google.com/endpoints/docs/openapi/when-why-api-key

https://stoplight.io/blog/api-keys-best-practices-to-authenticate-apis/

https://www.freecodecamp.org/news/best-practices-for-building-api-keys-97c26eabfea9/

https://stackoverflow.com/questions/4201431/what-exactly-is-oauth-open-authorization

https://stackoverflow.com/questions/4727226/how-does-oauth-2-protect-against-things-like-replay-attacks-using-the-security-t

https://stackoverflow.com/questions/4113934/how-is-oauth-2-different-from-oauth-1

https://medium.com/google-cloud/understanding-oauth2-and-building-a-basic-authorization-server-of-your-own-a-beginners-guide-cf7451a16f66

https://medium.com/@ratrosy/building-apis-with-openapi-ac3c24e33ee3

https://nordicapis.com/why-cant-i-just-send-jwts-without-oauth/

https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2

https://blogvaronis2.wpengine.com/what-is-oauth/

https://oauth.net/about/introduction/

https://medium.com/@sherryhsu/session-vs-token-based-authentication-11a6c5ac45e4

## HTTP Basic Authentication

Base64 encode username and password (username:password) and put it in the Authentication header.

In this approach, an HTTP user agent simply provides a **username** and **password** to prove their authentication. This approach does not require cookies, session IDs, login pages, and other such specialty solutions, and because it uses the **HTTP header** itself, there’s no need to handshakes or other complex response systems.

```
base64('superuser:sosecure') = c3VwZXJ1c2VyOnNvc2VjdXJl
HTTP header:
Authentication Basic c3VwZXJ1c2VyOnNvc2VjdXJl
```

<img src="https://miro.medium.com/max/1136/1*mW_BXoOpZ0H-Uu_VVPhBFA.png" alt="Image for post" style="zoom:50%;" />

However, base64 encoding is not secure at all, since it's super easy to decode. So it’s a bit like carrying your passport around all the time and using it as your main way of identification. If somebody steals it, they can in easily pretend to be you.

Securing your API using Basic Authentication can only work over TLS/HTTPS but never over HTTP. (Man-In-The-Middle attack)

For APIs on internal networks, Basic Authentication might be appropriate.

For mobile apps and single page apps, basic authentication is not a good scheme because you don’t want to store the username and the password on the device or in the Javascript code of the SPA.



## API keys

An API key is a randomly generated string prepared in a cryptographically strong manner. (generated from their hardware combination and IP data, or **randomly generated** by the server which knows them)

> In this approach, a **unique generated value** is assigned to each first time user, signifying that the user is known.
>
> Each API key corresponds to a record in a database, which specifies the access scopes of the and other related information. 

The client that needs access to the resources needs to register itself with the API. The API generates a Key and a Secret for each registered client. These are then stored with the server, and a copy of these are sent to the client. The client will need to pass these in when they try to connect.

> To use API keys in your API service, first **assign** every developer planning to use your API service a unique API key; every time the developer wants to **access** an API, he or she **passes** the key to the API service as a part of the request. Upon **receiving** an API call, the API service **verifies** the incoming API key, usually by matching it with the key records, and complies with (or declines) the request accordingly.

Other issues with the API key and client secret is the key management. You’ll need to provide a way for them to —

**a.** Generate the key and secret

**b.** Send it to them — this can be done via email or display it in their developer account on the API

**c.** Secure the key and secret

https://www.softwaretestinghelp.com/api-management-tools/

API keys make sense when the users of an API are only developers. However, as developers created tools for themselves, they started sharing them with others.

![API keys overview](https://cloud.google.com/endpoints/docs/images/api_keys_overview.png)

<img src="https://miro.medium.com/max/1136/1*Oa_9KvYFig6iW1RrNUUd6w.png" alt="Image for post" style="zoom:50%;" />

https://nordicapis.com/why-api-keys-are-not-enough/

 API keys are often used for what they’re not – **an API key is not a method of authorization**, it’s a method of authentication.

Because anyone who makes a request of a service transmits their key, in theory, this key can be picked up just as easy as any network transmission, and if any point in the entire network is insecure, the entire network is exposed

## Token based

API Keys and HTTP Basic Authentication **require that credentials be sent to the API service every time an API call is made**. Straightforward as it is, the design is far from ideal, as it incurs unnecessary overhead and poses additional security risks: your API service have to **repeatedly verify** the same set of credentials, and **transmitting sensitive** information over the network **over and over again** increases the chances of security compromises.

Token-based models help address some of the concerns above. Instead of passing credentials, clients now use tokens to access APIs. Tokens are **independent** of user credentials and usually **short lived**, consequently even if a leak happens, the damage can be quickly controlled

In an API service with token-based authentication, the user will type in their username and password (credentials), and the server will generate a token based on those credentials. The server then sends this token out to the use.

Now the user has exchanged the credentials for a server generated token. Now the user doesn’t need to send in login credentials with every request. Instead, they just send in the encoded token.

There are ways to encode date and time into this token so someone intercepting the token cannot re-use it after a certain time.

The user will need to pass this token through either the header, body or as a query parameter when it calls on an endpoint to access resources.

### JSON Web Token

https://trungquandev.com/hieu-sau-ve-jwt-json-web-tokens/?fbclid=IwAR3RqABq9ZJZgiKiYFZnOvbkSmLHU6tjgJzcazYIBfQ_JgaabyCxVQxTXw8

https://tools.ietf.org/html/rfc7519

A JSON web token is an encrypted string that contains information about who you are and JWT **self-contained** as well (your API service can verify them without having to inquire a source.)

It’s kind of like an ID badge for work or school. It will identify you and grant you access to the areas (resources) you have been given access to.

The JWT is generated when you login in and also stored in the Authentication header of every HTTP request.

The token itself is split into three parts. A header contains metadata about the type of encryption and token. The payload contains information about who you are and what permissions you have. Lastly, the signature is the header and the payload encrypted in a hash, so you can ensure the information haven’t been tampered with.

Token can be set to expire after a certain amount of time so users will need to log in again. Tokens can also be revoked from the server side if there has been any compromise.

JWTs don’t need **credentials**, they are **stateless** and they allow for fine-grained access control as well 

![image-20200827011215497](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200827011215497.png)

### Refreshing Token

### OAuth

https://tools.ietf.org/html/rfc6749

OAuth is an authentication standard that uses tokens similar to a JWT, however it delegates dealing with credentials to a third party

OAuth is about authorization and not authentication

Facebook apps are a good OAuth use case example. Say you’re using an app on Facebook, and it asks you to share your profile and pictures. Facebook (service provide) has your login data and your pictures. The app (consumer) , and as the user, you want to use the app to do something with your pictures. You specifically gave this app access to your pictures, which OAuth is managing in the background.

OAuth doesn’t pass authentication data between **consumers** and **service providers** – but instead acts as an authorization token of sorts.

> The valet key allows the valet to start and move the car but doesn’t give them access to the trunk or the glove box.
>
> An **OAuth token** is like that valet **key**. As a **user**, you get to tell the **consumers** what they **can** use and what they **can’t** use from each **service provider**. You can give each **consumer** a **different** valet **key**. They never have the full key or any of the private data that gives them access to the full key.

![OAuth Explained](https://www.varonis.com/blog/wp-content/uploads/2012/04/oauth-explained.png)

This means you could send an authentication request to Google and ask the user to authenticate using their Google credentials, through Google. Then Google will send you a token that authenticates the user and specifies what kind of user information from Google you have access to.

You need OAuth only when you want to enable a user of your service to allow a third-party client application to access his/her data hosted in your service without revealing his/her credentials (ID & password) to the application.

**What a pair of API key & API secret can do is just authentication of a client application.** If it is okay for you to allow an authenticated client application to access a user's data without explicit consent by the user, you don't have to use OAuth.

> ## How OAuth Works
>
> There are 3 main players in an OAuth transaction: the user, the consumer, and the service provider. This triumvirate has been affectionately deemed the OAuth Love Triangle.
>
> In our example, Joe is the user, Bitly is the consumer, and Twitter is the service provided who controls Joe’s secure resource (his Twitter stream). Joe would like Bitly to be able to post shortened links to his stream. Here’s how it works:
>
> **Step 1 – The User Shows Intent**
>
> - **Joe (User):** “Hey, Bitly, I would like you to be able to post links directly to my Twitter stream.”
> - **Bitly (Consumer):** “Great! Let me go ask for permission.”
>
> **Step 2 – The Consumer Gets Permission**
>
> - **Bitly:** “I have a user that would like me to post to his stream. Can I have a request token?”
> - **Twitter (Service Provider):** “Sure. Here’s a token and a secret.”
>
> The secret is used to prevent request forgery.  The consumer uses the secret to sign each request so that the service provider can verify it is actually coming from the consumer application.
>
> **Step 3 – The User Is Redirected to the Service Provider**
>
> - **Bitly:** “OK, Joe. I’m sending you over to Twitter so you can approve. Take this token with you.”
> - **Joe:** “OK!”
>
> **<Bitly directs Joe to Twitter for authorization>**
>
> This is the scary part. If Bitly were super-shady Evil Co. it could pop up a window that looked like Twitter but was really [phishing](https://www.varonis.com/blog/spot-phishing-scam/?__hstc=51647990.b87b6e0f511e43008049d03c97756493.1598468680485.1598468680485.1598468680485.1&__hssc=51647990.1.1598468680485&__hsfp=3849431553) for your username and password. Always be sure to verify that the URL you’re directed to is actually the service provider (Twitter, in this case).
>
> **Step 4 – The User Gives Permission**
>
> - **Joe:** “Twitter, I’d like to authorize this request token that Bitly gave me.”
> - **Twitter:** “OK, just to be sure, you want to authorize Bitly to do X, Y, and Z with your Twitter account?”
> - **Joe:** “Yes!”
> - **Twitter:** “OK, you can go back to Bitly and tell them they have permission to use their request token.”
>
> Twitter marks the request token as “good-to-go,” so when the consumer requests access, it will be accepted (so long as it’s signed using their shared secret).
>
> **Step 5 – The Consumer Obtains an Access Token**
>
> - **Bitly:** “Twitter, can I exchange this request token for an access token?”
> - **Twitter:** “Sure. Here’s your access token and secret.”
>
> **Step 6 – The Consumer Accesses the Protected Resource**
>
> - **Bitly:** “I’d like to post this link to Joe’s stream. Here’s my access token!”
> - **Twitter:** “Done!”
>
> In our scenario, Joe never had to share his Twitter credentials with Bitly. He simply delegated access using OAuth in a secure manner. At any time, Joe can login to Twitter and review the access he has granted and revoke tokens for specific applications without affecting others. OAuth also allows for granular permission levels. You can give Bitly the right to post to your Twitter account, but restrict LinkedIn to read-only access.

> ## OAuth 1.0 vs. OAuth 2.0
>
> OAuth 2.0 is a complete redesign from OAuth 1.0, and the two are not compatible. If you create a new application today, use OAuth 2.0. This blog only applies to OAuth 2.0, since OAuth 1.0 is deprecated.
>
> OAuth 2.0 is faster and easier to implement. OAuth 1.0 used complicated cryptographic requirements, only supported three flows, and did not scale.
>
> OAuth 2.0, on the other hand, has six flows for different types of applications and requirements, and enables signed secrets over HTTPS. OAuth tokens no longer need to be encrypted on the endpoints in 2.0 since they are encrypted in transit.
>
> - **access token**: sent like an API key, it allows the application to access a user’s data; optionally, access tokens can expire.
> - **refresh token**: optionally part of an OAuth flow, refresh tokens retrieve a new access token if they have expired.

## OTP

Phone, email

# Attack



## Injection

Injection là lỗ hổng xảy ra do sự thiếu sót trong việc lọc các dữ liệu đầu vào không đáng tin cậy. Khi bạn truyền các dữ liệu chưa được lọc tới Database (Ví dụ như lỗ hổng [SQL injection](https://resources.cystack.net/news/tan-cong-sql-injection/)), tới trình duyệt (lỗ hổng XSS), tới máy chủ LDAP (lỗ hổng LDAP Injection) hoặc tới bất cứ vị trí nào khác. Vấn đề là kẻ tấn công có thể chèn các đoạn mã độc để gây ra lộ lọt dữ liệu và chiếm quyền kiểm soát trình duyệt của khách hàng.

Để chống lại lỗ hổng này chỉ “đơn giản” là vấn đề bạn đã lọc đầu vào đúng cách chưa hay việc bạn cân nhắc liệu một đầu vào có thể được tin cậy hay không

| Injection attack                                             | Description                                                  | Potential impact                                             |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [**Code injection**](https://www.acunetix.com/blog/articles/code-injection/) | The attacker injects application code written in the application language. This code may be used to execute operating system commands with the privileges of the user who is running the web application. In advanced cases, the attacker may exploit additional privilege escalation vulnerabilities, which may lead to full web server compromise. | Full system compromise                                       |
| [**CRLF injection**](https://www.acunetix.com/websitesecurity/crlf-injection/) | The attacker injects an unexpected CRLF (Carriage Return and Line Feed) character sequence. This sequence is used to split an HTTP response header and write arbitrary contents to the response body. This attack may be combined with Cross-site Scripting (XSS). | Cross-site Scripting (XSS)                                   |
| [**Cross-site Scripting (XSS)**](https://www.acunetix.com/websitesecurity/cross-site-scripting/) | The attacker injects an arbitrary script (usually in JavaScript) into a legitimate website or web application. This script is then executed inside the victim’s browser. | Account impersonationDefacementRun arbitrary JavaScript in the victim’s browser |
| [**Email Header Injection**](https://www.acunetix.com/blog/articles/email-header-injection-web-vulnerability-detection/) | This attack is very similar to CRLF injections. The attacker sends IMAP/SMTP commands to a mail server that is not directly available via a web application. | Spam relayInformation disclosure                             |
| [**Host Header Injection**](https://www.acunetix.com/blog/articles/automated-detection-of-host-header-attacks/) | The attacker abuses the implicit trust of the HTTP Host header to poison password-reset functionality and web caches. | Password-reset poisoningCache poisoning                      |
| [**LDAP Injection**](https://www.acunetix.com/vulnerabilities/web/ldap-injection) | The attacker injects LDAP (Lightweight Directory Access Protocol) statements to execute arbitrary LDAP commands. They can gain permissions and modify the contents of the LDAP tree. | Authentication bypassPrivilege escalationInformation disclosure |
| [**OS Command Injection**](https://www.acunetix.com/blog/web-security-zone/os-command-injection/) | The attacker injects operating system commands with the privileges of the user who is running the web application. In advanced cases, the attacker may exploit additional privilege escalation vulnerabilities, which may lead to full system compromise. | Full system compromise                                       |
| [**SQL Injection (SQLi)**](https://www.acunetix.com/websitesecurity/sql-injection/) | The attacker injects SQL statements that can read or modify database data. In the case of advanced SQL Injection attacks, the attacker can use SQL commands to write arbitrary files to the server and even execute OS commands. This may lead to full system compromise. | Authentication bypassInformation disclosureData lossSensitive data theftLoss of data integrityDenial of serviceFull system compromise. |
| [**XPath injection**](https://www.acunetix.com/vulnerabilities/web/xpath-injection-vulnerability) | The attacker injects data into an application to execute crafted XPath queries. They can use them to access unauthorized data and bypass authentication. | Information disclosureAuthentication bypass                  |

## Broken Authentication

Đây là nhóm các vấn đề có thể xảy ra trong quá trình xác thực. Có một lời khuyên là không nên tự phát triển các giải pháp mã hóa vì rất khó có thể làm được chính xác.

Có rất nhiều rủi ro có thể gặp phải trong quá trình xác thực:

- URL có thể chứa Session ID và rò rỉ nó trong Referer Header của người dùng khác.
- Mật khẩu không được mã hóa hoặc dễ giải mã trong khi lưu trữ.
- Lỗ hổng Session Fixation.
- Tấn công Session Hacking có thể xảy ra khi thời gian hét hạn của session không được triển khai đúng hoặc sử dụng HTTP (không bảo mật SSL)…
- …

## XSS (Cross Site Scripting)

Để khai thác một lỗ hổng XSS, hacker sẽ chèn mã độc thông qua các đoạn script để thực thi chúng ở phía client. Thông thường, các cuộc tấn công XSS được sử dụng để vượt qua các kiểm soát truy cập và mạo danh người dùng.

> ### Reflected XSS
>
> the malicious script comes from the current HTTP request
>
> ![img](https://images.viblo.asia/28e81cf8-c006-4835-9ef0-a8df7d2ccd12.jpg)
>
> ### Stored XSS
>
> the malicious script comes from the website's database
>
> ![img](http://securitydaily.net/wp-content/uploads/2014/03/stored-xss-scenario1.png)
>
> ### DOM Based XSS
>
> the vulnerability exists in client-side code rather than server-side code
>
> In the following example, an application uses some JavaScript to read the value from an input field and write that value to an element within the HTML:
>
> var search = document.getElementById('search').value;
> var results = document.getElementById('results');
> results.innerHTML = 'You searched for: ' + search;
>
> If the attacker can control the value of the input field, they can easily construct a malicious value that causes their own script to execute:
>
> You searched for: `<img src=1 onerror='/* Bad stuff here... */'>`
>
> In a typical case, the input field would be populated from part of the HTTP request, such as a URL query string parameter, allowing the attacker to deliver an attack using a malicious URL, in the same manner as reflected XSS.
>
> ![img](https://images.viblo.asia/cc00ebb0-67cc-4110-91e4-4188104777fd.png)
>
> ### Prevent XSS
>
> - **Filter input on arrival.** At the point where user input is received, filter as strictly as possible based on what is expected or valid input.
> - **Encode data on output.** At the point where user-controllable data is output in HTTP responses, encode the output to prevent it from being interpreted as active content. Depending on the output context, this might require applying combinations of HTML, URL, JavaScript, and CSS encoding.
> - **Use appropriate response headers.** To prevent XSS in HTTP responses that aren't intended to contain any HTML or JavaScript, you can use the Content-Type and X-Content-Type-Options headers to ensure that browsers interpret the responses in the way you intend.
> - **Content Security Policy.** As a last line of defense, you can use Content Security Policy (CSP) to reduce the severity of any XSS vulnerabilities that still occur.
>

## Cross Site Request Forgery (CSRF)

https://www.acunetix.com/websitesecurity/csrf-attacks/

CSRF ( Cross Site Request Forgery) abuse server session and trick user execute malicious code with session of user

=> Protect: only let session available on your views

![image-20200611220835360](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20200611220835360.png)

### Prevent

#### Client side

- Logout
- check url

#### Server side

> ##### Use right HTTP method
>
> GET, POST, PUT, PATCH, DELETE
>
> ##### use CAPCHA, or confirm message
>
> ##### use CSRF token when send data
>
> send token to views and valid that form request in server
>
> ##### Separate cookie for admin site
>
> use "[admin.site.com](http://admin.site.com/)" intead of  "[site.com/admin](http://site.com/admin)"
>
> ##### check IP
>