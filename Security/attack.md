# Injection

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

# Broken Authentication

Đây là nhóm các vấn đề có thể xảy ra trong quá trình xác thực. Có một lời khuyên là không nên tự phát triển các giải pháp mã hóa vì rất khó có thể làm được chính xác.

Có rất nhiều rủi ro có thể gặp phải trong quá trình xác thực:

- URL có thể chứa Session ID và rò rỉ nó trong Referer Header của người dùng khác.
- Mật khẩu không được mã hóa hoặc dễ giải mã trong khi lưu trữ.
- Lỗ hổng Session Fixation.
- Tấn công Session Hacking có thể xảy ra khi thời gian hét hạn của session không được triển khai đúng hoặc sử dụng HTTP (không bảo mật SSL)…
- …

# XSS (Cross Site Scripting)

Để khai thác một lỗ hổng XSS, hacker sẽ chèn mã độc thông qua các đoạn script để thực thi chúng ở phía client. Thông thường, các cuộc tấn công XSS được sử dụng để vượt qua các kiểm soát truy cập và mạo danh người dùng.

## Reflected XSS

the malicious script comes from the current HTTP request

## ![img](https://images.viblo.asia/28e81cf8-c006-4835-9ef0-a8df7d2ccd12.jpg)

## Stored XSS

the malicious script comes from the website's database

![img](http://securitydaily.net/wp-content/uploads/2014/03/stored-xss-scenario1.png)

## DOM Based XSS

the vulnerability exists in client-side code rather than server-side code

In the following example, an application uses some JavaScript to read the value from an input field and write that value to an element within the HTML:

var search = document.getElementById('search').value;
var results = document.getElementById('results');
results.innerHTML = 'You searched for: ' + search;

If the attacker can control the value of the input field, they can easily construct a malicious value that causes their own script to execute:

You searched for: <img src=1 onerror='/* Bad stuff here... */'>

In a typical case, the input field would be populated from part of the HTTP request, such as a URL query string parameter, allowing the attacker to deliver an attack using a malicious URL, in the same manner as reflected XSS.

![img](https://images.viblo.asia/cc00ebb0-67cc-4110-91e4-4188104777fd.png)

## Các cách để ngăn chặn XSS

- **Filter input on arrival.** At the point where user input is received, filter as strictly as possible based on what is expected or valid input.
- **Encode data on output.** At the point where user-controllable data is output in HTTP responses, encode the output to prevent it from being interpreted as active content. Depending on the output context, this might require applying combinations of HTML, URL, JavaScript, and CSS encoding.
- **Use appropriate response headers.** To prevent XSS in HTTP responses that aren't intended to contain any HTML or JavaScript, you can use the Content-Type and X-Content-Type-Options headers to ensure that browsers interpret the responses in the way you intend.
- **Content Security Policy.** As a last line of defense, you can use Content Security Policy (CSP) to reduce the severity of any XSS vulnerabilities that still occur.

# Insecure Direct Object References

Lỗ hổng này xảy ra khi chương trình cho phép người dùng truy cập các tài nguyên (dữ liệu, file, database). Nếu không thực hiện quá trình kiểm soát quyền hạn (hoặc quá trình này không hoàn chỉnh) kẻ tấn công có thể truy cập một cách bất hợp pháp vào các dữ liệu nhạy cảm, quan trọng trên máy chủ.

**Cách ngăn chặn lỗ hổng:** Thực hiện phân quyền người dùng đúng cách và nhất quán với sự áp dụng triệt để các Whitelist.

# Security Misconfiguration

Trong thực tế, máy chủ website và các ứng dụng đa số bị cấu hình sai. Có lẽ do một vài sai sót như:

- Chạy ứng dụng khi chế độ debug được bật.
- Directory listing
- Sử dụng phần mềm lỗi thời (WordPress plugin, PhpMyAdmin cũ)
- Cài đặt các dịch vụ không cần thiết.
- Không thay đổi **default key** hoặc **mật khẩu**
- Trả về lỗi xử lý thông tin cho kẻ tấn công lợi dụng để tấn công, chẳng hạn như *stack traces*.

**Cách ngăn chặn lỗ hổng:**
Có một quá trình “xây dựng và triển khai” tốt (tốt nhất là tự động). Cần một quá trình audit các chính xác bảo mật trên máy chủ trước khi triển khai.

# Cross Site Request Forgery (CSRF)

CSRF ( Cross Site Request Forgery) là kỹ thuật tấn công bằng cách sử dụng quyền chứng thực của người dùng đối với một website. CSRF là kỹ thuật tấn công vào người dùng, dựa vào đó hacker có thể thực thi những thao tác phải yêu cầu sự chứng thực. Hiểu một cách nôm na, đây là kỹ thuật tấn công dựa vào mượn quyền trái phép.

**Hacker sử dụng phương pháp CSRF** để lừa trình duyệt của người dùng gửi đi các câu lệnh http đến các ứng dụng web. Trong trường hợp phiên làm việc của người dùng chưa hết hiệu lực thì các câu lệnh trên sẽ dc thực hiện với quyền chứng thực của người sử dụng.

- Người dùng Alie truy cập 1 diễn đàn yêu thích của mình như thường lệ. Một người dùng khác, Bob đăng tải 1 thông điệp lên diễn đàn. Giả sử rằng Bob có ý đồ không tốt và anh ta muốn xóa đi một dự án quan trọng nào đó mà Alice đang làm.
- Bob sẽ tạo 1 bài viết, trong đó có chèn thêm 1 đoạn code như sau:

```html
<img height="0" width="0" src="http://www.webapp.com/project/1/destroy">
```

## 4.1 Phía user

Để phòng tránh trở thành nạn nhân của các cuộc tấn công CSRF, người dùng internet nên thực hiện một số lưu ý sau:

- Nên thoát khỏi các website quan trọng: Tài khoản ngân hàng, thanh toán trực tuyến, các mạng xã hội, gmail, yahoo… khi đã thực hiện xong giao dịch hay các công việc cần làm. (Check - email, checkin…)
- Không nên click vào các đường dẫn mà bạn nhận được qua email, qua facebook … Khi bạn đưa chuột qua 1 đường dẫn, phía dưới bên trái của trình duyệt thường có địa chỉ website đích, bạn nên lưu ý để đến đúng trang mình muốn.
- Không lưu các thông tin về mật khẩu tại trình duyệt của mình (không nên chọn các phương thức "đăng nhập lần sau", "lưu mật khẩu" …
- Trong quá trình thực hiện giao dịch hay vào các website quan trọng không nên vào các website khác, có thể chứa các mã khai thác của kẻ tấn công.

## 4.2 Phía server

Có nhiều lời khuyến cáo được đưa ra, tuy nhiên cho đến nay vẫn chưa có biện pháp nào có thể phòng chống triệt để CSRF. Sau đây là một vài kĩ thuật sử dụng.

### 4.2.1 Lựa chọn việc sử dụng GET VÀ POST

Sử dụng GET và POST đúng cách. Dùng GET nếu thao tác là truy vấn dữ liệu. Dùng POST nếu các thao tác tạo ra sự thay đổi hệ thống (theo khuyến cáo của W3C tổ chức tạo ra chuẩn http) Nếu ứng dụng của bạn theo chuẩn RESTful, bạn có thể dùng thêm các HTTP verbs, như PATCH, PUThay DELETE

### 4.2.2 Sử dụng captcha, các thông báo xác nhận

Captcha được sử dụng để nhận biết đối tượng đang thao tác với hệ thống là con người hay không? Các thao tác quan trọng như "đăng nhập" hay là "chuyển khoản" ,"thanh toán" thường là hay sử dụng captcha. Tuy nhiên, việc sử dụng captcha có thể gây khó khăn cho một vài đối tượng người dùng và làm họ khó chịu. Các thông báo xác nhận cũng thường được sử dụng, ví dụ như việc hiển thị một thông báo xác nhận "bạn có muốn xóa hay k" cũng làm hạn chế các kĩ thuật Cả hai cách trên vẫn có thể bị vượt qua nếu kẻ tấn công có một kịch bản hoàn hảo và kết hợp với lỗi XSS.

### 4.2.3 Sử dụng token

Tạo ra một token tương ứng với mỗi form, token này sẽ là duy nhất đối với mỗi form và thường thì hàm tạo ra token này sẽ nhận đối số là"SESSION" hoặc được lưu thông tin trong SESSION. Khi nhận lệnh HTTP POST về, hệ thống sẽ thực hiên so khớp giá trị token này để quyết định có thực hiện hay không. Mặc định trong Rails, khi tạo ứng dụng mới:

```ruby
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception
end
```

Khi đó tất cả các form và Ajax request được tự động thêm sercurity token generate bởi Rails. Nếu security token không khớp, exception sẽ được ném ra.

### 4.2.4 Sử dụng cookie riêng biệt cho trang quản trị

Một cookie không thể dùng chung cho các domain khác nhau,chính vì vậy việc sử dụng "[admin.site.com](http://admin.site.com/)" thay vì sử dụng "[site.com/admin](http://site.com/admin)" là an toàn hơn.

### 4.2.5 Kiểm tra REFERRER

Kiểm tra xem các câu lệnh http gửi đến hệ thống xuất phát từ đâu. Một ứng dụng web có thể hạn chế chỉ thực hiện các lệnh http gửi đến từ các trang đã được chứng thực. Tuy nhiên cách làm này có nhiều hạn chế và không thật sự hiệu quả.

### 4.2.6 Kiểm tra IP

Một số hệ thống quan trọng chỉ cho truy cập từ những IP được thiết lập sẵn