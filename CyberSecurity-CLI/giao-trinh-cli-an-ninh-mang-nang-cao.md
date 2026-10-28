# Giáo trình CLI An Ninh Mạng Nâng Cao, Phần 2

> Phần 2, nối tiếp file `giao-trinh-cli-an-ninh-mang.md`, phần 1.
> Phần 1 là nền tảng tới trung cấp. Phần 2 là chuyên sâu, dành cho người
> đã chạy thành thạo nmap, curl, đọc log, viết script Bash cơ bản, và
> đã có PC Linux hoặc máy ảo lab riêng. Nếu bạn chưa xong phần 1, hãy
> học xong phần 1 trước, rồi mới học phần 2, vì phần 2 giả định bạn đã
> biết các lệnh cơ bản và cách đọc kết quả.
>
> Quy tắc an toàn giữ nguyên, và càng quan trọng hơn ở mức nâng cao:
> mọi lệnh quét, khai thác, thử mật khẩu, thử lỗ hổng trong phần 2 chỉ
> dùng cho hệ thống của chính bạn, mạng lab riêng, máy ảo lab, hoặc
> phạm vi pentest có hợp đồng bằng văn bản. Không chạy các lệnh này
> lên hệ thống, website, WiFi, tài khoản của người khác khi chưa có phép.
> Các tool tấn công mạnh, nếu dùng sai mục tiêu, sẽ gây khóa tài khoản,
> quá tải, mất dữ liệu, và vi phạm pháp luật. Trong phần 2, mỗi chương
> đều có phần phòng thủ và phát hiện đi kèm, vì người làm an ninh giỏi
> phải biết cả hai phía: tấn công làm gì, phòng thủ chặn và phát hiện
> bằng cách nào.

## Cách dùng phần 2

- Học theo thứ tự từ Chương A1 tới A13 nếu bạn muốn đi theo lộ trình
  pentest và SOC chuyên nghiệp.
- Nếu bạn đã chọn hướng Blue Team, hãy ưu tiên A4, A8, A9, A10.
- Nếu bạn đã chọn hướng Red Team có phép, hãy ưu tiên A1, A2, A5, A6,
  A12, nhưng vẫn phải học A8 và A9 để hiểu phòng thủ phát hiện bạn
  bằng cách nào.
- Mỗi chương có: khái niệm nâng cao, lệnh và công cụ, ví dụ trong lab,
  cách phòng thủ và phát hiện, lỗi và cạm bẫy, bài tập.
- Quy ước: `lab.local`, `may-ao-lab`, `192.168.2.0/24` là lab riêng.
  `scanme.nmap.org` chỉ dùng cho quét nmap cơ bản và script an toàn,
  không dùng cho khai thác, đoán mật khẩu, hoặc quét dồn dập.

## Mục lục phần 2

- Chương A1: Phương pháp pentest và báo cáo chuyên nghiệp
- Chương A2: Nmap nâng cao và tự viết script NSE
- Chương A3: Mạng nâng cao, VPN, TLS và IPv6
- Chương A4: Giám sát mạng nâng cao, Zeek, Suricata và phân tích gói sâu
- Chương A5: Bảo mật web nâng cao
- Chương A6: Leo thang đặc quyền và Active Directory ở mức khái niệm lab
- Chương A7: Mật mã nâng cao, PKI và TLS
- Chương A8: SOC, SIEM, Threat Hunting và MITRE ATT&CK
- Chương A9: Điều tra số và phân tích mã độc nâng cao
- Chương A10: Cloud, container và Kubernetes qua CLI
- Chương A11: Mobile và wireless nâng cao, chỉ trên thiết bị và mạng của bạn
- Chương A12: Tự động hóa chuyên sâu và framework pentest trong lab
- Chương A13: Lộ trình chuyên sâu, CTF, chứng chỉ và bài tập tổng hợp nâng cao
- Phụ lục nâng cao: Bảng tra cứu và lỗi thường gặp

---

# Chương A1: Phương pháp pentest và báo cáo chuyên nghiệp

## A1.1. Các giai đoạn pentest, PTES

PTES, Penetration Testing Execution Standard, chia pentest thành:

1. Thu thập thông tin trước khi tương tác, Pre-engagement: ký hợp đồng,
   xác định phạm vi, IP, tên miền, thời gian, quy tắc ứng xử, người liên
   hệ khi có sự cố, cách dừng khẩn cấp.
2. Thu thập thông tin, Intelligence Gathering: tra cứu công khai, DNS,
   whois, quét cổng, banner, trong phạm vi đã ký.
3. Mô hình đe dọa, Threat Modeling: liệt kê tài sản quan trọng nhất,
   kẻ tấn công có thể là ai, con đường nào khả thi nhất.
4. Phân tích lỗ hổng, Vulnerability Analysis: đối chiếu phiên bản với
   CVE, kiểm tra cấu hình, đọc tài liệu, chạy scanner trong lab hoặc
   trong phạm vi.
5. Khai thác, Exploitation: chỉ khai thác khi đã có phép và đã đánh giá
   rủi ro, ưu tiên khai thác an toàn, không gây mất dữ liệu, dừng ngay
   khi hệ thống có dấu hiệu quá tải.
6. Hậu khai thác, Post-Exploitation: xem quyền đã chiếm được tới đâu,
   có lan sang hệ thống khác không, nhưng không đọc dữ liệu nhạy cảm
   ngoài phạm vi, không giữ quyền truy cập sau khi kết thúc.
7. Báo cáo, Reporting: viết rõ lỗ hổng, mức độ, bằng chứng, cách tái
   hiện trong lab, cách sửa, thứ tự ưu tiên sửa.

OWASP Testing Guide là tài liệu tương tự cho web, chia theo từng loại
kiểm tra: thu thập thông tin, cấu hình, xác thực, quản lý session,
kiểm tra đầu vào, xử lý lỗi, mã hóa, logic nghiệp vụ, phía client.

## A1.2. Phân loại mức độ và CVSS ở mức hiểu

- Critical, High, Medium, Low, Informational: mức độ dựa vào tác động
  và khả năng khai thác, không dựa vào cảm giác.
- CVSS là hệ thống chấm điểm lỗ hổng, gồm vector như Attack Vector,
  Attack Complexity, Privileges Required, User Interaction, Scope,
  Confidentiality, Integrity, Availability. Bạn không cần thuộc lòng
  công thức, nhưng cần biết đọc một điểm CVSS và giải thích vì sao
  một lỗ hổng lại được chấm cao hay thấp.
- Một lỗ hổng có điểm cao nhưng ở hệ thống không mở ra Internet và
  đã có tường lửa chặn thì rủi ro thực tế có thể thấp hơn, và ngược lại,
  một lỗ hổng điểm trung bình nhưng ở hệ thống thanh toán mở ra Internet
  thì phải sửa trước. Báo cáo tốt luôn ghi cả điểm và ngữ cảnh.

## A1.3. Cách viết báo cáo pentest

Cấu trúc báo cáo tốt:

- Tóm tắt cho lãnh đạo: phạm vi, thời gian, kết quả chính, số lỗ hổng
  theo mức độ, rủi ro lớn nhất, khuyến nghị ưu tiên, không dùng thuật
  ngữ chuyên sâu ở phần này.
- Phương pháp: tool đã dùng, phạm vi, thời gian, giới hạn, ví dụ không
  khai thác hệ thống sản xuất vào giờ cao điểm.
- Chi tiết từng lỗ hổng: tiêu đề, mức độ, CVE nếu có, mô tả, bằng chứng,
  ảnh hưởng, cách tái hiện trong lab, cách sửa, tài liệu tham khảo.
- Phụ lục: danh sách IP và cổng đã quét, lệnh đã chạy, log rút gọn,
  đã che thông tin nhạy cảm.

Nguyên tắc báo cáo: viết đủ để đội sửa chữa tái hiện được, nhưng không
viết hướng dẫn tấn công chi tiết lên hệ thống thật, không đính kèm mật
khẩu thật, khóa riêng, dữ liệu khách hàng. Báo cáo pentest là tài liệu
nhạy cảm, chỉ gửi cho người có quyền trong phạm vi hợp đồng.

## A1.4. Bài tập A1

1. Viết một hợp đồng pentest giả định cho lab của bạn: phạm vi, thời
   gian, IP lab, quy tắc dừng, người liên hệ.
2. Chọn một lỗ hổng web trong lab PortSwigger, viết báo cáo một trang
   theo cấu trúc trên, gồm mô tả, ảnh hưởng, cách sửa.
3. Đọc một báo cáo CVE công khai, ghi lại CVSS, vector, và giải thích
   vì sao điểm đó cao hay thấp.

---

# Chương A2: Nmap nâng cao và tự viết script NSE

## A2.1. Quét nâng cao, evasion và timing ở mức khái niệm

```bash
sudo nmap -sS -T2 scanme.nmap.org
sudo nmap -sS -f scanme.nmap.org
sudo nmap -sS -D RND:5 scanme.nmap.org
sudo nmap -sS --source-port 53 scanme.nmap.org
sudo nmap -sS --data-length 20 scanme.nmap.org
nmap --top-ports 100 -sV -sC -oA ketqua scanme.nmap.org
```

- `-f`: chia nhỏ gói, fragment, để tránh một số hệ thống phát hiện đơn
  giản. Không phải lúc nào cũng hiệu quả, và có thể làm quét chậm hơn.
- `-D`: decoy, gửi thêm gói từ IP giả để che IP thật. `RND:5` nghĩa là
  5 IP ngẫu nhiên. Chỉ dùng trong lab, vì decoy có thể làm người khác
  bị nghi oan là kẻ tấn công, và có thể vi phạm quy tắc pentest nếu
  hợp đồng không cho phép.
- `--source-port`: đổi cổng nguồn, ví dụ 53, vì một số tường lửa cho
  qua gói từ cổng DNS. Đây là kỹ thuật né tường lửa cơ bản, cần hiểu
  rõ mới dùng trong lab.
- `--data-length`: thêm dữ liệu giả vào gói để tránh phát hiện theo kích
  thước gói.
- Evasion không phải là phép màu. Tường lửa hiện đại, IDS và SIEM vẫn
  phát hiện được quét dồn dập, quét lạ, và script bất thường bằng hành
  vi, không chỉ bằng một gói tin. Học evasion để hiểu phòng thủ, không
  phải để lén quét hệ thống người khác.

## A2.2. Các kiểu quét TCP nâng cao

```bash
sudo nmap -sX scanme.nmap.org
sudo nmap -sF scanme.nmap.org
sudo nmap -sN scanme.nmap.org
sudo nmap -sA scanme.nmap.org
sudo nmap -sM scanme.nmap.org
```

- `-sX`: Xmas, gửi gói có cờ FIN, PSH, URG cùng lúc, giống cây Giáng sinh
  nhiều đèn. Hệ thống tuân theo RFC sẽ trả RST cho cổng đóng và không
  trả gì cho cổng mở.
- `-sF`: FIN, chỉ gửi cờ FIN.
- `-sN`: Null, không gửi cờ nào.
- `-sA`: ACK, dùng để xác định tường lửa chặn hay không, không xác định
  cổng mở hay đóng.
- `-sM`: Maimon, gửi FIN và ACK cùng lúc.
- Các kiểu này cần root, và không hiệu quả với Windows, vì Windows trả
  RST cho mọi trường hợp, không phân biệt mở hay đóng. Đây là ví dụ
  điển hình: cùng một lệnh, kết quả khác nhau tùy hệ điều hành mục tiêu,
  nên bạn phải hiểu cơ chế TCP trước khi tin kết quả.

## A2.3. Quét UDP nâng cao và tối ưu thời gian

```bash
sudo nmap -sU --top-ports 20 scanme.nmap.org
sudo nmap -sU -p 53,67,68,69,123,161,500,520,1900 scanme.nmap.org
sudo nmap -sU --host-timeout 10m scanme.nmap.org
```

- UDP không có bắt tay, nên quét UDP chậm và hay cho kết quả
  `open|filtered`. Muốn nhanh hơn, hãy quét chỉ các cổng UDP phổ biến
  nhất, đừng quét hết 65535 cổng UDP khi chưa cần.
- `--host-timeout`: giới hạn thời gian cho một host, tránh kẹt quá lâu
  ở một máy chậm trong khi còn nhiều máy khác cần quét.
- Kết hợp `-sU` với `-sV` và `-sC` để xác nhận dịch vụ UDP thật, vì
  chỉ dựa vào số cổng là không đủ.

## A2.4. Tự viết script NSE cơ bản

Script NSE viết bằng Lua, lưu trong thư mục script của nmap, ví dụ trên
PC Linux là `/usr/share/nmap/scripts/`, trên Termux là
`$PREFIX/share/nmap/scripts/`. Đừng sửa script có sẵn, hãy tạo file mới
với tên riêng trong lab.

Cấu trúc tối thiểu của một script lấy banner:

```lua
description = "Lay banner don gian trong lab"
author = "Lab"
license = "Same as Nmap"
categories = {"discovery", "safe"}

portrule = function(host, port)
  return port.protocol == "tcp" and port.state == "open"
end

action = function(host, port)
  local socket = nmap.new_socket()
  socket:connect(host.ip, port.number)
  local status, data = socket:receive()
  socket:close()
  if status then
    return "Banner: " .. data
  else
    return "Khong lay duoc banner"
  end
end
```

- `description`, `author`, `categories`: thông tin script, category
  `safe` nghĩa là script an toàn, không làm crash dịch vụ.
- `portrule`: quyết định script chạy cho cổng nào, ví dụ chỉ cổng TCP
  đang mở.
- `action`: việc script làm, ở đây là mở socket, nhận banner, đóng socket.
- Chạy script tự viết trong lab:
  `nmap --script ten-script-cua-ban -p 80 lab.local`
- Đừng viết script brute force, exploit, hoặc script gửi hàng loạt
  request lên hệ thống thật. Script lab nên chỉ đọc, không ghi, không
  thay đổi trạng thái hệ thống mục tiêu.

## A2.5. Xử lý kết quả nmap bằng Python

```bash
nmap -oX ketqua.xml -p 22,80,9929 -sV scanme.nmap.org
python3 - << 'PY'
import xml.etree.ElementTree as ET
tree = ET.parse("ketqua.xml")
root = tree.getroot()
for host in root.findall("host"):
    for port in host.findall("ports/port"):
        print(port.get("portid"), port.find("state").get("state"))
PY
```

- XML là định dạng tốt nhất để script đọc, vì cấu trúc rõ ràng, không
  phải đoán bằng regex trên output chữ thường.
- Trong báo cáo chuyên nghiệp, bạn sẽ chuyển XML thành bảng, biểu đồ,
  hoặc đưa vào công cụ quản lý lỗ hổng, thay vì dán nguyên output dài
  vào báo cáo.

## A2.6. Bài tập A2

1. Trong lab, chạy `-sX`, `-sF`, `-sN` lên một máy Linux lab và một máy
   Windows lab, so sánh kết quả và giải thích vì sao khác nhau.
2. Viết một script NSE đơn giản chỉ lấy banner, chạy trong lab, ghi lại
   kết quả.
3. Quét XML rồi dùng Python đọc lại, in ra danh sách cổng mở theo từng
   host, không dùng `cat` và đọc bằng mắt.

---

# Chương A3: Mạng nâng cao, VPN, TLS và IPv6

## A3.1. VLAN và phân đoạn nâng cao

- VLAN, Virtual LAN, chia một switch vật lý thành nhiều mạng logic.
  Máy ở VLAN khác nhau không nói chuyện trực tiếp được nếu không qua
  router hoặc tường lửa, dù cắm chung một switch.
- VLAN hopping là kỹ thuật tấn công để nhảy từ VLAN này sang VLAN khác
  bằng cách giả mạo tag VLAN, chỉ nghiên cứu trong lab với switch lab
  riêng, và phòng thủ bằng cách tắt cổng trunk không dùng, đặt VLAN
  native riêng, không để cổng access nhận tag kép.
- Trong lab, bạn mô phỏng phân đoạn bằng nhiều subnet và quy tắc tường
  lửa, trước khi chạm vào switch thật.

## A3.2. VPN qua CLI, WireGuard và OpenVPN

WireGuard, gọn và hiện đại:

```bash
wg genkey | tee privatekey | wg pubkey > publickey
wg show
sudo wg-quick up wg0
sudo wg-quick down wg0
```

- `wg genkey`: tạo khóa riêng, `wg pubkey`: tạo khóa công khai từ khóa
  riêng. Khóa riêng tuyệt đối không chia sẻ, giống khóa SSH.
- `wg-quick up wg0`: bật tunnel theo file cấu hình `wg0.conf`, trong
  đó có địa chỉ, khóa đối tác, AllowedIPs, Endpoint.
- AllowedIPs quyết định lưu lượng nào đi qua tunnel, ví dụ `0.0.0.0/0`
  là tất cả, `10.0.0.0/24` là chỉ mạng VPN.

OpenVPN, truyền thống và phổ biến trong doanh nghiệp:

```bash
openvpn --config client.ovpn
openvpn --config server.conf
```

- File `.ovpn` chứa chứng chỉ, khóa, địa chỉ máy chủ, cổng, giao thức.
  Đừng chia sẻ file `.ovpn` có chứa khóa riêng lên GitHub hoặc chat.
- VPN không tự nó làm bạn ẩn danh hoàn toàn. VPN mã hóa đường truyền
  tới máy chủ VPN, nhưng máy chủ VPN vẫn thấy lưu lượng của bạn, và
  website vẫn thấy IP của máy chủ VPN. Đừng tin quảng cáo VPN nói bạn
  hoàn toàn ẩn danh.

## A3.3. TLS sâu hơn

```bash
openssl s_client -connect example.com:443 -servername example.com
openssl s_client -connect example.com:443 -tls1_2
nmap --script ssl-enum-ciphers -p 443 example.com
testssl.sh https://example.com
```

- `-servername`: SNI, tên miền bạn muốn máy chủ trả chứng chỉ cho, quan
  trọng khi một IP phục vụ nhiều website.
- `ssl-enum-ciphers`: script nmap liệt kê các bộ mã hóa TLS mà máy chủ
  hỗ trợ, để phát hiện máy chủ còn hỗ trợ mã hóa yếu, TLS 1.0, 1.1, hoặc
  bộ mã hóa không có Perfect Forward Secrecy.
- `testssl.sh`: tool kiểm tra TLS toàn diện trong lab, cần tải từ nguồn
  chính thức, chỉ chạy lên website bạn có phép.
- Phòng thủ TLS: tắt TLS 1.0 và 1.1, tắt bộ mã hóa yếu và RC4, bật HSTS,
  dùng chứng chỉ từ nhà phát hành tin cậy, tự động gia hạn bằng
  Certbot hoặc ACME, kiểm tra ngày hết hạn bằng script và cảnh báo
  trước 30 ngày.

## A3.4. DNSSEC, DoH và DoT

- DNSSEC: ký số bản ghi DNS để chống giả mạo DNS, kiểm tra bằng
  `dig +dnssec example.com`, xem có bản ghi RRSIG và DNSKEY không.
- DoH, DNS over HTTPS, và DoT, DNS over TLS: mã hóa truy vấn DNS để
  chống nghe lén và giả mạo trên đường truyền, nhưng máy chủ DNS bạn
  chọn vẫn thấy truy vấn của bạn. Chọn máy chủ DNS đáng tin, và hiểu
  rõ chính sách lưu log của họ.
- Phòng thủ DNS trong doanh nghiệp: chặn DNS ra ngoài không qua máy chủ
  DNS nội bộ, ghi log truy vấn DNS để phát hiện tên miền lạ, tên miền
  mới đăng ký, tên miền ngẫu nhiên dài bất thường.

## A3.5. IPv6 và bảo mật IPv6

```bash
ip -6 a
ip -6 r
ping6 -c 4 ::1
nmap -6 scanme.nmap.org
```

- IPv6 có không gian địa chỉ rất lớn, quét brute force toàn bộ dải IPv6
  là không khả thi như IPv4, nên kẻ tấn công thường dùng DNS, log, hoặc
  địa chỉ từ SLAAC để tìm máy.
- SLAAC và Neighbor Discovery có thể bị giả mạo tương tự ARP spoofing
  trong IPv4, phòng thủ bằng RA Guard trên switch, SEND, và tường lửa
  IPv6 riêng, đừng chỉ cấu hình tường lửa IPv4 rồi quên IPv6.
- Khi bạn bật IPv6 trên máy chủ, hãy kiểm tra tường lửa IPv6 bằng
  `ip6tables` hoặc `nftables`, vì nhiều hệ thống mặc định mở IPv6 dù
  bạn chỉ cấu hình chặn IPv4.

## A3.6. Bài tập A3

1. Trong lab, tạo một cặp khóa WireGuard, ghi lại khóa công khai, và
   giải thích vì sao khóa riêng không được chia sẻ.
2. Dùng `openssl s_client` kiểm tra một website lab, ghi lại phiên bản
   TLS, bộ mã hóa, ngày hết hạn chứng chỉ.
3. Chạy `nmap --script ssl-enum-ciphers` lên một máy chủ lab có HTTPS,
   liệt kê bộ mã hóa yếu nếu có, và viết cách tắt chúng.

---
# Chương A4: Giám sát mạng nâng cao, Zeek, Suricata và phân tích gói sâu

## A4.1. Zeek, trước đây là Bro

```bash
zeek -i eth0
zeek -r bat-goi.pcap
cat conn.log
cat dns.log
cat http.log
```

- Zeek không chỉ bắt gói, nó phân tích giao thức và ghi log có cấu trúc
  theo từng loại: `conn.log` cho kết nối, `dns.log` cho DNS, `http.log`
  cho HTTP, `ssl.log` cho TLS, `files.log` cho file truyền qua mạng.
- Trong SOC, Zeek là nguồn log rất giá trị, vì bạn có thể hỏi: IP nào
  kết nối ra ngoài lúc 3 giờ sáng, tên miền nào được hỏi nhiều nhất,
  file nào được tải về, chứng chỉ TLS nào lạ.
- Zeek cần PC lab đủ mạnh, không chạy tốt trên điện thoại, nên phần này
  bạn học lý thuyết và đọc log mẫu trước, lên PC mới chạy trực tiếp.

## A4.2. Suricata và Snort, IDS và IPS

```bash
suricata -i eth0 -c /etc/suricata/suricata.yaml
suricata -r bat-goi.pcap -c /etc/suricata/suricata.yaml
snort -i eth0 -c /etc/snort/snort.conf -A console
```

- IDS, Intrusion Detection System: chỉ phát hiện và cảnh báo, không chặn.
  IPS, Intrusion Prevention System: phát hiện và chặn ngay.
- Suricata và Snort dùng rule để phát hiện, ví dụ rule phát hiện quét
  cổng, rule phát hiện tên miền mã độc, rule phát hiện SQL injection
  trong HTTP.
- Ví dụ rule Snort đơn giản, chỉ mang tính minh họa trong lab:

```text
alert tcp any any -> any 80 (msg:"HTTP request to lab"; content:"GET"; http_method; sid:1000001;)
```

- `alert`: hành động, `tcp`: giao thức, `any any -> any 80`: từ bất kỳ
  đâu tới cổng 80, `content`: mẫu cần tìm, `sid`: ID của rule, phải
  duy nhất.
- Viết rule tốt là nghệ thuật: rule quá rộng sẽ báo động giả liên tục,
  rule quá hẹp sẽ bỏ sót tấn công thật. Trong SOC, bạn sẽ điều chỉnh
  rule theo log thật, theo phản hồi của đội ứng cứu, không viết rule
  một lần rồi bỏ đó mãi.

## A4.3. Phân tích gói sâu với tshark

```bash
tshark -r bat-goi.pcap -Y "http.request"
tshark -r bat-goi.pcap -Y "dns.flags.response == 0"
tshark -r bat-goi.pcap -Y "tls.handshake.type == 1"
tshark -r bat-goi.pcap -T fields -e frame.time -e ip.src -e ip.dst -e http.host -e http.request.uri
```

- `http.request`: chỉ hiện request HTTP, không hiện response.
- `dns.flags.response == 0`: chỉ hiện truy vấn DNS, không hiện trả lời.
- `tls.handshake.type == 1`: chỉ hiện ClientHello trong bắt tay TLS,
  để xem tên miền SNI, phiên bản TLS, bộ mã hóa client đề xuất.
- Khi bạn có keylog TLS của chính lab bạn, ví dụ file do trình duyệt
  lab tạo ra với biến môi trường `SSLKEYLOGFILE`, bạn có thể giải mã
  lưu lượng TLS trong lab để học, nhưng đừng bao giờ thu thập keylog
  của người khác, và đừng dùng keylog để đọc lưu lượng thật của người
  khác.

## A4.4. Phát hiện kênh ẩn và DNS tunneling

Dấu hiệu cần chú ý, không phải kết luận ngay:

- Tên miền con rất dài, ngẫu nhiên, thay đổi liên tục, ví dụ
  `a1b2c3d4e5f6.example.com`, `x9y8z7.example.com`.
- Số lượng truy vấn DNS tăng đột ngột, nhiều truy vấn TXT hoặc NULL.
- Lưu lượng DNS ra ngoài lớn bất thường, vì DNS bình thường chỉ hỏi
  tên miền ngắn, nhận IP ngắn.
- Kết nối ra ngoài vào giờ lạ, tới IP chưa từng thấy, với cổng lạ.
- Phòng thủ: ghi log DNS đầy đủ, chặn truy vấn DNS ra ngoài không qua
  máy chủ nội bộ, dùng danh sách tên miền độc hại đã biết, và điều tra
  tay trước khi chặn một tên miền mà hệ thống đang cần.

## A4.5. Bài tập A4

1. Dùng tshark đọc một file pcap lab, lọc chỉ request HTTP, ghi lại
   5 host và URI đầu tiên.
2. Viết một rule Suricata hoặc Snort đơn giản phát hiện request HTTP
   tới một đường dẫn lab cụ thể, chạy thử trong lab, kiểm tra cảnh báo
   có hiện không.
3. Đọc một file `dns.log` mẫu của Zeek, tìm tên miền nào được hỏi
   nhiều nhất, và giải thích vì sao bạn nghi ngờ hoặc không nghi ngờ
   tên miền đó.

---

# Chương A5: Bảo mật web nâng cao

Chương này chỉ dùng lab PortSwigger, DVWA, Juice Shop tự dựng. Mọi
ví dụ đều ở mức khái niệm và phòng thủ, không phải hướng dẫn tấn công
website thật. Khi bạn làm lab, hãy ghi lại cơ chế và cách sửa, không
ghi lại payload để dùng lên hệ thống thật.

## A5.1. SQL injection nâng cao

Các dạng, ở mức khái niệm:

- Union-based: dùng `UNION SELECT` để gộp kết quả từ bảng khác vào
  kết quả hiện tại, khi ứng dụng hiện kết quả trực tiếp.
- Error-based: dùng lỗi cơ sở dữ liệu để lộ thông tin, khi ứng dụng
  hiện chi tiết lỗi.
- Blind boolean-based: ứng dụng không hiện dữ liệu, chỉ hiện đúng hoặc
  sai, kẻ tấn công đoán từng ký tự bằng câu hỏi đúng sai.
- Blind time-based: ứng dụng không hiện gì khác biệt, kẻ tấn công đo
  thời gian phản hồi để đoán đúng sai, ví dụ dùng `SLEEP(5)`.
- Phòng thủ duy nhất đáng tin: dùng prepared statement và tham số hóa
  cho mọi truy vấn, không nối chuỗi từ người dùng vào SQL, giới hạn
  quyền của tài khoản cơ sở dữ liệu mà web dùng, ví dụ tài khoản web
  chỉ được SELECT trên bảng cần thiết, không được DROP, không được
  đọc bảng user hệ thống, bật WAF chỉ là lớp phụ, không thay thế cho
  prepared statement.

## A5.2. XSS nâng cao

- Reflected XSS: payload phản chiếu ngay trong response, cần nạn nhân
  bấm vào link độc.
- Stored XSS: payload lưu trong cơ sở dữ liệu, hiện cho mọi người xem
  trang đó, nguy hiểm hơn vì không cần nạn nhân bấm link riêng.
- DOM-based XSS: payload xử lý ở phía trình duyệt bằng JavaScript,
  máy chủ không thấy rõ, cần kiểm tra code JavaScript phía client.
- Phòng thủ: mã hóa output theo ngữ cảnh, ví dụ mã hóa HTML, mã hóa
  thuộc tính, mã hóa JavaScript khác nhau, dùng Content Security Policy
  để hạn chế nguồn script, đặt cookie với cờ HttpOnly để script không
  đọc được cookie session, kiểm tra đầu vào nhưng không chỉ dựa vào
  kiểm tra đầu vào.

## A5.3. SSRF, SSTI, XXE và deserialization

- SSRF: máy chủ bị lừa gửi request tới hệ thống nội bộ, ví dụ
  `http://169.254.169.254/` là địa chỉ metadata của cloud, hoặc
  `http://localhost:8080/admin`. Phòng thủ: kiểm tra URL, chặn IP nội
  bộ, chặn dải IP cloud metadata, dùng danh sách cho phép tên miền.
- SSTI, Server-Side Template Injection: dữ liệu người dùng được đưa
  vào template engine mà không kiểm tra, máy chủ render template thành
  code. Phòng thủ: không đưa dữ liệu người dùng vào template dưới dạng
  code, dùng template an toàn, tắt tính năng nguy hiểm của template.
- XXE, XML External Entity: file XML chứa thực thể ngoài, máy chủ đọc
  file hệ thống khi phân tích XML. Phòng thủ: tắt xử lý thực thể ngoài,
  dùng parser an toàn, không nhận XML từ nguồn không tin cậy.
- Deserialization không an toàn: dữ liệu đã serialize từ người dùng
  được giải serialize thành đối tượng và thực thi code. Phòng thủ: không
  nhận đối tượng đã serialize từ người dùng, dùng định dạng dữ liệu
  đơn giản như JSON với kiểm tra kiểu, ký số dữ liệu nếu phải truyền
  đối tượng.

## A5.4. JWT, OAuth và API security

- JWT gồm 3 phần nối bằng dấu chấm: header, payload, signature.
  Header nói thuật toán, payload chứa claim như user, hết hạn, signature
  là chữ ký để kiểm tra toàn vẹn.
- Lỗi JWT thường gặp trong lab: thuật toán `none` được chấp nhận, khóa
  ký yếu hoặc công khai, không kiểm tra hết hạn, không kiểm tra chữ ký.
  Phòng thủ: chỉ chấp nhận thuật toán mạnh như RS256 hoặc HS256 với
  khóa mạnh, kiểm tra chữ ký, kiểm tra hết hạn, kiểm tra issuer và
  audience, không lưu thông tin nhạy cảm trong payload vì payload chỉ
  mã hóa base64, không phải mã hóa bí mật.
- OAuth: luồng ủy quyền, không phải đăng nhập trực tiếp. Lỗi thường gặp:
  redirect_uri không kiểm tra, state không kiểm tra dẫn tới CSRF, token
  lộ trong URL, scope quá rộng. Phòng thủ: kiểm tra redirect_uri theo
  danh sách cho phép, dùng state và PKCE, không lưu token trong
  localStorage nếu có thể dùng cookie HttpOnly và SameSite.
- API security: xác thực mọi endpoint, kiểm tra quyền theo từng đối
  tượng, giới hạn tốc độ, phân trang, kiểm tra kiểu dữ liệu, ghi log
  truy cập API, không trả về dữ liệu thừa trong response, ví dụ trả về
  cả mật khẩu đã băm hoặc token nội bộ.

## A5.5. Các tool web nâng cao trong lab

- Burp Suite: proxy chặn và sửa request, repeater để thử lại, intruder
  để thử nhiều giá trị, chỉ dùng bản Community trong lab, không dùng
  để tấn công website thật.
- ZAP, OWASP Zed Attack Proxy: tương tự Burp, mã nguồn mở, có chế độ
  dòng lệnh và API để tự động hóa quét trong pipeline lab.
- Nuclei: chạy mẫu kiểm tra lỗ hổng đã biết, mẫu viết bằng YAML, chỉ
  chạy trong lab và phạm vi, đừng chạy nuclei với tất cả mẫu lên hệ
  thống thật khi chưa hiểu mẫu đó làm gì.
- SQLMap, ở mức nâng cao, bạn cần hiểu từng tùy chọn: `--dbs` liệt kê
  cơ sở dữ liệu, `--tables` liệt kê bảng, `--dump` lấy dữ liệu, nhưng
  trong lab bạn chỉ nên dump dữ liệu lab tự tạo, không dump dữ liệu
  thật của bất kỳ hệ thống nào.

## A5.6. Bài tập A5

1. Trong lab PortSwigger, làm xong một bài blind SQL injection ở mức
   cơ bản, ghi lại cách bạn phân biệt đúng và sai, và cách sửa bằng
   prepared statement.
2. Trong lab Juice Shop, tìm một stored XSS, ghi lại vị trí lưu payload
   và cách sửa bằng mã hóa output.
3. Viết một checklist kiểm tra JWT cho một API lab: thuật toán, chữ ký,
   hết hạn, issuer, audience, thông tin nhạy cảm trong payload.
4. Dùng ZAP hoặc Burp trong lab để chặn một request, sửa một tham số,
   ghi lại response thay đổi như thế nào.

---

# Chương A6: Leo thang đặc quyền và Active Directory ở mức khái niệm lab

Chương này chỉ dùng máy ảo lab cố tình cấu hình yếu, ví dụ LinPEAS
lab, Metasploitable, hoặc máy Windows lab riêng. Mục tiêu là hiểu vì
sao cấu hình yếu dẫn tới leo thang, và cách sửa, không phải để leo
thang trên máy thật của người khác.

## A6.1. Linux privilege escalation, các con đường thường gặp trong lab

- SUID sai: file có SUID mà user thường có thể chạy với quyền root,
  và file đó cho phép đọc file, chạy lệnh, hoặc ghi file. Phòng thủ:
  kiểm tra SUID định kỳ, gỡ SUID khỏi file không cần, cập nhật phần mềm.
- Sudo sai: user được phép chạy một lệnh với sudo mà không cần mật khẩu,
  và lệnh đó có thể chạy shell, đọc file, hoặc ghi file. Ví dụ sudo
  cho phép chạy `vim` thì có thể thoát ra shell bằng lệnh trong vim.
  Phòng thủ: chỉ cho sudo những lệnh thật sự cần, không cho sudo với
  trình soạn thảo, trình xem file, hoặc lệnh có thể chạy shell.
- Cron sai: script cron chạy bằng root nhưng file script đó user thường
  ghi được, hoặc thư mục chứa script đó user thường ghi được. Phòng
  thủ: script cron của root phải do root sở hữu, quyền ghi chỉ root.
- Kernel và phần mềm cũ: kernel hoặc dịch vụ cũ có lỗ hổng leo thang
  đã công bố. Phòng thủ: cập nhật, theo dõi CVE, không chạy dịch vụ
  cũ mở ra mạng.
- Capabilities: Linux capabilities chia nhỏ quyền root thành từng
  mảnh, ví dụ `CAP_NET_RAW`, `CAP_DAC_OVERRIDE`. File có capability
  nguy hiểm mà không cần có thể bị lợi dụng tương tự SUID. Kiểm tra
  bằng `getcap -r / 2>/dev/null` trong lab.
- Tool lab thường dùng: LinPEAS, LinEnum, chạy trong máy ảo lab để liệt
  kê cấu hình yếu, sau đó bạn đọc từng dòng output và tự sửa, không
  chạy tool rồi kết luận ngay mà không hiểu.

## A6.2. Windows và Active Directory ở mức khái niệm

- Active Directory là hệ thống quản lý user, máy, quyền trong mạng
  Windows doanh nghiệp. Domain Controller là máy chủ trung tâm, Kerberos
  là giao thức xác thực chính, LDAP là giao thức truy vấn thư mục.
- Các khái niệm cần biết, ở mức phòng thủ: Kerberoasting là xin vé
  dịch vụ Kerberos rồi dò mật khẩu ngoại tuyến trong lab, AS-REP
  Roasting là xin vé cho user không cần xác thực trước, Pass-the-Hash
  là dùng băm mật khẩu thay vì mật khẩu để đăng nhập, DCSync là giả
  làm Domain Controller để lấy băm mật khẩu.
- Phòng thủ AD: mật khẩu mạnh và dài cho tài khoản dịch vụ, tắt tài
  khoản không dùng, giới hạn quyền Domain Admin, bật MFA, ghi log
  Kerberos, theo dõi vé bất thường, vá Domain Controller, chia OU và
  Group Policy hợp lý.
- PowerShell là công cụ quản trị Windows, cũng là công cụ tấn công
  thường dùng trong lab. Phòng thủ PowerShell: bật ghi log module và
  script block, chặn script không ký, dùng Constrained Language Mode
  cho user thường khi cần.

## A6.3. Bài tập A6

1. Trong máy ảo Linux lab, chạy LinPEAS, tìm 3 cấu hình yếu, ghi lại
   từng cái và cách sửa.
2. Kiểm tra sudo của một user lab bằng `sudo -l`, ghi lại lệnh nào
   được phép, và phân tích lệnh đó có thể leo thang không, vì sao.
3. Viết một checklist hardening cho một máy chủ Linux lab: cập nhật,
   SUID, sudo, cron, SSH, tường lửa, log.

---
# Chương A7: Mật mã nâng cao, PKI và TLS

## A7.1. Mã hóa đối xứng và bất đối xứng

- Đối xứng: cùng một khóa để mã hóa và giải mã, ví dụ AES-256, ChaCha20.
  Nhanh, dùng cho dữ liệu lớn, nhưng hai bên phải chia sẻ khóa an toàn
  trước.
- Bất đối xứng: cặp khóa công khai và riêng, ví dụ RSA, ECC. Chậm hơn,
  dùng để trao đổi khóa đối xứng, ký số, xác thực, không dùng để mã hóa
  trực tiếp dữ liệu lớn.
- Kết hợp thực tế trong TLS: dùng RSA hoặc ECDHE để trao đổi khóa phiên,
  rồi dùng AES hoặc ChaCha20 để mã hóa dữ liệu trong phiên đó. ECDHE
  cho Perfect Forward Secrecy, nghĩa là lộ khóa dài hạn sau này cũng
  không giải mã được phiên cũ, vì khóa phiên đã bị xóa.

## A7.2. RSA và ECC ở mức hiểu, không cần chứng minh toán

- RSA dựa vào khó khăn của việc phân tích một số lớn thành hai số
  nguyên tố. Khóa RSA 2048 bit là chuẩn tối thiểu hiện nay cho hệ thống
  mới, RSA 1024 đã yếu, không dùng nữa.
- ECC dựa vào khó khăn của bài toán logarit rời rạc trên đường cong
  elliptic. Khóa ECC 256 bit có độ an toàn tương đương RSA 3072 bit,
  nhưng khóa ngắn hơn, nhanh hơn, phù hợp cho di động và IoT.
- Đừng tự nghĩ ra thuật toán mã hóa mới, đừng tự cài đặt RSA từ đầu
  cho hệ thống thật. Dùng thư viện đã được kiểm chứng như OpenSSL,
  libsodium, và dùng chế độ đã được khuyến nghị, ví dụ AES-GCM thay vì
  AES-ECB, vì ECB lộ mẫu dữ liệu lặp lại.

## A7.3. PKI và chứng chỉ

```bash
openssl x509 -in cert.pem -text -noout | grep -A 2 "Subject:"
openssl x509 -in cert.pem -noout -dates -issuer -subject
openssl verify -CAfile ca.pem cert.pem
openssl crl -in crl.pem -text -noout
```

- PKI: hệ thống nhà phát hành chứng chỉ, CA gốc, CA trung gian, chứng
  chỉ lá. Trình duyệt tin CA gốc, CA gốc ký cho CA trung gian, CA trung
  gian ký cho website, tạo thành chuỗi tin cậy.
- Chứng chỉ chứa: tên miền, khóa công khai, nhà phát hành, ngày bắt đầu,
  ngày hết hạn, chữ ký của nhà phát hành.
- Thu hồi: CRL là danh sách chứng chỉ đã thu hồi, OCSP là kiểm tra trực
  tuyến từng chứng chỉ. Nếu khóa riêng bị lộ, chứng chỉ phải bị thu hồi
  ngay, không chờ hết hạn.
- Phòng thủ PKI: tự động gia hạn, cảnh báo hết hạn trước 30 ngày, lưu
  khóa riêng an toàn, không push khóa riêng lên GitHub, kiểm tra chuỗi
  chứng chỉ đầy đủ, không dùng chứng chỉ tự ký cho hệ thống công khai.

## A7.4. Băm mật khẩu nâng cao và chính sách mật khẩu

- Thuật toán băm mật khẩu tốt phải chậm và có salt ngẫu nhiên cho mỗi
  mật khẩu, ví dụ bcrypt với cost cao, Argon2, PBKDF2 với nhiều vòng lặp.
  Chậm ở đây là cố tình, để kẻ tấn công dò mật khẩu cũng chậm theo.
- Salt ngăn tấn công bằng bảng băm sẵn, rainbow table, vì cùng một mật
  khẩu nhưng salt khác nhau sẽ cho băm khác nhau.
- Chính sách mật khẩu hiện đại: khuyến khích mật khẩu dài, ví dụ cụm
  4 từ ngẫu nhiên, thay vì bắt buộc ký tự đặc biệt phức tạp mà người
  dùng lại ghi ra giấy, kiểm tra mật khẩu mới có nằm trong danh sách
  mật khẩu đã rò rỉ không, bật MFA, không bắt đổi mật khẩu định kỳ
  khi chưa có dấu hiệu lộ, vì đổi liên tục làm người dùng chọn mật khẩu
  yếu hơn.

## A7.5. Bài tập A7

1. Dùng OpenSSL đọc một chứng chỉ lab, ghi lại issuer, subject, ngày
   hết hạn, thuật toán chữ ký.
2. So sánh RSA 2048 và ECC 256 về độ dài khóa và độ an toàn tương đương,
   ghi lại vì sao ECC ngắn hơn nhưng vẫn mạnh.
3. Viết chính sách mật khẩu cho một hệ thống lab: độ dài tối thiểu,
   MFA, khóa tài khoản sau bao nhiêu lần sai, và giải thích từng điểm.

---

# Chương A8: SOC, SIEM, Threat Hunting và MITRE ATT&CK

## A8.1. SOC là gì và làm việc như thế nào

SOC, Security Operations Center, là đội giám sát an ninh 24/7, nhận
cảnh báo từ SIEM, IDS, EDR, tường lửa, xác minh cảnh báo là thật hay
giả, điều tra, ứng cứu, và cải tiến quy tắc phát hiện.

Các cấp trong SOC, ở mức khái niệm:

- Cấp 1: nhận cảnh báo, phân loại, làm theo playbook, chuyển lên cấp
  cao hơn khi cần.
- Cấp 2: điều tra sâu hơn, xem log, gói tin, timeline, xác định phạm
  vi ảnh hưởng.
- Cấp 3: chuyên gia, phân tích mã độc, điều tra phức tạp, viết quy tắc
  mới, cải tiến hệ thống.

## A8.2. SIEM và log tập trung

```bash
cat /var/log/auth.log | grep "Failed"
journalctl -u ssh --since "2026-10-28 00:00:00"
```

- SIEM thu thập log từ nhiều nguồn: máy chủ, tường lửa, IDS, ứng dụng,
  cloud, rồi tương quan các log lại với nhau để phát hiện tấn công mà
  một log riêng lẻ không thấy được. Ví dụ: đăng nhập thất bại nhiều lần
  ở máy A, rồi đăng nhập thành công ở máy B, rồi tải file lớn ra ngoài,
  gộp lại mới thành một vụ việc.
- Các SIEM phổ biến: Splunk, ELK Stack gồm Elasticsearch, Logstash,
  Kibana, Microsoft Sentinel, Wazuh mã nguồn mở cho lab nhỏ.
- Trong lab, bạn có thể bắt đầu bằng cách gom log SSH, log web, log
  tường lửa vào một thư mục, rồi dùng grep, awk, Python để tương quan
  thủ công, trước khi học SIEM lớn. Kỹ năng đọc log thủ công là nền
  để hiểu SIEM làm gì.

## A8.3. MITRE ATT&CK

MITRE ATT&CK là bảng liệt kê chiến thuật và kỹ thuật tấn công đã quan
sát được trong thực tế, chia theo:

- Chiến thuật, Tactics: mục tiêu của kẻ tấn công ở từng giai đoạn,
  ví dụ Initial Access, Execution, Persistence, Privilege Escalation,
  Defense Evasion, Credential Access, Discovery, Lateral Movement,
  Collection, Exfiltration, Impact.
- Kỹ thuật, Techniques: cách cụ thể để đạt chiến thuật đó, ví dụ
  T1078 Valid Accounts, T1003 Credential Dumping, T1059 Command and
  Scripting Interpreter, T1027 Obfuscated Files.
- Khi bạn viết báo cáo SOC, hãy gắn mỗi phát hiện với một kỹ thuật
  ATT&CK, để đội khác hiểu kẻ tấn công đang ở giai đoạn nào, và cần
  chặn ở đâu tiếp theo.

## A8.4. YARA và Sigma, quy tắc phát hiện

YARA, dùng để nhận diện file mã độc theo mẫu:

```text
rule Lab_Test_Rule
{
  meta:
    description = "Vi du YARA trong lab, khong dung de ket luan that"
  strings:
    $a = "lab-malware-test-string"
    $b = { 4D 5A 90 00 }
  condition:
    $a and $b
}
```

- `strings`: mẫu cần tìm, có thể là chuỗi văn bản hoặc chuỗi hex.
- `condition`: điều kiện kết hợp các mẫu, ví dụ cả hai mẫu cùng xuất
  hiện.
- Chạy YARA trong lab: `yara rule.yar file-mau`, chỉ dùng file mẫu an
  toàn, không chạy YARA lên file hệ thống thật rồi kết luận ngay mà
  không kiểm tra tay.

Sigma, dùng để viết quy tắc phát hiện trong log, có thể chuyển sang
nhiều SIEM khác nhau:

```yaml
title: Dang nhap SSH that bai nhieu lan trong lab
logsource:
  product: linux
  service: sshd
detection:
  selection:
    event: "Failed password"
  condition: selection
level: medium
```

- Sigma giúp bạn viết quy tắc một lần, dùng cho nhiều SIEM, thay vì
  viết lại theo cú pháp riêng của từng SIEM.

## A8.5. Threat Hunting, săn tìm chủ động

Threat Hunting khác với chờ cảnh báo. Bạn chủ động hỏi: trong mạng của
tôi, có dấu hiệu nào của kỹ thuật ATT&CK nào không, dù chưa có cảnh
báo nào?

Quy trình hunting cơ bản:

1. Giả thuyết: ví dụ, có thể có tài khoản dịch vụ bị lạm dụng để chạy
   lệnh từ xa.
2. Thu thập dữ liệu: log đăng nhập, log tiến trình, log PowerShell,
   log mạng.
3. Phân tích: tìm tiến trình lạ, user lạ, thời gian lạ, IP lạ.
4. Kết luận: có bằng chứng hay không, mức độ chắc chắn ra sao.
5. Cải tiến: viết quy tắc phát hiện mới từ những gì đã tìm được, để
   lần sau hệ thống tự cảnh báo.

## A8.6. Bài tập A8

1. Chọn một kỹ thuật ATT&CK, ví dụ T1078 Valid Accounts, viết ra 3
   nguồn log có thể phát hiện kỹ thuật đó, và 3 dấu hiệu cần tìm.
2. Viết một rule YARA đơn giản trong lab, chạy lên một file mẫu tự tạo,
   kiểm tra rule có khớp không.
3. Viết một rule Sigma đơn giản phát hiện đăng nhập SSH thất bại, ghi
   lại logsource, detection, level.
4. Làm một buổi hunting giả định trong lab: giả thuyết có tiến trình lạ
   chạy lúc 3 giờ sáng, bạn sẽ xem log nào, theo thứ tự nào.

---

# Chương A9: Điều tra số và phân tích mã độc nâng cao

Cảnh báo lại, và quan trọng hơn ở mức nâng cao: chỉ phân tích file mẫu
an toàn, file do khóa học cung cấp, hoặc file bạn tự tạo trong máy ảo
cách ly, mạng ngắt, snapshot sẵn sàng khôi phục. Không phân tích mã độc
thật trên máy chính, không kết nối máy phân tích vào mạng thật, không
tải mã độc thật về máy thật để “xem thử”.

## A9.1. Quy trình điều tra số, DFIR nâng cao

1. Nhận diện: xác định vụ việc, phạm vi, hệ thống bị ảnh hưởng.
2. Bảo toàn: ngắt mạng khi cần, chụp ảnh đĩa, chụp RAM, tính băm, ghi
   lại thời gian và người thực hiện, để bằng chứng không bị thay đổi.
3. Thu thập: log, RAM, đĩa, mạng, theo thứ tự dễ bay hơi nhất trước:
   RAM, tiến trình, kết nối mạng, rồi mới tới đĩa.
4. Phân tích: timeline, tìm điểm bắt đầu, tìm cách kẻ tấn công vào,
   tìm dữ liệu bị lấy đi.
5. Ngăn chặn và khôi phục: chặn IP, khóa tài khoản, vá lỗ hổng, khôi
   phục từ bản sao lưu sạch.
6. Bài học: viết báo cáo, cập nhật quy tắc, tập huấn lại đội.

## A9.2. Chụp và phân tích RAM với Volatility

```bash
volatility -f ram-mem.lab imageinfo
volatility -f ram-mem.lab --profile=Win10x64 pslist
volatility -f ram-mem.lab --profile=Win10x64 netscan
volatility -f ram-mem.lab --profile=Win10x64 malfind
```

- Volatility là framework phân tích RAM, `pslist` liệt kê tiến trình,
  `netscan` liệt kê kết nối mạng, `malfind` tìm vùng nhớ có dấu hiệu
  mã độc chèn code.
- Phiên bản Volatility 3 dùng cú pháp khác, bạn cần đọc tài liệu của
  đúng phiên bản bạn cài, đừng copy lệnh của phiên bản cũ rồi chạy
  cho phiên bản mới.
- Phân tích RAM cần file RAM mẫu từ khóa học, không tự chụp RAM máy
  thật của người khác khi chưa có phép và quy trình pháp lý rõ ràng.

## A9.3. Chụp đĩa và timeline

```bash
dd if=/dev/sda of=anh-dia.lab bs=4M status=progress
sha256sum anh-dia.lab
fls -r -p anh-dia.lab
mactime -b bodyfile.txt
```

- `dd`: sao chép từng byte của đĩa sang file ảnh, dùng trong lab với
  đĩa lab riêng, không dùng `dd` lên đĩa hệ thống đang chạy khi chưa
  hiểu rõ, vì ghi nhầm `of` có thể xóa dữ liệu.
- `fls`: liệt kê file đã xóa và file còn lại trong ảnh đĩa.
- `mactime`: tạo timeline theo thời gian sửa, truy cập, tạo file, để
  bạn thấy kẻ tấn công đã làm gì, theo thứ tự nào.
- Luôn tính băm của ảnh đĩa ngay sau khi chụp, để chứng minh ảnh không
  bị thay đổi trong quá trình phân tích.

## A9.4. Phân tích mã độc tĩnh và động ở mức khái niệm

Tĩnh, không chạy file:

- Xem header, import, export, chuỗi, tài nguyên, chữ ký, dùng `file`,
  `strings`, `objdump`, `readelf`, Ghidra, radare2.
- Ghidra là tool phân tích ngược mã nguồn mở của NSA, có thể chuyển
  mã máy thành mã giả C để đọc, nhưng đọc Ghidra cần biết assembly cơ
  bản, không phải mở lên là hiểu ngay.
- `radare2` và `r2`: tool dòng lệnh phân tích nhị phân, lệnh mẫu trong
  lab: `r2 -A file-mau`, rồi `afl` liệt kê hàm, `pdf @ main` xem hàm main.

Động, chạy trong sandbox cách ly:

- Chạy file trong máy ảo snapshot, mạng giả lập, ghi lại tiến trình
  mới, file mới, kết nối mạng, thay đổi registry, dùng Process Monitor,
  Wireshark trong lab.
- Sandbox tự động như Cuckoo Sandbox có thể chạy file và cho báo cáo,
  nhưng cần cài đặt cẩn thận, và chỉ dùng file mẫu an toàn.
- Không bao giờ chạy mã độc thật ngoài sandbox, không kết nối sandbox
  vào mạng thật, không dùng tài khoản thật trong sandbox.

## A9.5. Bài tập A9

1. Trong máy ảo lab, chụp một file ảnh đĩa test nhỏ bằng `dd`, tính
   SHA256, liệt kê file bằng `fls` nếu có tool.
2. Dùng `strings` và `objdump` trên một file nhị phân lab, ghi lại 5
   hàm import bạn hiểu được.
3. Viết một kịch bản điều tra giả định đầy đủ 6 bước, từ nhận diện
   tới bài học, cho một máy lab bị đăng nhập lạ.

---
# Chương A10: Cloud, container và Kubernetes qua CLI

## A10.1. Cloud security ở mức khái niệm và CLI

Các cloud lớn đều có CLI riêng, chỉ dùng cho tài khoản cloud của chính
bạn, hoặc tài khoản lab do khóa học cấp, không dùng cho tài khoản của
người khác.

```bash
aws --version
aws s3 ls
aws iam list-users
aws ec2 describe-security-groups
az --version
az account show
gcloud --version
gcloud projects list
```

- `aws s3 ls`: liệt kê bucket S3, kiểm tra bucket nào mở công khai.
  Bucket S3 mở công khai là lỗi kinh điển, đã gây nhiều vụ lộ dữ liệu
  lớn, phòng thủ bằng cách chặn public access ở cấp tài khoản, kiểm tra
  bằng tool rà soát, và không lưu dữ liệu nhạy cảm trong bucket public.
- `aws iam list-users`: liệt kê user IAM, kiểm tra user nào có quyền
  admin nhưng không dùng MFA, khóa truy cập cũ không dùng.
- `aws ec2 describe-security-groups`: liệt kê security group, kiểm tra
  rule nào mở `0.0.0.0/0` tới cổng SSH, RDP, cơ sở dữ liệu.
- Phòng thủ cloud chung: quyền tối thiểu cho IAM, MFA cho tài khoản
  root và admin, ghi log CloudTrail, cảnh báo khi có thay đổi IAM,
  mã hóa dữ liệu lưu trữ, sao lưu và kiểm tra khôi phục.

## A10.2. Docker security qua CLI

```bash
docker --version
docker ps
docker images
docker run --rm -it ubuntu bash
docker inspect ten-container
docker scan ten-image
```

- `docker ps`: liệt kê container đang chạy, `docker images`: liệt kê
  image, `docker inspect`: xem cấu hình chi tiết, gồm quyền, mount,
  mạng.
- Lỗi Docker thường gặp trong lab: chạy container với `--privileged`,
  mount thư mục gốc `/` vào container, chạy container bằng user root
  khi không cần, dùng image cũ có lỗ hổng, để lộ Docker socket.
- Phòng thủ Docker: chạy container bằng user không phải root khi có
  thể, chỉ mount thư mục cần thiết, dùng image chính thức và cập nhật,
  quét image bằng `docker scan` hoặc Trivy, giới hạn tài nguyên CPU và
  RAM, tách mạng container theo ứng dụng.

Trivy, tool quét image trong lab:

```bash
trivy image ubuntu:latest
trivy image ten-image-cua-ban
```

## A10.3. Kubernetes security qua CLI

```bash
kubectl --version
kubectl get pods -A
kubectl get services -A
kubectl describe pod ten-pod -n ten-namespace
kubectl auth can-i --list
```

- `kubectl get pods -A`: liệt kê pod trên mọi namespace, `-A` là all.
- Lỗi Kubernetes thường gặp: API server mở ra Internet không xác thực,
  pod chạy bằng quyền root, service account có quyền quá rộng, secret
  lưu trong biến môi trường và lộ trong log, network policy không có
  nên pod nào cũng nói chuyện được với pod nào.
- Phòng thủ Kubernetes: bật RBAC và quyền tối thiểu cho service account,
  dùng NetworkPolicy để giới hạn lưu lượng giữa pod, dùng Pod Security
  Standards, quét image, lưu secret trong Secret Manager, ghi log audit
  của API server.

## A10.4. Bài tập A10

1. Trên tài khoản cloud lab của bạn, liệt kê bucket và security group,
   ghi lại rule nào mở quá rộng và cách sửa.
2. Chạy một container lab bằng user không phải root, kiểm tra bằng
   `docker inspect` xem user là ai.
3. Trên cụm Kubernetes lab, liệt kê pod và service, ghi lại pod nào
   chạy bằng root và cách sửa.

---

# Chương A11: Mobile và wireless nâng cao, chỉ trên thiết bị và mạng của bạn

## A11.1. Android qua ADB, chỉ trên điện thoại của bạn

```bash
adb devices
adb shell
adb shell pm list packages
adb logcat
adb pull /sdcard/file.txt ./
```

- `adb devices`: liệt kê điện thoại đã kết nối và bật gỡ lỗi USB.
- `adb shell`: vào shell của điện thoại, chạy lệnh Linux trên điện thoại.
- `adb logcat`: xem log hệ thống Android, rất nhiều, cần lọc bằng grep.
- Đừng bật gỡ lỗi USB và kết nối điện thoại vào máy lạ, vì ADB có thể
  cho máy lạ truy cập dữ liệu điện thoại của bạn. Tắt gỡ lỗi USB khi
  không dùng.
- Phân tích APK trong lab: chỉ phân tích APK do bạn tự build, hoặc APK
  mẫu từ khóa học, dùng `apktool`, `jadx`, `androguard` trong máy ảo
  lab, không phân tích APK lậu, không bẻ khóa ứng dụng của người khác.

## A11.2. Wireless, aircrack-ng và phòng thủ WiFi

Chỉ dùng trên mạng WiFi của chính bạn, với thiết bị của bạn, trong lab
riêng. Quét và thử mật khẩu WiFi của người khác khi chưa có phép là
vi phạm pháp luật và đạo đức, dù bạn chỉ “tập”.

Các công cụ, ở mức khái niệm và phòng thủ:

```bash
iwconfig
iwlist wlan0 scan
airmon-ng start wlan0
airodump-ng wlan0mon
```

- `iwlist scan`: liệt kê mạng WiFi xung quanh, tên, kênh, mã hóa.
- `airmon-ng`: chuyển card WiFi sang chế độ monitor để bắt gói tin
  không dây trong lab, cần card hỗ trợ monitor mode, điện thoại thường
  không hỗ trợ tốt, nên phần này học trên PC lab có card WiFi rời.
- `airodump-ng`: bắt gói tin WiFi, liệt kê AP và client trong lab.
- Phòng thủ WiFi: dùng WPA3 nếu thiết bị hỗ trợ, WPA2 với mật khẩu dài
  và ngẫu nhiên, tắt WPS, đổi mật khẩu mặc định của router, cập nhật
  firmware router, tách mạng khách và mạng chính, tắt tính năng quản
  trị từ xa của router khi không cần.

## A11.3. Bài tập A11

1. Trên điện thoại của bạn, dùng `adb devices` và `adb shell`, liệt kê
   5 gói ứng dụng hệ thống, ghi lại tên gói.
2. Trên mạng WiFi nhà bạn, liệt kê các mạng xung quanh bằng tool quét
   WiFi hợp pháp, ghi lại kênh nào đông nhất, kênh nào vắng nhất, và
   giải thích vì sao chọn kênh vắng giúp WiFi ổn định hơn.
3. Viết checklist hardening cho router nhà bạn: mật khẩu, WPA, WPS,
   firmware, mạng khách.

---

# Chương A12: Tự động hóa chuyên sâu và framework pentest trong lab

## A12.1. Metasploit Framework trong lab

Metasploit là framework pentest mã nguồn mở, có sẵn trong Kali Linux,
chỉ dùng trong lab và phạm vi pentest có hợp đồng.

```bash
msfconsole
search type:exploit platform:linux
use exploit/linux/ten-exploit-lab
show options
set RHOSTS may-ao-lab
check
run
```

- `search`: tìm module theo loại, nền tảng, tên.
- `use`: chọn module, `show options`: xem tham số cần điền, `set`: đặt
  tham số, `check`: kiểm tra mục tiêu lab có dễ bị tấn công bởi module
  đó không, `run`: chạy module.
- Trong lab, bạn nên bắt đầu bằng module `check` và module scanner,
  không phải module exploit mạnh nhất, và luôn chụp snapshot máy ảo
  lab trước khi chạy exploit, để khôi phục khi máy lab bị crash.
- Metasploit không phải là công cụ bấm một nút là chiếm được mọi hệ
  thống. Bạn vẫn cần hiểu lỗ hổng, phiên bản, cấu hình, và cách phòng
  thủ, nếu không bạn sẽ chỉ biết bấm lệnh mà không hiểu vì sao nó thành
  công hay thất bại.

## A12.2. Python cho an ninh nâng cao

Các thư viện hay dùng trong lab:

- `requests`: gửi HTTP, kiểm tra API lab.
- `scapy`: tạo và phân tích gói tin, chỉ dùng trong lab, cần root trên
  PC.
- `paramiko`: SSH bằng Python trong lab.
- `pwntools`: viết exploit trong lab CTF, chỉ dùng cho bài CTF và máy
  ảo lab.
- Khi bạn viết tool Python cho an ninh, hãy viết kèm xử lý lỗi, timeout,
  giới hạn tốc độ, và nhật ký, để tool không quét dồn dập làm quá tải
  hệ thống lab, và để bạn biết tool đã làm gì khi có sự cố.

Ví dụ Python gửi request tới lab, có timeout và kiểm tra mã trạng thái:

```python
import requests
try:
    r = requests.get("http://lab.local", timeout=5)
    print(r.status_code)
except requests.RequestException as e:
    print("Loi:", e)
```

## A12.3. Pipeline tự động hóa rà soát trong lab

Một pipeline lab đơn giản, chạy mỗi ngày trong lab riêng:

1. Nmap quét cổng và lưu XML.
2. Python đọc XML, so sánh với lần quét trước, báo cáo cổng mới mở.
3. Nuclei chạy mẫu an toàn trong lab, ghi lại kết quả.
4. Gửi báo cáo tóm tắt vào file log, không gửi dữ liệu nhạy cảm qua
   email công cộng.
5. Đội sửa chữa xem báo cáo, vá lỗi, chạy lại pipeline để kiểm tra đã
   sửa chưa.

Đây là mô hình DevSecOps ở mức cơ bản: bảo mật không phải là bước cuối
cùng trước khi phát hành, mà là bước chạy tự động trong suốt quá trình
phát triển và vận hành.

## A12.4. Bài tập A12

1. Trong máy ảo Kali lab, mở `msfconsole`, tìm một module scanner cho
   dịch vụ lab của bạn, chạy `check` và ghi lại kết quả.
2. Viết một script Python đọc file XML nmap, in ra cổng mới mở so với
   file XML hôm qua, nếu có.
3. Vẽ pipeline rà soát lab của bạn thành 5 bước, ghi rõ tool nào dùng
   ở bước nào, output nào lưu ở đâu.

---

# Chương A13: Lộ trình chuyên sâu, CTF, chứng chỉ và bài tập tổng hợp nâng cao

## A13.1. Lộ trình chuyên sâu sau phần 2

Giai đoạn chuyên sâu 1, 3 tới 6 tháng sau phần 2:

- Chọn một hướng chính: Blue Team thì học SIEM, Zeek, Suricata, YARA,
  Sigma, incident response. Red Team thì học pentest web nâng cao,
  Active Directory lab, Metasploit trong lab, báo cáo.
- Làm CTF mỗi tuần một bài, bắt đầu từ PicoCTF, TryHackMe, HackTheBox
  Easy, rồi lên Medium.
- Viết blog hoặc ghi chép lab bằng chính ngôn ngữ của bạn, không cần
  công khai, nhưng viết ra giúp bạn nhớ và giúp người khác kiểm tra
  được bạn đã hiểu thật hay chưa.

Giai đoạn chuyên sâu 2, 6 tới 12 tháng:

- Học một mảng sâu thật sự: ví dụ web nâng cao, hoặc cloud, hoặc
  forensics, hoặc mạng, đừng học lan man 10 mảng cùng lúc.
- Tham gia cộng đồng an ninh mạng uy tín, đọc báo cáo CVE, đọc writeup
  CTF, đọc tài liệu chính thức của tool, không chỉ đọc tin trên mạng
  xã hội.
- Làm một dự án lab hoàn chỉnh: ví dụ dựng một mạng lab có web, cơ sở
  dữ liệu, tường lửa, SIEM, rồi tự tấn công lab đó có phép, tự phát
  hiện, tự viết báo cáo sửa chữa.

## A13.2. Chứng chỉ nâng cao tham khảo

- OSCP: pentest thực hành, đã nói ở phần 1, phù hợp sau khi bạn đã làm
  lab nhiều tháng.
- OSEP, OSED, OSWE của OffSec: chuyên sâu hơn theo từng mảng, ví dụ
  evasion, exploit development, web nâng cao, chỉ học khi bạn đã vững
  OSCP hoặc tương đương.
- GIAC: nhiều chứng chỉ theo từng mảng như GCIH cho incident handling,
  GCIA cho phân tích xâm nhập, GSEC cho nền tảng, phù hợp Blue Team.
- CISSP: quản lý an ninh, chính sách, kiến trúc, phù hợp khi bạn đã có
  kinh nghiệm làm việc vài năm, không phải chứng chỉ cho người mới.
- Chứng chỉ là bằng chứng bạn đã học và thi, không phải là kỹ năng duy
  nhất. Khi phỏng vấn, người ta vẫn hỏi bạn đã làm lab nào, đã xử lý
  vụ việc nào, đã viết quy tắc nào.

## A13.3. Bài tập tổng hợp nâng cao cuối phần 2

Bài 1, mạng nâng cao:

- Dùng nmap nâng cao trong lab, gồm `-sV`, `-sC`, `--script` riêng, lưu
  XML, dùng Python đọc lại và so sánh với lần quét trước.

Bài 2, web nâng cao:

- Trong lab PortSwigger, làm xong một bài blind SQL injection và một
  bài SSRF ở mức cơ bản, ghi lại cơ chế và cách sửa cho từng bài.

Bài 3, SOC:

- Viết một rule Sigma và một rule YARA đơn giản trong lab, chạy thử,
  ghi lại kết quả khớp hay không khớp, và vì sao.

Bài 4, forensics:

- Trong máy ảo lab, tạo một timeline giả định từ log và file, ghi lại
  thứ tự sự kiện, điểm bắt đầu, và cách ngăn chặn.

Bài 5, cloud và container:

- Trên tài khoản cloud lab, kiểm tra bucket và security group, ghi lại
  rule mở quá rộng và cách sửa. Trên Docker lab, quét một image bằng
  Trivy, ghi lại lỗ hổng nghiêm trọng nhất và cách sửa bằng cách cập
  nhật image.

Khi làm xong 5 bài này, bạn đã có một bộ hồ sơ lab nâng cao, đủ để
kể lại khi người khác hỏi bạn đã học sâu tới đâu, và đủ để chọn hướng
chuyên sâu tiếp theo mà không bị lan man.

---

# Phụ lục nâng cao: Bảng tra cứu và lỗi thường gặp

## Bảng tra cứu nâng cao

- Nmap nâng cao: `-sX`, `-sF`, `-sN`, `-sA`, `-sM`, `-f`, `-D`, `--data-length`
- TLS: `openssl s_client`, `ssl-enum-ciphers`, `testssl.sh`
- IDS: Suricata, Snort, Zeek, `tshark -Y`
- SIEM và hunting: Sigma, YARA, MITRE ATT&CK, ELK, Splunk, Wazuh
- Forensics: Volatility, `dd`, `fls`, `mactime`, Ghidra, radare2
- Cloud: `aws s3 ls`, `aws iam list-users`, `az account show`, `gcloud projects list`
- Container: `docker ps`, `docker inspect`, `trivy image`, `kubectl get pods -A`

## Lỗi nâng cao thường gặp

- Chạy NSE script tự viết bị lỗi Lua: kiểm tra lại `portrule` và
  `action`, kiểm tra tên file có đuôi `.nse` không, kiểm tra script
  có nằm đúng thư mục script của nmap không, chạy với `-d` để xem chi
  tiết lỗi.
- Chạy Suricata không thấy cảnh báo: kiểm tra rule đã bật chưa, kiểm
  tra interface đã đúng chưa, kiểm tra file pcap có chứa lưu lượng
  khớp rule không, đừng kết luận hệ thống sạch chỉ vì không có cảnh báo.
- Chạy Metasploit bị lỗi RHOSTS: kiểm tra bạn đã `set RHOSTS` thành IP
  lab chưa, kiểm tra module có phù hợp với nền tảng và phiên bản lab
  không, kiểm tra máy lab có sống và cổng có mở không bằng nmap trước.
- Chạy Trivy không quét được image: kiểm tra tên image có đúng không,
  kiểm tra mạng có tải được cơ sở dữ liệu lỗ hổng không, kiểm tra phiên
  bản Trivy có cũ không.
- Phân tích RAM bằng Volatility bị lỗi profile: kiểm tra phiên bản
  Volatility, kiểm tra profile có khớp với hệ điều hành của file RAM
  mẫu không, đừng dùng lệnh của Volatility 2 cho Volatility 3.

---

## Lời kết phần 2

Phần 1 cho bạn nền tảng, phần 2 cho bạn chiều sâu. Bạn không cần học
hết phần 2 trong một tuần, cũng không cần trở thành chuyên gia mọi mảng
trong phần 2. Bạn chỉ cần chọn một hướng, làm lab thật, viết báo cáo
thật, và lặp lại. An ninh mạng là nghề học cả đời, vì công nghệ thay
đổi, lỗ hổng mới xuất hiện mỗi ngày, và kẻ tấn công cũng học không ngừng.

Điều quan trọng nhất vẫn giữ nguyên từ phần 1: chỉ thử hệ thống được
phép, ghi chép cẩn thận, và sửa chữa để hệ thống tốt hơn, không phải
để chứng tỏ bạn phá được hệ thống nào. Khi bạn làm được điều đó, bạn
đã đi đúng hướng của một người làm an ninh mạng tử tế và chuyên nghiệp.
