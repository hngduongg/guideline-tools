# Giáo trình CLI An Ninh Mạng / An Toàn Thông Tin

> Dành cho người học bằng dòng lệnh, bắt đầu từ điện thoại Android với Termux,
> rồi chuyển sang PC Linux khi có máy. Học từng bài một, chạy được rồi mới
> sang bài mới. Mọi lệnh quét, thử nghiệm trong giáo trình này chỉ dùng cho
> hệ thống của chính bạn, mạng nhà bạn, hoặc lab được phép thử nghiệm rõ ràng
> như scanme.nmap.org, TryHackMe, HackTheBox lab, OverTheWire, PortSwigger
> Web Security Academy, máy ảo tự dựng. Không quét, không thử mật khẩu,
> không khai thác hệ thống, website, WiFi của người khác khi chưa có phép
> bằng văn bản. Quét trái phép có thể vi phạm pháp luật và bị khóa tài khoản,
> khóa IP, xử lý theo quy định.

## Cách dùng giáo trình này

- Đọc theo thứ tự từ Chương 0 tới Chương 12 nếu bạn mới bắt đầu.
- Nếu đã biết một phần, bạn có thể nhảy tới chương cần, nhưng đừng bỏ qua
  Chương 0 và Chương 1, vì đó là nền để đọc được mọi output sau này.
- Mỗi chương có: mục tiêu, kiến thức cần, lệnh, ví dụ an toàn, cách đọc
  kết quả, lỗi thường gặp, bài tập tự kiểm tra.
- Quy ước trong giáo trình:
  - `muc-tieu` là máy hoặc website bạn được phép thử. Ví dụ chuẩn là
    `scanme.nmap.org`, máy ảo của bạn, hoặc IP trong mạng nhà bạn.
  - `192.168.1.0/24` là ví dụ cho mạng nhà. Bạn phải thay 3 số đầu bằng
    đúng 3 số đầu của mạng nhà bạn. Ví dụ nhà bạn là `192.168.0.x` thì dùng
    `192.168.0.0/24`.
  - Lệnh có `sudo` nghĩa là cần quyền quản trị. Trên Termux không root,
    lệnh đó sẽ báo lỗi quyền, đó là bình thường, không phải bạn gõ sai.
    Lên PC Linux thì thêm `sudo` vào đầu, trên Windows thì mở terminal
    bằng quyền Administrator.
  - Không gõ mật khẩu thật, không gõ IP thật của người khác, không đăng
    kết quả quét có chứa thông tin nhạy cảm lên mạng xã hội.

## Mục lục

- Chương 0: Đạo đức, pháp lý và lab an toàn
- Chương 1: Nền tảng CLI Linux và Termux
- Chương 2: Mạng máy tính qua CLI
- Chương 3: Khám phá mạng và quét cổng với Nmap
- Chương 4: Bắt gói tin và phân tích lưu lượng
- Chương 5: Web qua CLI
- Chương 6: SSH, mã hóa, băm và quản lý khóa
- Chương 7: Hệ thống, nhật ký và phòng thủ qua CLI
- Chương 8: Tường lửa, phân đoạn mạng và hardening cơ bản
- Chương 9: Phân tích file, mã độc cơ bản và điều tra dấu vết
- Chương 10: Tự động hóa bằng script cho an ninh mạng
- Chương 11: Lộ trình, chứng chỉ và bài tập tổng hợp
- Chương 12: Bảng tra cứu nhanh
- Phụ lục: Lỗi thường gặp và cách gỡ

---

# Chương 0: Đạo đức, pháp lý và lab an toàn

## 0.1. Ba loại vai trò trong an ninh mạng

- Phòng thủ, Blue Team: giám sát, đọc log, vá lỗi, ứng cứu sự cố,
  cấu hình an toàn. Đây là hướng phù hợp nhất cho người mới, vì bạn học
  cách hệ thống hoạt động bình thường trước, rồi mới nhận ra bất thường.
- Tấn công có phép, Red Team / Pentest: tìm lỗ hổng theo hợp đồng, có
  phạm vi, thời gian, mục tiêu ghi rõ, và báo cáo để sửa. Không có hợp
  đồng thì đó không còn là pentest.
- Tím, Purple Team: hai bên cùng làm việc để phòng thủ tốt hơn, dùng kết
  quả tấn công để viết quy tắc phát hiện.

Người mới không cần chọn ngay. Học nền chung trước, rồi chọn một hướng
để đi sâu, sau đó mới mở rộng sang hướng còn lại.

## 0.2. Quy tắc vàng trước khi gõ bất kỳ lệnh quét nào

1. Bạn có sở hữu hệ thống đó không, hoặc có phép bằng văn bản không?
   Nếu không, dừng lại.
2. Phạm vi là gì? Một IP, một dải mạng, một website, hay một tài khoản lab?
   Chỉ làm trong phạm vi đó.
3. Lệnh này có gây quá tải, khóa tài khoản, xóa dữ liệu không? Nếu có
   khả năng đó, đừng chạy, hoặc chạy trong lab riêng.
4. Kết quả có chứa mật khẩu, khóa riêng, thông tin cá nhân không? Nếu có,
   đừng chia sẻ, đừng push lên GitHub, đừng chụp màn hình đăng công khai.
5. Bạn có ghi lại lệnh đã chạy và kết quả để báo cáo và học lại không?
   Thói quen ghi log là thói quen của người làm an ninh chuyên nghiệp.

## 0.3. Lab an toàn bạn có thể dùng ngay

Trên điện thoại, không cần PC:

- Termux trên Android: môi trường Linux thu nhỏ, học CLI, nmap, curl,
  traceroute, Python, Go.
- scanme.nmap.org: máy chủ do nhóm Nmap dựng để mọi người tập quét hợp pháp.
  Chỉ quét cơ bản, đừng quét dồn dập, đừng chạy script tấn công vào đó.
- OverTheWire Bandit: học Linux qua SSH, mỗi màn là một lệnh.
  Lệnh mẫu, thay `bandit0` và mật khẩu theo màn bạn đang chơi, chỉ dùng
  trong lab OverTheWire:
  `ssh bandit0@bandit.labs.overthewire.org -p 2220`
- OverTheWire Natas: học bảo mật web qua trình duyệt, mỗi màn là một bài web.
- PortSwigger Web Security Academy: lab web miễn phí, có hướng dẫn từng
  lỗ hổng, chạy trên trình duyệt, không cần cài đặt nặng.
- TryHackMe, HackTheBox: lab có hướng dẫn, có máy ảo riêng cho từng người
  học, cần tài khoản và làm theo phạm vi của lab đó.

Khi có PC:

- Máy ảo VirtualBox hoặc VMware: cài Ubuntu, Debian, Kali Linux trong máy
  ảo, đặt mạng ở chế độ Host-Only hoặc NAT riêng, đừng để máy ảo lab
  nối thẳng vào mạng thật khi bạn đang tập tấn công.
- DVWA, Juice Shop, Metasploitable: ứng dụng và máy ảo cố tình có lỗ hổng
  để tập, chỉ chạy trong mạng lab riêng, không đưa lên Internet.
- Docker lab: chạy web lab trong container riêng, tắt container khi xong.

## 0.4. Phân biệt quét, thăm dò và tấn công

- Quét cổng: hỏi máy chủ đang mở cổng nào. Đây là bước trinh sát, vẫn
  cần phép nếu đó không phải máy của bạn.
- Thăm dò phiên bản: hỏi dịch vụ đang chạy phiên bản nào. Nhẹ hơn khai
  thác, nhưng vẫn là tương tác với hệ thống người khác.
- Khai thác: dùng lỗ hổng để chiếm quyền, đọc dữ liệu, thay đổi hệ thống.
  Chỉ làm trong lab hoặc theo hợp đồng pentest.
- Tấn công từ chối dịch vụ, đoán mật khẩu hàng loạt, quét dồn dập: gây
  quá tải, khóa tài khoản, dễ bị coi là tấn công dù bạn chỉ “tập”.

## 0.5. Bài tập Chương 0

1. Viết ra 3 mục tiêu bạn được phép thử ngay hôm nay, ví dụ: scanme,
   mạng nhà bạn, máy ảo của bạn.
2. Viết ra 3 mục tiêu bạn tuyệt đối không thử khi chưa có phép.
3. Tạo một thư mục ghi chép, ví dụ `~/lab-note/`, và ghi lại mỗi lệnh
   bạn chạy trong ngày, kèm ngày giờ và kết quả ngắn gọn.

---

# Chương 1: Nền tảng CLI Linux và Termux

## 1.1. Termux là gì, cài từ đâu, vì sao không dùng bản CH Play cũ

Termux là terminal và môi trường Linux trên Android, không cần root.
Bạn cài gói bằng `pkg`, chạy lệnh Linux, viết script, chạy Python, Go,
nmap, curl ngay trên điện thoại.

Lưu ý quan trọng đã được kiểm chứng nhiều lần:

- Bản Termux trên CH Play là bản cũ, bị kẹt ở chuẩn Android cũ, không
  tự cập nhật được, kho gói hay lỗi. Nhóm Termux khuyên cài qua F-Droid
  hoặc GitHub Releases.
- Cài F-Droid từ đúng `f-droid.org`, rồi trong F-Droid tìm Termux và cài.
  Chỉ cài mỗi Termux là đủ cho giai đoạn đầu. Các app phụ như Termux:API,
  Termux:Boot, Termux:Widget chỉ cần khi bạn muốn dùng camera, chạy script
  lúc khởi động, hoặc tạo shortcut ngoài màn hình.
- Không trộn bản CH Play với bản F-Droid, vì chữ ký khác nhau, Android
  sẽ không cho cài đè.
- Termux cố tình giữ chuẩn Android 9, API 28, vì từ Android 10 trở đi,
  quy tắc W^X cấm app tự chạy file trong thư mục riêng mà nó có thể ghi.
  Mà cả công việc của Termux là tải gói Linux về rồi chạy file trong
  thư mục đó, nên nâng chuẩn sẽ làm gãy Termux. Vì vậy cảnh báo “thiết
  kế cho phiên bản Android cũ” là bình thường với Termux, không phải
  dấu hiệu máy bạn quá cũ, và cũng không có nghĩa Termux chỉ chạy trên
  máy cũ. Nó vẫn chạy trên Android 11, 12 trở lên.

## 1.2. Cập nhật và cài gói

```bash
pkg update
pkg upgrade
```

- `pkg update`: cập nhật danh sách gói mới nhất từ kho, chưa cài gì cả.
- `pkg upgrade`: nâng cấp các gói đã cài lên bản mới.
- `pkg install ten-goi`: cài một gói, ví dụ `pkg install nmap`.
- `pkg install` đứng một mình, không có tên gói, sẽ không làm gì có ích.
  Lệnh cập nhật chuẩn là `pkg update && pkg upgrade`, còn `pkg install`
  thì phải đi kèm tên gói.

Ví dụ cài bộ công cụ cơ bản cho giáo trình này:

```bash
pkg update && pkg upgrade
pkg install nmap traceroute curl wget git python openssh termux-tools
```

- `nmap`: quét mạng.
- `traceroute`: xem đường đi của gói tin.
- `curl`, `wget`: tải và thử HTTP.
- `git`: lấy code, lấy lab, quản lý phiên bản.
- `python`: chạy script, xử lý log, viết tool nhỏ.
- `openssh`: có lệnh `ssh`, `scp`, `ssh-keygen`.

Kiểm tra một tool đã cài chưa:

```bash
nmap --version
curl --version
python --version
```

## 1.3. Thư mục, file và quyền

```bash
pwd
ls
ls -l
ls -la
cd ~
cd ..
cd /data/data/com.termux/files/home
mkdir lab-note
touch lab-note/ngay-dau.txt
cat lab-note/ngay-dau.txt
```

- `pwd`: in thư mục đang đứng, viết tắt của Print Working Directory,
  không phải password. Đổi mật khẩu là `passwd`, khác hẳn.
- `ls -l`: hiện quyền, chủ sở hữu, kích thước, ngày sửa.
- `ls -la`: hiện cả file ẩn bắt đầu bằng dấu chấm.
- Quyền đọc, ghi, chạy: `rwx` cho chủ sở hữu, nhóm, người khác.
  Ví dụ `-rwxr-xr-x` nghĩa là file, chủ được đọc ghi chạy, nhóm và
  người khác được đọc chạy.
- Đổi quyền: `chmod +x script.sh` cho phép chạy, `chmod 600 file.txt`
  chỉ chủ được đọc ghi, dùng cho file chứa thông tin nhạy cảm.
- Đổi chủ: `chown user:group file`, trên Termux ít dùng, trên PC Linux
  hay dùng khi sửa file hệ thống.
- Xóa file: `rm file`, xóa thư mục: `rm -r thu-muc`. Lệnh này không có
  thùng rác mặc định, xóa là mất, nên với file quan trọng hãy di chuyển
  sang thư mục dự phòng thay vì xóa thẳng.

Đọc file an toàn, không mở file lạ bằng quyền cao:

```bash
cat file.txt
less file.txt
head -n 20 file.txt
tail -n 20 file.txt
tail -f /var/log/auth.log
```

- `head`: xem đầu file.
- `tail`: xem cuối file, `tail -f` theo dõi file đang ghi thêm, rất
  hay dùng để xem log trực tiếp.

## 1.4. Tìm, lọc và xử lý văn bản

Đây là kỹ năng Blue Team dùng mỗi ngày để đọc log.

```bash
grep "Failed" file.log
grep -i "failed" file.log
grep -r "password" ~/lab-note/
grep -n "error" file.log
grep -v "OK" file.log
```

- `grep`: tìm dòng chứa mẫu. `-i` không phân biệt hoa thường,
  `-n` hiện số dòng, `-v` đảo ngược, chỉ hiện dòng không khớp,
  `-r` tìm trong thư mục con.

```bash
cat file.log | sort | uniq -c | sort -nr | head -20
cut -d " " -f 1 file.log
cut -d ":" -f 1 /etc/passwd
awk '{print $1}' file.log
awk '{print $1, $9}' file.log
sed 's/cu/moi/g' file.txt
tr 'a-z' 'A-Z' < file.txt
wc -l file.log
```

- `sort | uniq -c`: đếm số lần xuất hiện, dùng để tìm IP nào gọi nhiều
  nhất, lỗi nào lặp nhiều nhất.
- `cut -d`: cắt theo dấu phân cách, `-f` chọn cột.
- `awk`: xử lý theo cột, mạnh hơn `cut` khi khoảng trắng không đều.
- `sed`: thay thế văn bản theo mẫu, `s/cu/moi/g` thay tất cả.
- `wc -l`: đếm dòng, dùng để biết log có bao nhiêu dòng, kết quả quét
  có bao nhiêu host.

Ví dụ thực tế: tìm 10 IP gọi nhiều nhất trong log web:

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
```

Ví dụ tìm dòng lỗi SSH:

```bash
grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr
```

Trên Termux, đường dẫn log hệ thống khác PC Linux, bạn sẽ tập với
file log tự tạo và log của lab trước, rồi lên PC mới đọc `/var/log/`.

## 1.5. Tiến trình, tài nguyên và mạng ở mức hệ điều hành

```bash
ps aux
top
htop
free -h
df -h
du -sh ~/lab-note
uptime
who
w
id
```

- `ps aux`: liệt kê tiến trình đang chạy.
- `top`, `htop`: xem CPU, RAM theo thời gian thực, `htop` cần cài thêm.
- `free -h`: xem RAM, swap.
- `df -h`: xem dung lượng đĩa.
- `du -sh`: xem thư mục chiếm bao nhiêu.
- `id`: xem user, nhóm, quyền hiện tại.
- `who`, `w`: xem ai đang đăng nhập.

Kết thúc tiến trình cẩn thận, chỉ kết thúc tiến trình của bạn hoặc
tiến trình lab, không kết thúc tiến trình hệ thống khi chưa hiểu rõ:

```bash
kill PID
kill -9 PID
```

## 1.6. Biến, chuyển hướng và pipe

```bash
echo "hello"
ten="Capybara"
echo $ten
echo "ket qua" > ketqua.txt
echo "them dong" >> ketqua.txt
cat ketqua.txt
ls > danh-sach.txt 2> loi.txt
ls | grep txt
```

- Biến không có khoảng trắng quanh dấu `=`. Viết `ten="Capybara"` là
  đúng, viết `ten = "Capybara"` là sai.
- `>` ghi đè file, `>>` ghi thêm vào cuối file, không xóa nội dung cũ.
- `2>` chuyển hướng lỗi, `2>&1` gộp lỗi vào output thường.
- `|` pipe: đưa output của lệnh trước thành input của lệnh sau, đây là
  cách người làm CLI xâu chuỗi nhiều tool nhỏ thành một quy trình lớn.

## 1.7. Bài tập Chương 1

1. Trong Termux, chạy `pkg update && pkg upgrade`, rồi cài `nmap`,
   `curl`, `traceroute` nếu chưa có.
2. Tạo thư mục `~/lab-note`, tạo file `ngay-dau.txt`, ghi 3 dòng bằng
   `echo` và `>>`, rồi đọc lại bằng `cat`.
3. Dùng `grep`, `sort`, `uniq -c` để đếm xem trong một file log tự tạo,
   từ nào xuất hiện nhiều nhất.
4. Chạy `nmap --version` và `curl --version`, ghi phiên bản vào file
   ghi chép của bạn.

---
# Chương 2: Mạng máy tính qua CLI

## 2.1. Mô hình và khái niệm cần nhớ

- IP: địa chỉ của máy trên mạng. IPv4 có dạng `192.168.1.10`, IPv6 dài
  hơn, dạng `2001:db8::1`.
- Subnet và CIDR: `/24` nghĩa là 24 bit đầu là phần mạng, 8 bit cuối là
  phần máy, tức là 256 địa chỉ từ `.0` tới `.255`. Ví dụ `192.168.1.0/24`
  là cả mạng nhà có 3 số đầu là `192.168.1`.
- Gateway: cửa ra của một mạng con, thường là địa chỉ `.1` trong mạng đó.
  Mỗi mạng con có gateway riêng, nên `192.168.0.1` và `192.168.1.1` là
  hai gateway của hai mạng khác nhau, không phải một gateway đổi số.
- DNS: dịch vụ dịch tên miền thành IP, ví dụ `scanme.nmap.org` thành một IP.
- TCP: có bắt tay 3 bước, đáng tin cậy, dùng cho web, SSH, email.
  Các bước: SYN, SYN-ACK, ACK.
- UDP: không bắt tay, nhanh, không đảm bảo, dùng cho DNS, NTP, game,
  video trực tiếp.
- Cổng, port: cửa của dịch vụ trên một máy. Một IP có 65535 cổng TCP và
  65535 cổng UDP. Cổng dưới 1024 thường là dịch vụ hệ thống, cần quyền
  cao để mở.
- MAC: địa chỉ phần cứng của card mạng, dùng trong mạng LAN, khác IP.

## 2.2. Xem IP và đường mạng của chính bạn

Trên Linux và Termux:

```bash
ip a
ip r
ip -brief a
ss -tuln
ss -tlnp
```

- `ip a`: xem IP của từng card mạng, cả IPv4 và IPv6.
- `ip r`: xem bảng định tuyến, dòng có `default via` chính là gateway.
- `ss -tuln`: xem cổng TCP và UDP đang lắng nghe trên máy bạn, `-t` là
  TCP, `-u` là UDP, `-l` là listening, `-n` là hiện số, không dịch tên.
- Trên Windows, lệnh tương đương là `ipconfig`, `netstat -ano`, nhưng
  trên Linux và Termux bạn dùng `ip a` và `ss`, không dùng `ipconfig`.

Trên PC Linux, bạn còn có thể dùng `ifconfig` nếu đã cài gói
`net-tools`, nhưng `ip` và `ss` là công cụ mới, nên học `ip` và `ss`
trước.

## 2.3. Ping, traceroute, mtr

```bash
ping -c 4 8.8.8.8
ping -c 4 scanme.nmap.org
traceroute 8.8.8.8
traceroute scanme.nmap.org
mtr --report 8.8.8.8
```

- `ping -c 4`: gửi 4 gói ICMP echo, đo thời gian phản hồi, tỉ lệ mất gói.
  `-c` là count, số gói. Không có `-c` thì ping chạy mãi tới khi bạn
  bấm Ctrl+C.
- `traceroute`: liệt kê từng hop, tức là từng router mà gói tin đi qua,
  kèm thời gian mỗi hop. Hop đầu thường là WiFi hoặc router nhà bạn.
- Dòng `* * *` trong traceroute nghĩa là router đó không trả lời, không
  phải lúc nào cũng là lỗi. Nhiều router tắt ICMP để giảm tải.
- `mtr`: kết hợp ping và traceroute, báo cáo mất gói theo từng hop,
  cần cài thêm trên Termux và PC.

Bài tập: chạy `ping -c 4 8.8.8.8`, ghi lại thời gian trung bình, rồi
chạy `traceroute 8.8.8.8`, đếm xem có bao nhiêu hop, hop nào có thời
gian tăng đột ngột.

## 2.4. DNS qua CLI

```bash
dig scanme.nmap.org
dig +short scanme.nmap.org
dig scanme.nmap.org MX
dig scanme.nmap.org TXT
nslookup scanme.nmap.org
host scanme.nmap.org
```

- `dig`: công cụ DNS chi tiết nhất, hiện cả phần câu hỏi, câu trả lời,
  thời gian truy vấn, máy chủ DNS đã trả lời.
- `dig +short`: chỉ hiện IP, gọn để dùng trong script.
- Bản ghi A: IPv4, AAAA: IPv6, MX: máy chủ mail, TXT: văn bản xác minh,
  NS: máy chủ tên miền, CNAME: bí danh.
- `nslookup` và `host` gọn hơn, dùng khi bạn chỉ cần kiểm tra nhanh.
- Lỗi `NXDOMAIN` nghĩa là tên miền không tồn tại. Lỗi timeout nghĩa là
  máy chủ DNS không trả lời, kiểm tra lại mạng và DNS bạn đang dùng.

Ví dụ kiểm tra DNS của một tên miền trong lab:

```bash
dig +short example.com
dig example.com AAAA
```

Không dùng dig để tra cứu hàng loạt tên miền của người khác với mục
đích thu thập thông tin trái phép. Dùng cho tên miền của bạn và lab.

## 2.5. Whois và thông tin công khai

```bash
whois example.com
whois 45.33.32.156
```

- `whois`: tra cứu chủ sở hữu tên miền, ngày đăng ký, ngày hết hạn,
  máy chủ DNS, tổ chức sở hữu dải IP.
- Thông tin whois là công khai, nhưng đừng dùng nó để quấy rối chủ sở
  hữu, và đừng suy diễn chủ sở hữu từ một bản ghi whois đã che giấu
  thông tin cá nhân theo quy định bảo mật.

## 2.6. HTTP cơ bản qua curl

```bash
curl -I https://example.com
curl -v https://example.com
curl https://example.com
curl -L https://example.com
curl -o trang.html https://example.com
```

- `curl -I`: chỉ lấy header, không tải nội dung, dùng để xem máy chủ,
  mã trạng thái, kiểu nội dung.
- `curl -v`: hiện chi tiết quá trình, gồm DNS, bắt tay TLS, request và
  response header. Rất hay để học HTTP thật.
- `curl -L`: theo chuyển hướng, ví dụ từ http sang https.
- `curl -o`: lưu nội dung vào file.
- Mã HTTP thường gặp: 200 OK, 301 và 302 chuyển hướng, 403 cấm truy cập,
  404 không tìm thấy, 500 lỗi máy chủ.
- Header thường gặp: `Server` cho biết phần mềm máy chủ, `Content-Type`
  cho biết loại nội dung, `Set-Cookie` đặt cookie, `Location` là nơi
  chuyển hướng tới.

Ví dụ xem header của scanme hoặc example:

```bash
curl -I https://scanme.nmap.org
```

Chỉ dùng curl để thử các trang bạn được phép, đừng dùng curl để gửi
hàng loạt request làm quá tải website người khác.

## 2.7. Các cổng phổ biến cần thuộc

- 21 TCP FTP, truyền file cũ, không mã hóa.
- 22 TCP SSH, đăng nhập từ xa có mã hóa.
- 23 TCP Telnet, đăng nhập từ xa không mã hóa, không nên dùng.
- 25 TCP SMTP, gửi mail.
- 53 TCP và UDP DNS, dịch tên miền.
- 80 TCP HTTP, web không mã hóa.
- 110 TCP POP3, nhận mail cũ.
- 123 UDP NTP, đồng bộ giờ.
- 143 TCP IMAP, nhận mail.
- 161 UDP SNMP, giám sát thiết bị.
- 443 TCP HTTPS, web có mã hóa TLS.
- 445 TCP SMB, chia sẻ file Windows.
- 993 TCP IMAPS, 995 TCP POP3S, bản mã hóa của IMAP và POP3.
- 3306 TCP MySQL, 5432 TCP PostgreSQL, 6379 TCP Redis, 27017 TCP MongoDB:
  cơ sở dữ liệu, không bao giờ được mở trực tiếp ra Internet nếu không
  có lý do và bảo vệ rõ ràng.
- 8080 TCP, 8443 TCP: cổng web thay thế, hay dùng cho proxy và ứng dụng.

## 2.8. Bài tập Chương 2

1. Chạy `ip a` và `ip r`, ghi lại IP của máy bạn và gateway của bạn.
2. Chạy `ping -c 4 8.8.8.8` và `traceroute 8.8.8.8`, so sánh số hop và
   thời gian.
3. Chạy `dig +short scanme.nmap.org`, rồi `curl -I https://example.com`,
   ghi lại mã trạng thái và header `Server` nếu có.
4. Chạy `ss -tuln` trên máy bạn, liệt kê các cổng đang lắng nghe, và
   tự hỏi mỗi cổng đó thuộc về chương trình nào.

---

# Chương 3: Khám phá mạng và quét cổng với Nmap

Nmap là công cụ trung tâm của giáo trình này. Bạn đã học phần lõi trong
các buổi thực hành, chương này hệ thống lại đầy đủ, không bỏ phần nào,
để bạn tra cứu sau này.

## 3.1. Nmap là gì và khi nào dùng

- Nmap, Network Mapper, là công cụ mã nguồn mở để khám phá mạng, quét
  cổng, dò dịch vụ, dò hệ điều hành, chạy script kiểm tra.
- Dùng khi bạn cần biết: trong mạng có máy nào sống, máy đó mở cổng nào,
  cổng đó chạy dịch vụ gì, phiên bản nào, có script nào cho thêm thông
  tin không.
- Không dùng Nmap để quét hệ thống không có phép, quét dồn dập, hoặc
  chạy script tấn công vào hệ thống thật.

## 3.2. Quét cơ bản và cách đọc kết quả

```bash
nmap scanme.nmap.org
nmap --version
```

Kết quả mẫu, đã được kiểm chứng trong buổi học thực hành:

```text
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
9929/tcp open  nping-echo
Nmap done: 1 IP address (1 host up) scanned in 95.67 seconds
```

Cách đọc từng cột và từng dòng:

- `PORT`: số cổng và giao thức, ví dụ `22/tcp`.
- `STATE`: trạng thái. `open` là cổng mở, có dịch vụ lắng nghe và
  tường lửa cho qua. `closed` là máy trả lời nhưng không có dịch vụ ở
  cổng đó. `filtered` là tường lửa chặn, nmap không biết được mở hay
  đóng. `open|filtered` thường gặp ở UDP, nghĩa là không chắc được.
- `SERVICE`: tên dịch vụ thường gắn với cổng đó, ở quét cơ bản thì đây
  là đoán theo số cổng, chưa phải đo thật.
- Dòng cuối: `1 IP address (1 host up)` nghĩa là bạn quét 1 IP, 1 máy
  sống. Số giây là tổng thời gian quét. Quét 1000 cổng qua Internet
  mất gần 100 giây là bình thường, vì nhiều cổng không trả lời, nmap
  phải chờ hết thời gian chờ và thử lại.

## 3.3. Tìm máy sống

```bash
nmap -sn 192.168.1.0/24
nmap -sn 192.168.0.1-20
nmap -sn scanme.nmap.org
```

- `-sn`: chỉ tìm máy sống, không quét cổng. `s` là scan, `n` là no port
  scan, hiểu nôm na là quét nhưng không quét cổng.
- Kết quả mỗi máy sống sẽ hiện `Nmap scan report for IP` và
  `Host is up (thời gian latency)`.
- Latency gần bằng 0, ví dụ `0.00059s`, thường là chính máy bạn hoặc
  máy trong cùng mạng LAN rất gần.
- Dòng cuối `256 IP addresses (2 hosts up)` nghĩa là bạn quét cả dải
  `/24` gồm 256 địa chỉ, chỉ có 2 máy trả lời.

## 3.4. Chọn cổng

```bash
nmap -p 80 scanme.nmap.org
nmap -p 22,80,9929 scanme.nmap.org
nmap -p 1-1000 scanme.nmap.org
nmap -p- scanme.nmap.org
nmap --top-ports 20 scanme.nmap.org
```

- `-p` là port. Sau `-p` phải là một cụm duy nhất, các số nối bằng dấu
  phẩy, không có khoảng trắng.
- Sai: `nmap -p 22 80 9929 -sV scanme.nmap.org`. Khi đó nmap chỉ hiểu
  `-p 22`, còn `80` và `9929` đứng riêng sẽ bị hiểu thành tên máy chủ,
  và bạn sẽ gặp lỗi `Failed to resolve "80"`, `Failed to resolve "9929"`,
  `Error resolving 80: No address associated with hostname`.
- Đúng: `nmap -p 22,80,9929 -sV scanme.nmap.org`.
- `-p-`: quét hết 65535 cổng, rất lâu, chỉ dùng khi bạn thật sự cần và
  có thời gian, có phép.
- `--top-ports 20`: chỉ quét 20 cổng phổ biến nhất, nhanh để kiểm tra
  nhanh.

## 3.5. Dò phiên bản dịch vụ

```bash
nmap -sV scanme.nmap.org
nmap -p 22,80,9929 -sV scanme.nmap.org
```

- `-sV` là Service Version. Chữ V viết hoa. Chữ `v` viết thường là
  verbose, hiện chi tiết quá trình, khác hẳn.
- `-sV` chỉ thêm việc dò phiên bản, không đổi danh sách cổng, không đổi
  mục tiêu.
- Không có `-sV`, cột SERVICE chỉ đoán theo số cổng. Có `-sV`, nmap gửi
  thêm gói thăm dò, đọc banner và phản hồi, rồi ghi thêm cột VERSION,
  ví dụ OpenSSH phiên bản nào, Apache phiên bản nào.
- Vì phải thăm dò thêm nên `-sV` chạy lâu hơn quét thường một chút.

## 3.6. Các kiểu quét TCP và UDP, và quyền root

```bash
nmap -sT scanme.nmap.org
nmap -sS scanme.nmap.org
nmap -sU -p 53 scanme.nmap.org
nmap -sU -p 53,123,161 scanme.nmap.org
```

- `-sT`: TCP Connect, hoàn thành bắt tay 3 bước bằng socket bình thường
  của hệ điều hành, không cần root, đây là kiểu mặc định khi bạn không
  có quyền root, và là kiểu bạn đã chạy thành công trên Termux.
- `-sS`: TCP SYN, quét nửa mở, gửi SYN, nhận SYN-ACK thì gửi RST để
  không hoàn thành kết nối, nhanh và kín hơn, nhưng cần root vì phải
  tự tạo gói tin thô.
- `-sU`: UDP, gửi gói UDP, chờ ICMP port unreachable hoặc phản hồi.
  UDP không có bắt tay, nên kết quả hay là `open|filtered`, và quét
  chậm hơn TCP nhiều. Cổng UDP ví dụ: 53 DNS, 123 NTP, 161 SNMP.
  Bạn có thể thay 53 bằng cổng UDP khác, cách viết vẫn dùng dấu phẩy,
  ví dụ `-p 53,123,161`.
- Lỗi thường gặp, không phải bạn gõ sai:
  `You requested a scan type which requires root privileges. QUITTING!`
  Nghĩa là kiểu quét đó cần root, nmap thoát luôn, không quét tiếp.
  Trên điện thoại không root, đừng cố root máy chỉ để chạy lệnh này.
  Trên PC Linux, thêm `sudo` vào đầu, ví dụ `sudo nmap -sU -p 53 ...`,
  trên Windows thì mở terminal bằng quyền Administrator.

Bảng tóm tắt quyền:

- Không cần root: quét cơ bản, `-p`, `-sV`, `-sn`, `-oN`, `-sC`,
  `-sT`, `--script` mặc định và script an toàn.
- Cần root: `-sS`, `-sU`, `-O`, `-A` vì trong `-A` có `-O`, và các
  kiểu quét TCP nâng cao như `-sX`, `-sF`, `-sN`.

## 3.7. Dò hệ điều hành

```bash
nmap -O scanme.nmap.org
sudo nmap -O scanme.nmap.org
```

- `-O` là chữ O trong từ OS, viết hoa, không phải số 0. Gõ số 0 sẽ báo
  sai tùy chọn.
- `-O` gửi các gói thăm dò TCP và IP đặc biệt, phân tích cách máy chủ
  trả lời, ví dụ TTL, kích thước cửa sổ, thứ tự tùy chọn, để đoán hệ
  điều hành và phiên bản, kèm độ chính xác theo phần trăm.
- `-O` cần root, cần ít nhất một cổng mở và một cổng đóng để đoán chính
  xác hơn, và kết quả chỉ là đoán, không phải chắc chắn 100%.
- Trên điện thoại không root, `-O` sẽ báo lỗi quyền giống `-sU`. Lên PC
  có `sudo` thì chạy được bằng đúng lệnh đã học, chỉ thêm `sudo`.

## 3.8. Lưu kết quả

```bash
nmap -p 22,80,9929 -sV -oN ketqua.txt scanme.nmap.org
nmap -oX ketqua.xml scanme.nmap.org
nmap -oG ketqua.gnmap scanme.nmap.org
nmap -oA ketqua scanme.nmap.org
nmap -iL danhsach.txt
```

- `-oN`: output Normal, dạng chữ thường như trên màn hình, dễ đọc.
- `-oX`: output XML, dùng cho tool khác đọc và xử lý.
- `-oG`: output Grepable, dạng dễ lọc bằng grep.
- `-oA`: lưu cả 3 dạng cùng lúc, chỉ cần ghi tên gốc, không cần đuôi.
- `-iL`: đọc danh sách mục tiêu từ file, mỗi dòng một IP hoặc tên miền.
- Kiểm tra file đã lưu: `ls ketqua.txt`, đọc lại: `cat ketqua.txt`.
- Dùng tên file mới cho mỗi lần quét quan trọng, để lần sau không ghi
  đè lên kết quả cũ khi bạn chưa kịp xem.

## 3.9. Script Nmap, NSE

NSE là Nmap Scripting Engine, bộ script viết bằng Lua, chạy sau khi
quét cổng để lấy thêm thông tin, kiểm tra cấu hình, kiểm tra lỗ hổng
cơ bản.

```bash
nmap -sC -p 22,80,9929 scanme.nmap.org
nmap --script http-title -p 80 scanme.nmap.org
nmap --script http-headers -p 80 scanme.nmap.org
nmap --script ssh-hostkey -p 22 scanme.nmap.org
nmap --script ssl-cert -p 443 scanme.nmap.org
nmap --script "http-*" -p 80 scanme.nmap.org
```

- `-sC`: chạy bộ script mặc định, tương đương `--script=default`.
  Không cần root.
- `--script`: hai gạch ngang, vì đây là tùy chọn dạng tên dài. Sau đó
  là tên script đầy đủ, không cần đuôi `.nse`.
- `-sC` và `-sU` chỉ giống nhau ở hình thức đều bắt đầu bằng `-s`,
  còn việc làm khác hẳn. `-sU` đổi giao thức sang UDP và cần root,
  `-sC` vẫn quét TCP và chạy thêm script, không cần root. Bạn có thể
  ghép cả hai, nhưng phần UDP vẫn cần root.
- Kết quả script hiện dưới mỗi cổng, bắt đầu bằng `|`, dòng cuối của
  một script ghi là `|_`.

Ví dụ kết quả `-sC` đã chạy thành công trên scanme:

```text
22/tcp   open  ssh
| ssh-hostkey:
|   1024 ac:00:a0:1a:82:ff:cc:55:99:dc:67:2b:34:97:6b:75 (DSA)
|   2048 20:3d:2d:44:62:2a:b0:5a:9d:b5:b3:05:14:c2:a6:b2 (RSA)
|   256 96:02:bb:5e:57:54:1c:4e:45:2f:56:4c:4a:24:b2:57 (ECDSA)
|_  256 33:fa:91:0f:e0:e1:7b:1f:6d:05:a2:b0:f1:54:41:56 (ED25519)
80/tcp   open  http
|_http-favicon: Nmap Project
|_http-title: Go ahead and ScanMe!
9929/tcp open  nping-echo
```

Cách đọc:

- `ssh-hostkey`: dấu vân tay của khóa công khai SSH. Số đầu là độ dài
  khóa tính bằng bit, dãy hex ở giữa là fingerprint, trong ngoặc là
  thuật toán. Đây là khóa công khai, không phải mật khẩu, không phải
  khóa riêng tư. Nếu fingerprint đổi bất thường với máy bạn hay kết nối
  thì phải nghi ngờ.
- `http-favicon: Nmap Project`: favicon của trang thuộc về dự án Nmap.
- `http-title: Go ahead and ScanMe!`: tiêu đề trang web, đúng trang test
  mà nhóm Nmap dựng lên để mời mọi người quét.
- Cổng 9929 không có script thêm, vì dịch vụ nping-echo không có thông
  tin gì thêm để script lấy, đó là bình thường.

Lỗi script thường gặp:

```text
NSE: failed to initialize the script engine:
.../nse_main.lua:842: 'http' did not match a category, filename, or directory
QUITTING!
```

Nghĩa là tên script bạn gõ không khớp với tên script, nhóm script, hay
file nào cả. `http` một mình không phải tên script, tên đầy đủ phải là
`http-title`, `http-headers`, `http-favicon`. Mấy dòng stack traceback
phía dưới là chi tiết nội bộ, bạn chỉ cần đọc dòng `'http' did not match`.
Sửa bằng cách gõ tên đầy đủ, hoặc dùng dấu sao trong ngoặc kép để chạy
hết nhóm bắt đầu bằng http: `--script "http-*"`.

Nhóm script, category, thường gặp:

- `default`: bộ mặc định của `-sC`, an toàn, không tấn công.
- `safe`: chỉ script an toàn, không làm crash dịch vụ.
- `discovery`: khám phá thêm thông tin, ví dụ banner, DNS, SMB.
- `version`: dò phiên bản sâu hơn.
- `auth`: kiểm tra xác thực, có thể thử đăng nhập ẩn danh, chỉ dùng
  trong lab.
- `vuln`: kiểm tra lỗ hổng đã biết, chỉ dùng trong lab hoặc theo hợp
  đồng, vì có thể gây tải và ghi log cảnh báo.
- `brute`: đoán mật khẩu, `exploit`: khai thác, `dos`: từ chối dịch vụ.
  Ba nhóm này tuyệt đối không chạy lên hệ thống thật khi chưa có phép
  rõ ràng, và không chạy để “tập” lên website người khác.

## 3.10. Tốc độ, chi tiết và bỏ qua ping

```bash
nmap -v scanme.nmap.org
nmap -vv scanme.nmap.org
nmap -T4 scanme.nmap.org
nmap -Pn scanme.nmap.org
nmap --reason scanme.nmap.org
```

- `-v`: verbose, hiện chi tiết hơn. `-vv` chi tiết hơn nữa, dùng khi
  bạn muốn xem nmap đang làm gì trong lúc chờ.
- `-T`: timing, thang từ 0 tới 5. 0 là chậm nhất, kín nhất, 5 là nhanh
  nhất, ồn nhất. Mặc định là 3. Người mới nên dùng mặc định hoặc `-T4`
  khi quét lab, đừng vội dùng `-T5` lên hệ thống thật vì dễ bị chặn.
- `-Pn`: bỏ qua bước kiểm tra máy sống, coi như máy đang sống rồi quét
  thẳng. Dùng khi tường lửa chặn ping làm nmap tưởng máy chết dù máy
  vẫn sống.
- `--reason`: hiện lý do nmap kết luận một cổng là open, closed hay
  filtered, ví dụ nhận được RST, hay không nhận được gì.

## 3.11. Kết hợp hay dùng, không cần root

```bash
nmap -p 22,80,9929 -sV -sC scanme.nmap.org
nmap -p 22,80,9929 -sV -oN ketqua.txt scanme.nmap.org
nmap -sV -sC -oN ketqua.txt scanme.nmap.org
nmap -sn 192.168.1.0/24 -oN mang-nha.txt
```

## 3.12. Bài tập Chương 3

1. Quét `scanme.nmap.org` bằng lệnh cơ bản, ghi lại 3 cổng mở và thời
   gian quét.
2. Quét lại chỉ 3 cổng đó với `-sV`, so sánh cột SERVICE và VERSION.
3. Chạy `-sC` với 3 cổng đó, ghi lại ssh-hostkey và http-title.
4. Chạy `--script http-title` chỉ với cổng 80, kiểm tra xem output có
   đúng chỉ còn một dòng title không.
5. Chạy `-sn` trên mạng nhà bạn, ghi lại số máy sống, IP gateway và IP
   của chính điện thoại bạn, kiểm tra xem gateway có phải là `.1`
   trong đúng mạng đó không.
6. Cố tình gõ sai `--script http` một lần trong lab để nhìn lỗi NSE,
   rồi sửa thành `http-title` và chạy lại thành công.

---
# Chương 4: Bắt gói tin và phân tích lưu lượng

## 4.1. Vì sao cần bắt gói tin

Quét cổng cho bạn biết cửa nào mở, còn bắt gói tin cho bạn biết dữ liệu
thật sự đi qua dây như thế nào, máy nào nói chuyện với máy nào, giao
thức nào, có gì bất thường không. Đây là kỹ năng cốt lõi của Blue Team
và của người điều tra sự cố.

Chỉ bắt gói tin trên mạng của bạn, máy của bạn, hoặc lab được phép.
Không bắt gói tin trên mạng công cộng, mạng công ty khi chưa có phép,
vì bạn có thể thu thập dữ liệu của người khác.

## 4.2. tcpdump

```bash
sudo tcpdump -i any -c 20
sudo tcpdump -i eth0 -n host 8.8.8.8
sudo tcpdump -i eth0 -n port 80
sudo tcpdump -i eth0 -n port 53
sudo tcpdump -i eth0 -w bat-goi.pcap
sudo tcpdump -r bat-goi.pcap -n
```

- `-i`: chọn card mạng, `any` là tất cả, `eth0` là card dây thường gặp
  trên PC, trên Termux tên card có thể khác, bạn xem bằng `ip a`.
- `-n`: không dịch IP thành tên miền, đọc nhanh hơn và chính xác hơn.
- `-c`: số gói cần bắt rồi dừng, ví dụ `-c 20` là bắt 20 gói.
- `host`: lọc theo IP, `port`: lọc theo cổng, `src` và `dst` lọc theo
  nguồn và đích.
- `-w`: ghi ra file pcap để phân tích sau, không in ra màn hình.
- `-r`: đọc lại file pcap đã ghi.
- tcpdump cần root hoặc quyền bắt gói tin, trên điện thoại không root
  bạn sẽ học lý thuyết và đọc file pcap mẫu trước, lên PC có `sudo`
  mới bắt trực tiếp được.

Ví dụ đọc một gói TCP: bạn sẽ thấy IP nguồn, IP đích, cổng nguồn,
cổng đích, cờ SYN, ACK, RST, độ dài gói. Cờ SYN là bắt đầu kết nối,
SYN-ACK là máy chủ đồng ý, ACK là xác nhận, RST là cắt kết nối đột ngột.

## 4.3. tshark, phiên bản dòng lệnh của Wireshark

```bash
tshark -i eth0 -c 20
tshark -r bat-goi.pcap -Y "http"
tshark -r bat-goi.pcap -Y "dns"
tshark -r bat-goi.pcap -T fields -e ip.src -e ip.dst -e tcp.port
```

- `tshark` cần cài thêm, trên PC Linux cài bằng `apt install tshark`,
  trên Termux có thể không có sẵn hoặc cần quyền cao.
- `-Y`: lọc hiển thị theo giao thức, ví dụ chỉ hiện gói HTTP hoặc DNS.
- `-T fields -e`: chỉ in ra các trường bạn chọn, rất hay để đưa vào
  script thống kê, ví dụ đếm IP nguồn nhiều nhất.

## 4.4. Phân tích nhanh một file pcap bằng CLI

```bash
tcpdump -r bat-goi.pcap -n | head -20
tcpdump -r bat-goi.pcap -n host 192.168.1.10
tshark -r bat-goi.pcap -q -z io,phs
```

- Đếm giao thức: xem trong file có bao nhiêu gói TCP, UDP, ICMP, DNS.
- Tìm IP lạ: IP nào gửi nhiều gói nhất, IP nào gửi vào cổng lạ.
- Tìm DNS lạ: tên miền nào được hỏi nhiều nhất, có tên miền ngẫu nhiên
  dài bất thường không, đó có thể là dấu hiệu của kênh ẩn hoặc mã độc
  gọi về máy chủ điều khiển.
- Không mở file pcap lạ từ Internet bằng quyền cao, và không tải file
  pcap chứa dữ liệu nhạy cảm lên dịch vụ phân tích công cộng.

## 4.5. Bài tập Chương 4

1. Trên PC lab, bắt 20 gói khi bạn mở một trang web trong lab, ghi lại
   IP đích và cổng đích.
2. Đọc lại file pcap bằng `tcpdump -r`, lọc chỉ gói tới cổng 80 và 443.
3. Viết một dòng lệnh đếm xem IP nguồn nào xuất hiện nhiều nhất trong
   file pcap mẫu của bạn, dùng `awk`, `sort`, `uniq -c`.

---

# Chương 5: Web qua CLI

## 5.1. HTTP sâu hơn với curl

```bash
curl -I https://example.com
curl -v https://example.com
curl -A "Mozilla/5.0" https://example.com
curl -H "Accept: application/json" https://example.com/api
curl -X POST -d "user=test&pass=test" https://example.com/login
curl -b "session=abc123" https://example.com/profile
curl -c cookie.txt https://example.com/login
curl --data-urlencode "q=hello world" https://example.com/search
curl -o /dev/null -s -w "%{http_code} %{time_total}\n" https://example.com
```

- `-A`: User-Agent, tên trình duyệt hoặc tool bạn giả lập, chỉ dùng để
  kiểm tra máy chủ phản ứng khác nhau theo User-Agent trong lab.
- `-H`: thêm header tùy chỉnh, ví dụ `Accept`, `Authorization`.
  Không gửi token thật, mật khẩu thật của tài khoản thật qua curl trong
  lab công cộng, và không lưu token vào lịch sử lệnh chung.
- `-X`: chỉ định phương thức, `GET`, `POST`, `PUT`, `DELETE`.
- `-d`: dữ liệu gửi kèm POST, dạng form.
- `-b`: gửi cookie, `-c`: lưu cookie nhận được vào file.
- `-w`: in ra mã trạng thái và thời gian, dùng để đo hiệu năng và kiểm
  tra nhanh nhiều URL.

Bài tập: dùng `curl -I` và `curl -v` trên một trang lab, ghi lại mã
trạng thái, header `Server`, `Content-Type`, và xem có header bảo mật
nào như `Strict-Transport-Security`, `Content-Security-Policy`,
`X-Frame-Options` không. Thiếu header bảo mật là một điểm cần ghi nhận
trong báo cáo phòng thủ, không phải là lỗ hổng để khai thác ngay.

## 5.2. Khám phá thư mục và file web trong lab

Các tool này chỉ dùng trong lab và với phép rõ ràng, vì chúng gửi rất
nhiều request để đoán tên thư mục và file.

```bash
gobuster dir -u http://lab.local -w /usr/share/wordlists/dirb/common.txt
ffuf -u http://lab.local/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

- `gobuster` và `ffuf`: đoán thư mục, file, tham số, subdomain bằng
  danh sách từ, wordlist.
- `-u`: URL mục tiêu, `FUZZ` là vị trí sẽ thay bằng từng từ trong
  wordlist.
- `-w`: wordlist, danh sách từ để thử.
- Khi chạy tool này lên website thật mà không có phép, bạn có thể bị
  coi là tấn công, bị chặn IP, và làm quá tải máy chủ. Trong lab, hãy
  giới hạn wordlist nhỏ trước, ví dụ 100 từ, rồi mới tăng dần.

## 5.3. OWASP Top 10 nhìn từ CLI

Đây là 10 nhóm lỗ hổng web thường gặp nhất, bạn cần biết tên, hiểu cơ
chế, và biết cách phòng thủ, không cần thuộc lòng payload tấn công.

1. Broken Access Control: người dùng thường truy cập được chức năng
   hoặc dữ liệu của người khác, ví dụ đổi ID trên URL là xem được hồ
   sơ người khác. Phòng thủ: kiểm tra quyền ở máy chủ cho mọi request,
   không chỉ ẩn nút trên giao diện.
2. Cryptographic Failures: dữ liệu nhạy cảm không mã hóa, dùng HTTP
   thay vì HTTPS, mật khẩu lưu dạng rõ hoặc băm yếu. Phòng thủ: HTTPS
   mọi nơi, băm mật khẩu bằng thuật toán chậm và có salt như bcrypt,
   Argon2.
3. Injection: SQL injection, XSS, command injection. Dữ liệu người dùng
   được đưa thẳng vào câu lệnh mà không kiểm tra. Phòng thủ: dùng
   prepared statement, mã hóa output, không nối chuỗi vào câu lệnh.
4. Insecure Design: thiết kế thiếu kiểm soát từ đầu, ví dụ không giới
   hạn số lần đăng nhập, không kiểm tra quyền. Phòng thủ: mô hình đe
   dọa, kiểm tra quyền, giới hạn tốc độ.
5. Security Misconfiguration: cấu hình mặc định, trang quản trị mở,
   header thiếu, lỗi hiện chi tiết. Phòng thủ: tắt tính năng không cần,
   ẩn lỗi chi tiết, rà soát cấu hình.
6. Vulnerable and Outdated Components: dùng thư viện, framework cũ có
   lỗ hổng đã công bố. Phòng thủ: cập nhật, theo dõi CVE, khóa phiên bản.
7. Identification and Authentication Failures: mật khẩu yếu, không khóa
   tài khoản sau nhiều lần sai, session không hết hạn. Phòng thủ: mật
   khẩu mạnh, xác thực đa yếu tố, session timeout.
8. Software and Data Integrity Failures: cập nhật phần mềm không kiểm
   tra chữ ký, dữ liệu không kiểm tra toàn vẹn. Phòng thủ: kiểm tra
   chữ ký, hash, nguồn đáng tin.
9. Security Logging and Monitoring Failures: không ghi log, không cảnh
   báo khi đăng nhập lạ, xóa log. Phòng thủ: ghi log đầy đủ, cảnh báo,
   bảo vệ log.
10. Server-Side Request Forgery, SSRF: máy chủ bị lừa gửi request tới
    hệ thống nội bộ. Phòng thủ: kiểm tra URL, chặn IP nội bộ, danh sách
    cho phép.

## 5.4. Các tool web khác, chỉ dùng trong lab

- `nikto`: quét cấu hình web cơ bản, kiểm tra header, file nguy hiểm.
  Lệnh mẫu trong lab: `nikto -h http://lab.local`.
- `whatweb`: nhận diện công nghệ web đang chạy, ví dụ WordPress, Apache.
  Lệnh mẫu: `whatweb http://lab.local`.
- `sqlmap`: kiểm tra SQL injection tự động, rất mạnh và rất nguy hiểm
  nếu dùng sai mục tiêu. Chỉ dùng trong lab DVWA hoặc Juice Shop tự
  dựng, với cơ sở dữ liệu lab riêng, không bao giờ chạy lên website thật.
  Lệnh mẫu trong lab, sau khi bạn đã hiểu rõ tham số và có phép:
  `sqlmap -u "http://lab.local/page?id=1" --batch`.
- `nuclei`: chạy mẫu kiểm tra lỗ hổng đã biết, chỉ dùng trong lab và
  theo phạm vi.
- Khi bạn chưa hiểu một tool làm gì, đừng chạy nó với tùy chọn mạnh nhất.
  Đọc `--help` trước, chạy với phạm vi nhỏ nhất, trong lab trước.

## 5.5. Bài tập Chương 5

1. Dùng `curl -I` trên 3 trang lab, so sánh header bảo mật của chúng.
2. Dùng `curl -v` và ghi lại các bước DNS, TLS, request, response của
   một request HTTPS.
3. Trong lab PortSwigger, làm xong một bài SQL injection cơ bản và một
   bài XSS cơ bản, ghi lại cơ chế và cách sửa, không ghi lại payload
   dùng để tấn công hệ thống thật.
4. Liệt kê 3 cấu hình sai bạn tìm thấy trong lab web của mình và cách
   sửa từng cái.

---

# Chương 6: SSH, mã hóa, băm và quản lý khóa

## 6.1. SSH cơ bản và an toàn

```bash
ssh user@may-chu
ssh -p 2222 user@may-chu
ssh -i ~/.ssh/id_ed25519 user@may-chu
scp file.txt user@may-chu:/tmp/
scp user@may-chu:/tmp/file.txt ./
ssh-keygen -t ed25519 -C "ghi-chu-cua-ban"
cat ~/.ssh/id_ed25519.pub
ssh-copy-id user@may-chu
```

- `ssh`: đăng nhập từ xa có mã hóa, cổng mặc định 22.
- `-p`: đổi cổng, ví dụ lab SSH chạy ở 2222 như OverTheWire Bandit.
- `-i`: chỉ định file khóa riêng để đăng nhập, không dùng mật khẩu.
- `scp`: sao chép file qua SSH, an toàn hơn FTP.
- `ssh-keygen -t ed25519`: tạo cặp khóa mới, thuật toán Ed25519 hiện đại,
  khóa ngắn nhưng mạnh. Không đặt mật khẩu rỗng cho khóa dùng thật nếu
  máy có thể bị người khác truy cập, hãy đặt passphrase.
- `id_ed25519` là khóa riêng, tuyệt đối không chia sẻ, không push lên
  GitHub, không gửi qua chat. `id_ed25519.pub` là khóa công khai, có
  thể đưa cho máy chủ để cho phép đăng nhập.
- `ssh-copy-id`: chép khóa công khai lên máy chủ, sau đó bạn đăng nhập
  bằng khóa thay vì mật khẩu.
- Tắt đăng nhập bằng mật khẩu trên máy chủ của bạn khi đã có khóa, tắt
  đăng nhập root từ xa, đổi cổng SSH mặc định nếu cần, nhưng đổi cổng
  không thay thế cho khóa mạnh và cập nhật phần mềm.

## 6.2. OpenSSL cơ bản

```bash
openssl version
openssl s_client -connect example.com:443
openssl x509 -in cert.pem -text -noout
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc
openssl enc -d -aes-256-cbc -in file.enc -out file-dec.txt
```

- `s_client`: kiểm tra chứng chỉ TLS của một website, xem ngày hết hạn,
  nhà phát hành, thuật toán.
- `x509 -text`: đọc chi tiết chứng chỉ, kiểm tra tên miền có khớp không,
  chứng chỉ đã hết hạn chưa.
- Lệnh tạo chứng chỉ tự ký chỉ dùng trong lab, trình duyệt sẽ cảnh báo
  vì chứng chỉ tự ký không được nhà phát hành tin cậy xác nhận.
- Mã hóa file bằng `openssl enc` cần mật khẩu mạnh, và bạn phải nhớ
  mật khẩu đó, mất mật khẩu là mất dữ liệu.

## 6.3. Băm và kiểm tra toàn vẹn

```bash
md5sum file.txt
sha1sum file.txt
sha256sum file.txt
sha256sum file.txt > file.txt.sha256
sha256sum -c file.txt.sha256
```

- Băm là hàm một chiều, từ dữ liệu tạo ra một chuỗi cố định, thay đổi
  một ký tự là băm đổi hoàn toàn.
- MD5 và SHA1 đã yếu, không dùng để bảo vệ mật khẩu hay chữ ký quan
  trọng, chỉ dùng để kiểm tra file có bị thay đổi vô tình không.
- SHA256 là chuẩn phổ biến hiện nay để kiểm tra toàn vẹn file.
- `sha256sum -c`: kiểm tra lại file có khớp với băm đã lưu không.
- Không lưu mật khẩu dưới dạng MD5, SHA1, SHA256 thuần. Mật khẩu phải
  băm bằng thuật toán chậm, có salt, như bcrypt, Argon2, PBKDF2, để
  kẻ tấn công không thể dò hàng triệu mật khẩu mỗi giây.

## 6.4. GPG cơ bản

```bash
gpg --full-generate-key
gpg --list-keys
gpg --export -a "Ten Ban" > public.key
gpg --encrypt -r "Ten Nguoi Nhan" file.txt
gpg --decrypt file.txt.gpg
gpg --sign file.txt
gpg --verify file.txt.sig
```

- GPG dùng cho mã hóa và ký số, xác nhận người gửi và toàn vẹn dữ liệu.
- Khóa riêng GPG cũng tuyệt đối không chia sẻ, giống khóa SSH.
- Khi bạn xác minh chữ ký của phần mềm tải về, hãy tải chữ ký từ nguồn
  chính thức và kiểm tra khóa công khai từ nhiều nguồn độc lập nếu có thể.

## 6.5. Mật khẩu, John và Hashcat, chỉ dùng trong lab

- `john` và `hashcat` là tool dò mật khẩu từ băm, dùng để kiểm tra độ
  mạnh mật khẩu của chính bạn, hoặc trong bài tập lab với băm lab riêng.
- Tuyệt đối không dùng chúng để dò mật khẩu tài khoản thật của người
  khác, không dùng wordlist chứa mật khẩu thật rò rỉ từ vụ lộ dữ liệu
  để tấn công tài khoản thật.
- Bài tập an toàn: tự tạo một băm SHA256 của mật khẩu yếu do bạn tự đặt
  trong lab, ví dụ mật khẩu `123456`, rồi dùng John hoặc Hashcat trong
  máy ảo lab để xem nó bị dò nhanh như thế nào, từ đó hiểu vì sao mật
  khẩu yếu là nguy hiểm, và vì sao mật khẩu dài, ngẫu nhiên, dùng trình
  quản lý mật khẩu lại quan trọng.
- Lệnh mẫu chỉ mang tính minh họa trong lab, bạn cần đọc tài liệu của
  từng tool và chạy trong máy ảo riêng:
  `john --wordlist=wordlist-lab.txt hash-lab.txt`
  `hashcat -m 0 hash-lab.txt wordlist-lab.txt`

## 6.6. Bài tập Chương 6

1. Tạo một cặp khóa Ed25519 trong Termux hoặc PC lab, xem khóa công
   khai, và ghi lại sự khác nhau giữa khóa riêng và khóa công khai.
2. Dùng `openssl s_client` kiểm tra chứng chỉ của một trang HTTPS trong
   lab, ghi lại ngày hết hạn và nhà phát hành.
3. Tạo một file, tính SHA256, sửa một ký tự, tính lại SHA256 và so sánh.
4. Mã hóa một file test bằng OpenSSL với mật khẩu lab, giải mã lại và
   kiểm tra nội dung có khớp không.

---
# Chương 7: Hệ thống, nhật ký và phòng thủ qua CLI

## 7.1. Người dùng, nhóm và quyền

```bash
whoami
id
cat /etc/passwd
cat /etc/group
sudo useradd ten-moi
sudo passwd ten-moi
sudo usermod -aG sudo ten-moi
sudo userdel ten-moi
sudo groupadd nhom-moi
```

- `/etc/passwd`: danh sách user, shell, thư mục nhà, không chứa mật khẩu
  thật, mật khẩu đã băm nằm trong `/etc/shadow` chỉ root đọc được.
- Đừng chỉnh sửa trực tiếp `/etc/passwd` và `/etc/shadow` bằng tay khi
  chưa hiểu rõ, hãy dùng `useradd`, `usermod`, `passwd`.
- Nguyên tắc quyền tối thiểu: mỗi user, mỗi dịch vụ chỉ có quyền đủ để
  làm việc của nó, không cho quyền root bừa bãi.
- Kiểm tra user nào có shell đăng nhập thật, user nào là user hệ thống
  với shell `/usr/sbin/nologin` hoặc `/bin/false`.

## 7.2. Quyền file nâng cao và điểm cần kiểm tra

```bash
ls -l /usr/bin/sudo
find / -perm -4000 -type f 2>/dev/null | head -20
ls -ld /tmp
chmod 1777 /tmp
```

- SUID: bit `s` ở vị trí thực thi của chủ sở hữu, ví dụ `sudo` cần SUID
  để user thường có thể chạy với quyền cao hơn trong phạm vi cho phép.
  File nào có SUID mà bạn không nhận ra thì phải nghi ngờ và kiểm tra
  nguồn gốc của nó, đừng xóa bừa khi chưa hiểu, vì có thể làm gãy hệ
  thống.
- Thư mục `/tmp` thường có sticky bit `t`, nghĩa là user chỉ xóa được
  file của chính mình trong thư mục chung, không xóa file của người khác.
- Kiểm tra file mới thay đổi gần đây trong hệ thống lab:
  `find /etc -type f -mtime -1 2>/dev/null`

## 7.3. Nhật ký hệ thống

Trên PC Linux hiện đại:

```bash
journalctl -n 50
journalctl -u ssh
journalctl --since "2026-10-28 00:00:00" --until "2026-10-28 23:59:59"
journalctl -p err
sudo tail -n 100 /var/log/auth.log
sudo tail -n 100 /var/log/syslog
sudo grep "Failed password" /var/log/auth.log
sudo grep "Accepted password" /var/log/auth.log
```

- `journalctl`: xem log của systemd, `-u` lọc theo dịch vụ, `-p err`
  chỉ hiện lỗi trở lên, `--since` và `--until` lọc theo thời gian.
- `/var/log/auth.log`: log xác thực, đăng nhập SSH thành công và thất
  bại, thay đổi sudo. Trên một số hệ thống, file này là
  `/var/log/secure`.
- `/var/log/syslog` hoặc `/var/log/messages`: log hệ thống chung.
- Log web: `/var/log/apache2/access.log`, `/var/log/nginx/access.log`,
  tùy máy chủ bạn dùng.
- Khi điều tra, đừng chỉ xem một dòng lỗi, hãy xem trước và sau dòng đó
  vài dòng, xem IP nào, user nào, thời gian nào, có lặp lại nhiều lần
  không, có thành công sau nhiều lần thất bại không. Chuỗi thất bại rồi
  thành công là dấu hiệu cần cảnh báo.

Ví dụ thống kê IP đăng nhập SSH thất bại nhiều nhất:

```bash
sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr | head -20
```

Ví dụ này chỉ là mẫu, số cột có thể khác nhau tùy định dạng log, bạn
cần xem một dòng log thật trước rồi mới chọn đúng cột trong `awk`.

## 7.4. Giám sát và cảnh báo cơ bản

- `fail2ban`: tự động chặn IP đăng nhập sai nhiều lần trong một khoảng
  thời gian. Cấu hình trong lab, kiểm tra bằng `fail2ban-client status`,
  đừng bật fail2ban lên hệ thống thật khi chưa hiểu rõ quy tắc chặn,
  vì bạn có thể tự chặn chính mình.
- `auditd`: ghi lại các hành động quan trọng trên hệ thống, ví dụ mở
  file nhạy cảm, thay đổi quyền. Xem log bằng `ausearch`.
- `rkhunter`, `chkrootkit`: quét dấu hiệu rootkit cơ bản trong lab,
  kết quả có thể có báo động giả, cần kiểm tra tay trước khi kết luận.
- Sao lưu log ra nơi khác, vì kẻ tấn công sau khi chiếm quyền thường
  xóa hoặc sửa log trên máy nạn nhân để che dấu vết. Log tập trung ở
  máy chủ log riêng là thực hành phòng thủ tốt.

## 7.5. Cập nhật và vá lỗi

```bash
sudo apt update
sudo apt upgrade
sudo apt list --upgradable
sudo apt autoremove
```

- Trên Debian và Ubuntu, `apt update` cập nhật danh sách, `apt upgrade`
  nâng cấp gói. Trên Termux, tương đương là `pkg update && pkg upgrade`.
- Đừng trì hoãn cập nhật phần mềm có lỗ hổng đã công bố, đặc biệt là
  dịch vụ mở ra Internet như SSH, web, VPN.
- Trước khi nâng cấp hệ thống quan trọng, hãy sao lưu cấu hình và dữ
  liệu, và nâng cấp trong cửa sổ bảo trì, không nâng cấp giữa lúc hệ
  thống đang phục vụ cao điểm khi chưa có kế hoạch.

## 7.6. Bài tập Chương 7

1. Trên PC lab, xem 50 dòng log gần nhất bằng `journalctl`, tìm một
   lỗi và ghi lại dịch vụ gây lỗi.
2. Tạo một file log giả có 20 dòng đăng nhập thất bại từ 3 IP khác nhau,
   dùng `grep`, `awk`, `sort`, `uniq -c` để tìm IP nào thất bại nhiều nhất.
3. Liệt kê các user có shell đăng nhập thật trên máy lab của bạn, và
   giải thích vì sao user hệ thống lại dùng shell nologin.
4. Tìm 5 file có SUID trên máy lab, ghi lại tên và đường dẫn, kiểm tra
   xem file nào là bình thường, file nào cần hỏi thêm.

---

# Chương 8: Tường lửa, phân đoạn mạng và hardening cơ bản

## 8.1. Nguyên tắc tường lửa

- Mặc định chặn hết, chỉ mở những gì cần. Đây là nguyên tắc quan trọng
  nhất, ngược lại với mặc định mở hết rồi mới chặn từng cái.
- Tường lửa có thể chặn theo IP nguồn, IP đích, cổng, giao thức, trạng
  thái kết nối.
- Tường lửa không thay thế cho cập nhật phần mềm, mật khẩu mạnh, và
  kiểm tra quyền. Nó chỉ là một lớp trong nhiều lớp phòng thủ.

## 8.2. UFW, tường lửa đơn giản trên Ubuntu

```bash
sudo ufw status
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw deny 23/tcp
sudo ufw delete allow 80/tcp
sudo ufw status numbered
```

- `ufw enable`: bật tường lửa, cẩn thận: nếu bạn đang kết nối SSH từ xa
  mà chưa cho phép cổng 22 thì bật tường lửa có thể làm bạn mất kết nối.
  Luôn cho phép SSH trước khi bật, hoặc làm việc trực tiếp trên máy.
- `ufw allow`: cho phép, `ufw deny`: chặn, `ufw status numbered`: hiện
  danh sách có số thứ tự để xóa đúng quy tắc.

## 8.3. iptables và nftables, mức thấp hơn

```bash
sudo iptables -L -n -v
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -j DROP
sudo nft list ruleset
```

- `iptables`: công cụ tường lửa cũ nhưng vẫn gặp nhiều, `-L` liệt kê,
  `-A` thêm quy tắc, `-j ACCEPT` là cho qua, `-j DROP` là lặng lẽ bỏ gói,
  không trả lời, khác với REJECT là trả lời từ chối.
- `nftables`: công cụ mới thay thế iptables trên nhiều hệ thống mới,
  cú pháp gọn hơn, dùng `nft list ruleset` để xem toàn bộ.
- Đừng copy quy tắc tường lửa từ Internet rồi chạy ngay lên máy thật
  khi chưa hiểu từng dòng, vì một quy tắc DROP sai vị trí có thể chặn
  hết lưu lượng, kể cả SSH của bạn.

## 8.4. Phân đoạn mạng

- Mạng quản trị, mạng người dùng, mạng máy chủ, mạng IoT nên tách riêng
  bằng VLAN hoặc subnet khác nhau, không để một thiết bị IoT yếu có thể
  nói chuyện trực tiếp với máy chủ cơ sở dữ liệu.
- DMZ: vùng đệm cho máy chủ phải mở ra Internet như web, mail, nằm giữa
  tường lửa ngoài và tường lửa trong, không đặt cơ sở dữ liệu trực tiếp
  trong DMZ.
- Trên lab nhỏ, bạn có thể mô phỏng bằng 2 subnet: `192.168.1.0/24` cho
  máy người dùng, `192.168.2.0/24` cho máy chủ lab, và quy tắc chỉ cho
  phép cổng cần thiết đi qua.

## 8.5. Hardening SSH và web cơ bản

SSH, trong file `/etc/ssh/sshd_config` trên PC lab:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
Port 2222
AllowUsers ten-user-cua-ban
```

- Sau khi sửa file cấu hình, kiểm tra cú pháp và khởi động lại dịch vụ
  SSH, đừng đóng phiên SSH đang làm việc trước khi thử đăng nhập bằng
  phiên mới, để tránh tự khóa mình ngoài máy.
- Web: tắt hiển thị phiên bản chi tiết, tắt thư mục liệt kê file, chỉ
  mở cổng 80 để chuyển hướng sang 443, dùng HTTPS, đặt header bảo mật,
  cập nhật framework và plugin.

## 8.6. Bài tập Chương 8

1. Trên máy ảo lab, bật UFW, chỉ cho phép 22 và 80, rồi dùng nmap từ máy
   khác trong lab để kiểm tra xem các cổng khác có bị chặn không.
2. Viết ra 3 quy tắc tường lửa bạn muốn áp dụng cho một máy chủ web lab,
   và giải thích vì sao mỗi quy tắc cần thiết.
3. Vẽ sơ đồ 2 subnet lab của bạn, ghi rõ máy nào ở subnet nào, và cổng
   nào được phép đi qua giữa hai subnet.

---

# Chương 9: Phân tích file, mã độc cơ bản và điều tra dấu vết

Cảnh báo: chương này chỉ dùng file mẫu an toàn trong lab, file do bạn
tự tạo, hoặc file mẫu từ khóa học. Không mở file nghi ngờ mã độc trên
máy chính, không chạy file thực thi lạ, không tải file lạ về máy thật
rồi mở bằng quyền cao. Phân tích mã độc thật cần môi trường cách ly,
máy ảo snapshot, mạng ngắt, và kiến thức chuyên sâu hơn.

## 9.1. Nhận diện file

```bash
file file-la
ls -l file-la
sha256sum file-la
strings file-la | head -50
xxd file-la | head -20
hexdump -C file-la | head -20
```

- `file`: đoán loại file dựa vào chữ ký, ví dụ ELF, PE, ZIP, JPEG.
- `strings`: trích các chuỗi ký tự có thể đọc được trong file nhị phân,
  ví dụ tên miền, đường dẫn, thông báo lỗi. Chuỗi lạ không tự nó chứng
  minh file là mã độc, cần kết hợp nhiều dấu hiệu.
- `xxd`, `hexdump -C`: xem file ở dạng hex, kiểm tra chữ ký đầu file,
  ví dụ file ELF bắt đầu bằng `7f 45 4c 46`, file ZIP bắt đầu bằng
  `50 4b 03 04`.
- Tính SHA256 trước khi phân tích, để sau này đối chiếu file có bị thay
  đổi không, và để tra cứu băm trên cơ sở dữ liệu mẫu an toàn nếu cần.

## 9.2. Kiểm tra file nén và file ẩn

```bash
unzip -l file.zip
tar -tzf file.tar.gz
binwalk file.bin
```

- `unzip -l`, `tar -tzf`: liệt kê nội dung file nén mà không giải nén,
  an toàn hơn giải nén ngay.
- `binwalk`: phân tích firmware và file nhị phân để tìm file ẩn bên
  trong, chỉ dùng trong lab, cần cài thêm.
- Đừng giải nén file lạ vào thư mục hệ thống, hãy giải nén vào thư mục
  lab riêng, trong máy ảo snapshot, và xóa sau khi xong.

## 9.3. Điều tra dấu vết cơ bản, DFIR ở mức CLI

- Thu thập: thời gian, IP, user, tiến trình, kết nối mạng, file thay đổi,
  log liên quan, trước khi sửa chữa gì. Sửa chữa trước khi thu thập sẽ
  làm mất dấu vết.
- Bảo toàn: sao chép log và file quan trọng ra nơi khác, tính băm để
  chứng minh file không bị thay đổi sau khi thu thập.
- Phân tích: tìm điểm bắt đầu, ví dụ đăng nhập lạ lúc 3 giờ sáng, tiến
  trình lạ, kết nối ra IP lạ, file mới xuất hiện trong thư mục hệ thống.
- Báo cáo: ghi lại những gì đã làm, lệnh đã chạy, kết quả, kết luận và
  mức độ chắc chắn. Đừng kết luận vội từ một dấu hiệu duy nhất.
- Các lệnh hay dùng trong điều tra: `ps aux`, `ss -tulnp`, `lsof`,
  `history`, `last`, `lastlog`, `find / -mtime -1`, `journalctl`.

```bash
last
lastlog
history | tail -50
ss -tulnp
lsof -i
ps aux --sort=-%cpu | head -20
```

- `last`: xem lịch sử đăng nhập gần đây.
- `history`: xem lệnh đã gõ trong shell hiện tại, nhưng kẻ tấn công có
  thể xóa history, nên history trống cũng là một dấu hiệu cần chú ý,
  không phải bằng chứng chắc chắn là sạch.
- `lsof -i`: xem tiến trình nào đang mở kết nối mạng nào.

## 9.4. Bài tập Chương 9

1. Tự tạo một file văn bản, tính SHA256, đổi một ký tự, tính lại và so
   sánh.
2. Dùng `strings` trên một file nhị phân hệ thống trong lab, ghi lại 5
   chuỗi bạn hiểu được và 5 chuỗi bạn chưa hiểu, rồi tra cứu từng chuỗi
   chưa hiểu.
3. Viết một kịch bản điều tra giả định: một máy lab có đăng nhập SSH lạ
   lúc 3 giờ sáng, bạn sẽ chạy những lệnh nào theo thứ tự nào, và ghi
   lại bằng chứng gì trước khi sửa chữa.

---
# Chương 10: Tự động hóa bằng script cho an ninh mạng

Script trong an ninh mạng không phải để làm phần mềm lớn. Script là để
gom các lệnh bạn vẫn gõ tay thành một file, để máy tự gõ lại, tự ghi log,
tự báo cáo, đỡ sai sót khi lặp lại nhiều lần.

## 10.1. Script Bash đầu tiên, an toàn

Tạo file bằng trình soạn thảo, ví dụ `nano`, không dùng cách nối chuỗi
phức tạp khi bạn mới học. Mỗi script chỉ làm một việc, chạy được thật,
rồi mới thêm tính năng mới.

```bash
#!/bin/bash
# Kiem tra may chu scanme co song khong, chi dung trong lab
set -e
echo "Bat dau kiem tra: $(date)"
ping -c 2 scanme.nmap.org
echo "Ket thuc: $(date)"
```

- Dòng đầu `#!/bin/bash` gọi là shebang, cho hệ thống biết dùng Bash
  để chạy file này.
- `set -e`: gặp lỗi thì dừng ngay, không chạy tiếp trong trạng thái lỗi.
- `$(date)`: chèn ngày giờ hiện tại vào output.
- Cho quyền chạy: `chmod +x kiem-tra.sh`, chạy: `./kiem-tra.sh`.

Script quét và lưu kết quả, dùng lại kiến thức nmap đã học:

```bash
#!/bin/bash
set -e
MUC_TIEU="scanme.nmap.org"
NGAY=$(date +%Y-%m-%d_%H-%M)
nmap -p 22,80,9929 -sV -oN "ketqua-${NGAY}.txt" "$MUC_TIEU"
echo "Da luu ket qua vao ketqua-${NGAY}.txt"
```

- Biến không có khoảng trắng quanh dấu `=`.
- Dùng dấu ngoặc kép quanh biến `"$MUC_TIEU"` để tránh lỗi khi tên có
  khoảng trắng hoặc ký tự đặc biệt.
- Tên file có ngày giờ để lần sau không ghi đè lên kết quả cũ.

## 10.2. Điều kiện, vòng lặp và hàm

```bash
#!/bin/bash
if ping -c 1 -W 2 8.8.8.8 > /dev/null 2>&1; then
  echo "Mang dang hoat dong"
else
  echo "Khong co mang, kiem tra lai WiFi"
fi

for CONG in 22 80 443; do
  echo "Dang kiem tra cong $CONG"
  nmap -p "$CONG" scanme.nmap.org
done

kiem_tra_cong() {
  local CONG="$1"
  echo "Kiem tra cong $CONG"
  nmap -p "$CONG" scanme.nmap.org
}

kiem_tra_cong 80
```

- `if` phải có `then`, và khối lệnh phải nằm giữa `then` và `fi`.
  Thiếu dấu `{` cùng dòng với `if` trong Go là một lỗi khác, trong Bash
  thì cấu trúc là `if ...; then ... fi`, đừng nhầm hai ngôn ngữ.
- `for` lặp qua danh sách, mỗi lần biến nhận một giá trị.
- Hàm giúp bạn không lặp lại cùng một đoạn lệnh nhiều lần, và giúp
  script dễ đọc hơn. `local` dùng để biến trong hàm không ảnh hưởng
  ra ngoài.

## 10.3. Đọc file, xử lý log bằng script

```bash
#!/bin/bash
while read -r DONG; do
  echo "Dang xu ly: $DONG"
done < danhsach-ip.txt
```

- `while read` đọc từng dòng trong file, an toàn hơn việc dùng `for`
  với file có khoảng trắng.
- Đừng dùng script để đọc mật khẩu thật từ file rồi gửi đi nơi khác.
  Script xử lý log chỉ nên đọc IP, thời gian, mã lỗi, không đọc nội
  dung nhạy cảm rồi đăng công khai.

## 10.4. Python một dòng và script nhỏ cho an ninh

Python rất hay để xử lý log, JSON, HTTP, vì có nhiều thư viện sẵn.

```bash
python -c "import json, sys; data=json.load(sys.stdin); print(data.keys())" < file.json
python -m http.server 8000
```

- Lệnh thứ hai chạy máy chủ web tĩnh trong thư mục hiện tại, cổng 8000,
  chỉ dùng trong mạng lab riêng, tắt ngay khi xong bằng Ctrl+C, đừng
  để máy chủ này mở ra Internet.
- Khi bạn viết script Python cho an ninh, hãy bắt đầu từ script đọc
  file, đếm, lọc, rồi mới tới script gửi request, và luôn có chế độ
  chạy thử trên dữ liệu lab trước.

## 10.5. Lập lịch bằng cron

```bash
crontab -e
crontab -l
```

Ví dụ chạy script kiểm tra mỗi ngày lúc 7 giờ sáng, ghi log ra file:

```text
0 7 * * * /home/user/kiem-tra.sh >> /home/user/kiem-tra.log 2>&1
```

- 5 trường đầu là phút, giờ, ngày trong tháng, tháng, ngày trong tuần.
  `0 7 * * *` nghĩa là 0 phút, 7 giờ, mọi ngày, mọi tháng, mọi ngày
  trong tuần.
- Đừng đặt cron chạy quét dồn dập mỗi phút lên hệ thống người khác.
  Cron cho quét lab của bạn nên thưa, ví dụ mỗi ngày một lần là đủ.

## 10.6. Bài tập Chương 10

1. Viết một script Bash chỉ làm một việc: ping scanme 2 gói và in ngày
   giờ bắt đầu và kết thúc.
2. Viết một script quét 3 cổng đã biết của scanme, lưu kết quả vào file
   có ngày giờ trong tên, rồi đọc lại file bằng `cat`.
3. Viết một script dùng `for` để quét 3 cổng 22, 80, 443, mỗi cổng một
   lần, in ra dòng “Đang kiểm tra cổng X” trước mỗi lần quét.
4. Đặt một cron chạy script kiểm tra của bạn mỗi ngày một lần trong lab,
   kiểm tra bằng `crontab -l` xem cron đã ghi đúng chưa.

---

# Chương 11: Lộ trình, chứng chỉ và bài tập tổng hợp

## 11.1. Lộ trình cho người học trên điện thoại trước, PC sau

Giai đoạn 1, điện thoại Termux, 4 tới 8 tuần, mỗi ngày 20 tới 30 phút:

- Tuần 1 và 2: Chương 1 và Chương 2, CLI, file, quyền, ping, traceroute,
  dig, curl.
- Tuần 3 và 4: Chương 3, nmap cơ bản, `-p`, `-sV`, `-sn`, `-oN`, `-sC`,
  `--script` riêng, đọc kết quả scanme.
- Tuần 5 và 6: Chương 5 và Chương 7 ở mức đọc hiểu, PortSwigger lab web
  cơ bản, đọc log bằng grep và awk.
- Tuần 7 và 8: Chương 10, viết script Bash đầu tiên, lưu kết quả quét,
  tự động hóa việc lặp lại.

Giai đoạn 2, PC Linux và máy ảo, 2 tới 4 tháng:

- Cài Ubuntu hoặc Debian trong máy ảo, học `apt`, `systemd`, `journalctl`,
  UFW, SSH key.
- Học tcpdump và tshark trên PC lab, học `-sU`, `-O`, `-sS` với `sudo`.
- Học web sâu hơn: OWASP Top 10, PortSwigger trung cấp, DVWA và Juice
  Shop trong lab riêng.
- Học Python cho an ninh: đọc log, gửi request, xử lý JSON.

Giai đoạn 3, chuyên sâu, 6 tới 12 tháng trở lên:

- Chọn một hướng: Blue Team thì học SIEM, phát hiện xâm nhập, ứng cứu
  sự cố, phân tích log nâng cao. Red Team thì học pentest có phép, khai
  thác trong lab, báo cáo.
- Học mạng nâng cao: VLAN, VPN, TLS, DNSSEC, tường lửa doanh nghiệp.
- Học lập trình đủ để đọc code, viết tool nhỏ, hiểu lỗ hổng từ code,
  không cần trở thành lập trình viên chuyên nghiệp ngay.

## 11.2. Chứng chỉ tham khảo, không bắt buộc lúc mới học

- CompTIA Security+: nền tảng an ninh chung, phù hợp người mới, thi
  trắc nghiệm, không cần lab nặng.
- eJPT của INE: pentest cơ bản, có lab thực hành, phù hợp sau khi bạn
  đã quen nmap và web cơ bản.
- CompTIA CySA+: phân tích an ninh, Blue Team, phù hợp sau Security+.
- OSCP của OffSec: pentest nâng cao, lab nặng, thi thực hành 24 giờ,
  chỉ nên thi khi bạn đã làm lab nhiều tháng và có PC đủ mạnh.
- Chứng chỉ không thay thế kỹ năng lab thật. Nhà tuyển dụng thường hỏi
  bạn đã làm lab nào, đã viết báo cáo nào, đã tự động hóa việc gì, nhiều
  hơn là hỏi bạn có bao nhiêu chứng chỉ.

## 11.3. Bài tập tổng hợp cuối giáo trình

Bài 1, trinh sát mạng nhà bạn, chỉ mạng nhà bạn:

- Dùng `-sn` tìm máy sống, ghi lại gateway, IP của bạn, số máy sống.
- Dùng nmap quét một máy trong nhà bạn mà bạn sở hữu, ví dụ máy ảo lab,
  ghi lại cổng mở và dịch vụ.

Bài 2, trinh sát scanme:

- Quét cơ bản, ghi lại 3 cổng mở và thời gian quét.
- Quét `-sV` và `-sC` với 3 cổng đó, ghi lại phiên bản và script output.
- Lưu tất cả vào file bằng `-oN`, đọc lại file bằng `cat`.

Bài 3, web lab:

- Dùng `curl -I` và `curl -v` trên một trang lab, ghi lại mã trạng thái
  và header bảo mật còn thiếu.
- Làm xong một bài PortSwigger về SQL injection hoặc XSS ở mức cơ bản,
  ghi lại cơ chế và cách sửa.

Bài 4, phòng thủ:

- Trên máy ảo lab, bật UFW, chỉ cho phép SSH và web, dùng nmap từ máy
  khác trong lab để kiểm tra.
- Đọc log SSH trong lab, tìm IP đăng nhập thất bại nhiều nhất bằng
  `grep` và `awk`.

Bài 5, tự động hóa:

- Viết một script Bash quét 3 cổng của scanme, lưu kết quả vào file có
  ngày giờ, in ra dòng bắt đầu và kết thúc.
- Đặt cron chạy script đó mỗi ngày một lần trong lab, kiểm tra bằng
  `crontab -l`.

Khi làm xong 5 bài này, bạn đã có một bộ hồ sơ lab cơ bản để kể lại
khi người khác hỏi bạn đã học và làm được gì, thay vì chỉ nói bạn đã
đọc giáo trình.

---

# Chương 12: Bảng tra cứu nhanh

## 12.1. Lệnh Linux hay dùng

- Xem thư mục: `pwd`, `ls -l`, `cd`
- Tạo và đọc file: `mkdir`, `touch`, `cat`, `less`, `head`, `tail`
- Tìm và lọc: `grep`, `sort`, `uniq -c`, `cut`, `awk`, `sed`, `wc -l`
- Tiến trình và tài nguyên: `ps aux`, `top`, `htop`, `free -h`, `df -h`
- Mạng: `ip a`, `ip r`, `ss -tuln`, `ping -c 4`, `traceroute`
- Gói: Termux `pkg update && pkg upgrade`, `pkg install ten-goi`,
  Ubuntu và Debian `sudo apt update && sudo apt upgrade`

## 12.2. Lệnh Nmap hay dùng, không cần root

- `nmap scanme.nmap.org`
- `nmap -sn 192.168.1.0/24`
- `nmap -p 22,80,9929 scanme.nmap.org`
- `nmap -p 22,80,9929 -sV scanme.nmap.org`
- `nmap -sC -p 22,80,9929 scanme.nmap.org`
- `nmap --script http-title -p 80 scanme.nmap.org`
- `nmap -p 22,80,9929 -sV -oN ketqua.txt scanme.nmap.org`
- `nmap -v scanme.nmap.org`
- `nmap -Pn scanme.nmap.org`
- `nmap -T4 scanme.nmap.org`

## 12.3. Lệnh Nmap cần root, dùng trên PC có sudo

- `sudo nmap -sS scanme.nmap.org`
- `sudo nmap -sU -p 53 scanme.nmap.org`
- `sudo nmap -O scanme.nmap.org`
- `sudo nmap -A scanme.nmap.org`

## 12.4. Trạng thái cổng Nmap

- `open`: mở, có dịch vụ, tường lửa cho qua.
- `closed`: máy trả lời, không có dịch vụ ở cổng đó.
- `filtered`: tường lửa chặn, không biết mở hay đóng.
- `open|filtered`: thường gặp ở UDP, không chắc được.
- `unfiltered`: truy cập được nhưng không biết mở hay đóng, thường gặp
  ở quét ACK.

## 12.5. Mã HTTP thường gặp

- 200 OK, 201 Created, 204 No Content
- 301 Moved Permanently, 302 Found
- 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found
- 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable

## 12.6. Bản ghi DNS thường gặp

- A: IPv4, AAAA: IPv6, CNAME: bí danh, MX: mail, NS: máy chủ tên miền,
  TXT: văn bản xác minh, SOA: thông tin vùng, PTR: tra ngược IP thành tên.

## 12.7. Cổng TCP và UDP thường gặp

Xem lại Chương 2, mục 2.7. Khi bạn quên một cổng, hãy tra bảng đó trước,
đừng đoán theo cảm tính. Cổng 22 là SSH, 53 là DNS, 80 là HTTP, 443 là
HTTPS, 3306 là MySQL, 5432 là PostgreSQL.

---

# Phụ lục: Lỗi thường gặp và cách gỡ

## Lỗi 1: Failed to resolve "80", Failed to resolve "9929"

Nguyên nhân: bạn gõ các cổng cách nhau bằng khoảng trắng, ví dụ
`nmap -p 22 80 9929 -sV scanme.nmap.org`. Nmap chỉ hiểu `-p 22`, còn
`80` và `9929` bị hiểu thành tên máy chủ, nên đi tra DNS và thất bại.

Cách sửa: nối các cổng bằng dấu phẩy, không có khoảng trắng:
`nmap -p 22,80,9929 -sV scanme.nmap.org`.

## Lỗi 2: You requested a scan type which requires root privileges. QUITTING!

Nguyên nhân: bạn dùng kiểu quét cần root như `-sU`, `-O`, `-sS` trên
máy không root, ví dụ Termux trên điện thoại chưa root.

Cách sửa: trên điện thoại, đừng root máy chỉ để chạy lệnh này. Học lý
thuyết trước, lên PC thì thêm `sudo` vào đầu, ví dụ
`sudo nmap -sU -p 53 scanme.nmap.org`. Lỗi này không phải bạn gõ sai
cú pháp, mà là thiếu quyền.

## Lỗi 3: NSE: failed to initialize the script engine, 'http' did not match

Nguyên nhân: tên script bạn gõ không khớp với tên script, nhóm script,
hay file nào cả. `http` một mình không phải tên script.

Cách sửa: gõ tên đầy đủ, ví dụ `http-title`, hoặc dùng dấu sao trong
ngoặc kép để chạy hết nhóm bắt đầu bằng http: `--script "http-*"`.
Mấy dòng stack traceback phía dưới là chi tiết nội bộ, bạn chỉ cần đọc
dòng `'http' did not match`.

## Lỗi 4: Termux trên CH Play báo không tự cập nhật, thiết kế cho Android cũ

Nguyên nhân: bản CH Play bị kẹt ở chuẩn Android cũ, nhóm Termux đã
ngừng cập nhật bản đó, vì nâng chuẩn sẽ làm gãy khả năng chạy file
Linux trong thư mục riêng do quy tắc W^X từ Android 10.

Cách sửa: gỡ bản CH Play nếu chưa dùng gì quan trọng, cài F-Droid từ
đúng `f-droid.org`, rồi cài Termux trong F-Droid. Cảnh báo “thiết kế
cho Android cũ” với Termux là bình thường, không phải máy bạn quá cũ.

## Lỗi 5: pkg install không tìm thấy gói, repository không có Release file

Nguyên nhân thường gặp nhất: bạn đang dùng bản Termux CH Play cũ, kho
gói đã chết, hoặc danh sách gói chưa cập nhật, hoặc mạng yếu làm tải
danh sách bị lỗi giữa chừng.

Cách sửa theo thứ tự: kiểm tra bạn đang dùng bản F-Droid hay CH Play,
chạy `pkg update` rồi chạy lại, đổi mạng WiFi ổn định hơn, chạy lại
`pkg update && pkg upgrade`. Nếu vẫn lỗi kho, hãy kiểm tra lại nguồn
cài Termux trước, đừng tải file APK Termux từ web lạ.

## Lỗi 6: Quét nmap rất lâu, hơn 90 giây

Nguyên nhân: quét 1000 cổng qua Internet, nhiều cổng không trả lời,
nmap phải chờ hết thời gian chờ và thử lại, cộng với mạng điện thoại
và máy chủ ở xa.

Cách sửa: đây là bình thường, không phải lỗi. Muốn nhanh hơn, hãy quét
chỉ các cổng đã biết bằng `-p 22,80,9929`, hoặc dùng `--top-ports 20`
khi chỉ cần kiểm tra nhanh, hoặc dùng `-T4` trong lab.

## Lỗi 7: Chữ O và số 0 trong -O

Nguyên nhân: `-O` là chữ O trong từ OS, viết hoa, không phải số 0.
Gõ số 0 sẽ báo sai tùy chọn.

Cách sửa: gõ chữ O hoa, ví dụ `nmap -O scanme.nmap.org`, trên PC có
sudo thì `sudo nmap -O scanme.nmap.org`.

## Lỗi 8: Gõ -sV thành -sv

Nguyên nhân: `-sV` là Service Version, chữ V viết hoa. `-sv` với `v`
thường là verbose, hiện chi tiết, không phải dò phiên bản.

Cách sửa: kiểm tra lại chữ hoa, chữ thường trong mọi tùy chọn nmap,
vì nmap phân biệt hoa thường rất chặt.

---

## Lời kết

Bạn không cần thuộc hết giáo trình này trong một đêm. Bạn chỉ cần làm
đúng một việc mỗi ngày: chọn một lệnh, chạy trong lab được phép, đọc
kết quả, ghi lại, rồi mai làm tiếp lệnh sau. Nền tảng vững, thói quen
ghi chép cẩn thận, và nguyên tắc chỉ thử hệ thống được phép sẽ đưa bạn
đi xa hơn việc chạy thật nhiều lệnh mà không hiểu chúng làm gì.

Khi bạn có PC, hãy quay lại Chương 3, Chương 4 và Chương 8, chạy lại
các lệnh cần root bằng `sudo` trong lab riêng, và so sánh kết quả với
những gì bạn đã làm trên điện thoại. Bạn sẽ thấy mọi lệnh đều giống
nhau, chỉ khác ở quyền và tốc độ, còn kiến thức thì đã mang theo được
từ điện thoại sang PC.

Chúc bạn học bền, học chắc, và luôn làm an ninh mạng một cách tử tế
và có phép.

---

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
