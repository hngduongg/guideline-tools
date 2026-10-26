# Giáo trình Shell đầy đủ và chi tiết

> Dành cho người đã biết gõ lệnh trên terminal, đọc được shell nhưng chưa
> tự viết được script. Học theo nhịp: mỗi bài chỉ một việc thật, một khái
> niệm mới, chạy được thật rồi mới sang bài sau.
>
> Cách học đúng với giáo trình này:
>
> 1. Đọc mục tiêu của chương trước, đừng đọc hết rồi mới làm.
> 2. Gõ lại ví dụ bằng tay, chạy thật, xem kết quả thật.
> 3. Tự bóc ví dụ ra tiếng Việt, từng dòng một, trước khi xem phần bóc sẵn.
> 4. Làm bài tập cuối chương, chỉ một việc thật, rồi mới sang chương sau.
> 5. Không thuộc lòng dấu và từ tiếng Anh. Quên thì tra lại Phụ lục A và B.
>    Người viết shell giỏi cũng tra, khác nhau ở chỗ họ biết tra cái gì.
>
> Công cụ cần có: một máy Linux hoặc Termux trên điện thoại, trình soạn
> thảo (trên điện thoại có thể viết bằng trình soạn thảo GitHub, rồi tải
> về), và `bash`. Không cần cài gì thêm cho phần lớn giáo trình này.

---

## Mục lục

- [Phần 0. Chuẩn bị và nguyên tắc học](#phần-0-chuẩn-bị-và-nguyên-tắc-học)
- [Phần 1. Shell là gì, terminal là gì](#phần-1-shell-là-gì-terminal-là-gì)
- [Phần 2. Lệnh, tham số, đường dẫn, biến môi trường](#phần-2-lệnh-tham-số-đường-dẫn-biến-môi-trường)
- [Phần 3. Biến trong shell](#phần-3-biến-trong-shell)
- [Phần 4. Bảng tra dấu và ký tự đặc biệt](#phần-4-bảng-tra-dấu-và-ký-tự-đặc-biệt)
- [Phần 5. Kiểm tra điều kiện: test, `[ ]`, `[[ ]]`](#phần-5-kiểm-tra-điều-kiện-test---)
- [Phần 6. Rẽ nhánh: if và case](#phần-6-rẽ-nhánh-if-và-case)
- [Phần 7. Vòng lặp: for, while, until](#phần-7-vòng-lặp-for-while-until)
- [Phần 8. Hàm](#phần-8-hàm)
- [Phần 9. Xử lý tham số dòng lệnh](#phần-9-xử-lý-tham-số-dòng-lệnh)
- [Phần 10. Vào ra, chuyển hướng, đường ống](#phần-10-vào-ra-chuyển-hướng-đường-ống)
- [Phần 11. Xử lý văn bản: grep, sed, awk, cut, sort](#phần-11-xử-lý-văn-bản-grep-sed-awk-cut-sort)
- [Phần 12. File, thư mục và quyền](#phần-12-file-thư-mục-và-quyền)
- [Phần 13. Tiến trình, hệ thống, chạy định kỳ](#phần-13-tiến-trình-hệ-thống-chạy-định-kỳ)
- [Phần 14. Mạng trong shell, chỉ trên máy và mạng của mình](#phần-14-mạng-trong-shell-chỉ-trên-máy-và-mạng-của-mình)
- [Phần 15. Lỗi, mã thoát và gỡ lỗi](#phần-15-lỗi-mã-thoát-và-gỡ-lỗi)
- [Phần 16. Quy trình viết một script thực tế từ đầu đến cuối](#phần-16-quy-trình-viết-một-script-thực-tế-từ-đầu-đến-cuối)
- [Phần 17. Bốn script mẫu hoàn chỉnh](#phần-17-bốn-script-mẫu-hoàn-chỉnh)
- [Phần 18. An toàn và bảo mật khi viết script](#phần-18-an-toàn-và-bảo-mật-khi-viết-script)
- [Phần 19. Sau shell thì học gì tiếp](#phần-19-sau-shell-thì-học-gì-tiếp)
- [Phụ lục A. 20 từ tiếng Anh hay gặp nhất](#phụ-lục-a-20-từ-tiếng-anh-hay-gặp-nhất)
- [Phụ lục B. Bảng tra nhanh](#phụ-lục-b-bảng-tra-nhanh)
- [Phụ lục C. Lộ trình 30 ngày và checklist tự đánh giá](#phụ-lục-c-lộ-trình-30-ngày-và-checklist-tự-đánh-giá)

---

## Phần 0. Chuẩn bị và nguyên tắc học

### 0.1. Ba thứ làm người mới thấy rối, và cách gỡ từng thứ

Nhiều người mới không rối vì shell khó, mà rối vì ba thứ dồn lại một lúc:

1. **Dấu:** `[]`, `<>`, `\`, `/`, `()`, `;`, `:`, `""`, `*`, `#`, `$`, `|`,
   `=`, `,`, `{}`, `&`, `%`, `''`, `!`, `?`, `-`. Dấu không có nghĩa cố
   định, nghĩa của nó đổi theo chỗ đứng. Ví dụ `$` trước tên biến là
   "lấy giá trị của biến", nhưng `$1` là "tham số thứ nhất", còn `$$` là
   "số tiến trình của chính script này". Vì vậy đừng học dấu rời rạc,
   học dấu trong một dòng thật, trong một ngữ cảnh thật. Quên thì tra
   Phần 4.
2. **Tiếng Anh:** shell chỉ dùng đi dùng lại khoảng 20 từ: `if` (nếu),
   `then` (thì), `else` (nếu không), `fi` (hết if), `for` (với mỗi),
   `while` (trong khi), `do` (làm), `done` (hết vòng lặp), `case`
   (trường hợp), `esac` (hết case), `echo` (in ra), `read` (đọc vào),
   `exit` (thoát), `return` (trả về), `shift` (dịch tham số), `test`
   (kiểm tra), `source` (nạp vào shell hiện tại). Xem Phụ lục A, dịch
   bằng lời của chính mình, rồi dùng tên biến tiếng Việt, thông báo
   tiếng Việt trong script của mình.
3. **Tư duy kể bước:** viết script không bắt đầu bằng code, mà bắt đầu
   bằng việc kể các bước bằng tiếng Việt, theo đúng thứ tự mình làm
   bằng tay. Ví dụ việc "sao lưu một thư mục" kể ra là: (1) chọn thư mục
   nguồn, (2) tạo tên file đích theo ngày, (3) nén lại, (4) kiểm tra file
   đích có thật và lớn hơn 0 không, (5) in ra kết quả. Kể được bước thì
   mỗi bước dịch thành một hoặc hai dòng lệnh, thế là thành script.

### 0.2. Quy tắc vàng của giáo trình này

- Một file chỉ làm một việc. Một việc lặp lại thì tách thành hàm.
- Tên biến, tên hàm, thông báo viết bằng tiếng Việt không dấu hoặc có
  dấu đều được, miễn là chính mình đọc hiểu ngay, ví dụ `thu_muc_nguon`,
  `tap_tin_dich`, `dem_loi`.
- Luôn đặt biến trong dấu ngoặc kép: `"$ten_file"`, không viết `$ten_file`
  trần, trừ khi cố ý muốn tách từ. Đây là lỗi gây ra nhiều tai nạn nhất.
- Chạy thử trên dữ liệu giả, thư mục giả, file giả trước, rồi mới chạy
  trên dữ liệu thật. Script an ninh thì ưu tiên chỉ đọc, không sửa, không
  xóa trong lần chạy đầu tiên.
- Trước khi chạy script của người khác, đọc hết script, hiểu từng dòng
  làm gì, rồi mới chạy. Không chạy `curl ... | bash` từ nguồn không tin.

### 0.3. Kiểm tra môi trường của mình

```bash
bash --version
which bash
echo "$SHELL"
pwd
ls
```

Bóc ví dụ:

- `bash --version`: in ra phiên bản bash đang dùng.
- `which bash`: cho biết file chương trình `bash` nằm ở đâu.
- `echo "$SHELL"`: in ra shell mặc định của tài khoản.
- `pwd`: in ra thư mục đang đứng, viết tắt của Print Working Directory,
  không phải password. Đổi mật khẩu là `passwd`, khác hẳn.
- `ls`: liệt kê file trong thư mục hiện tại.

Bài tập phần 0: mở terminal, chạy 5 lệnh trên, ghi lại kết quả vào một
file `kiem-tra-moi-truong.txt` bằng lệnh `echo` và chuyển hướng `>`.

---

## Phần 1. Shell là gì, terminal là gì

### 1.1. Phân biệt 4 khái niệm hay bị lẫn

- **Terminal (thiết bị đầu cuối):** cái cửa sổ để mình gõ chữ vào và xem
  chữ máy in ra. Trên điện thoại, Termux là một terminal. Trên Linux,
  GNOME Terminal, Konsole cũng là terminal.
- **Shell:** chương trình đọc lệnh mình gõ, hiểu lệnh, rồi bảo hệ điều
  hành làm. `bash` là một shell, `sh` là shell đơn giản hơn, `zsh` là
  một shell khác. Khi mình gõ `ls`, chính shell là thứ tìm chương trình
  `ls` và chạy nó.
- **Lệnh (command):** có hai loại. Lệnh nội tại (builtin) là lệnh shell tự
  làm, ví dụ `cd`, `echo`, `pwd`, `read`. Lệnh ngoài là chương trình riêng
  nằm trong một file, ví dụ `ls`, `grep`, `awk`, `curl`, `nmap`. Phân
  biệt bằng `type ten_lenh`.
- **Script:** một file văn bản chứa nhiều lệnh theo thứ tự, để máy tự gõ
  hộ mình. Script không phải một ngôn ngữ khác hẳn lệnh, script chính là
  các lệnh mình vẫn gõ, được lưu lại thành file.

### 1.2. Bash là viết tắt của gì

`bash` là Bourne Again SHell. Bourne là họ của Stephen Bourne, tác giả
shell Bourne đời đầu. "Bourne Again" chơi chữ với "born again", nghĩa là
"sinh ra thêm một lần nữa", tức là phiên bản làm lại, tốt hơn của shell
Bourne. Biết vậy để khỏi đoán mò thành các cụm khác.

### 1.3. Dòng shebang, dòng đầu tiên của mọi script

```bash
#!/usr/bin/env bash
```

Bóc từng phần:

- `#`: ở đầu file script, khi đi cùng `!` ngay sau nó thành một trường hợp
  đặc biệt, không phải chú thích thường. Hệ điều hành đọc dòng này để
  biết dùng chương trình nào để chạy file.
- `!`: dấu cảm thán, ở đây chỉ là một phần của ký hiệu `#!`.
- `/usr/bin/env`: chương trình `env`, nhiệm vụ của nó là tìm `bash` trong
  `PATH` rồi chạy. Cách này linh hoạt hơn ghi cứng `/bin/bash`, vì trên
  Termux `bash` không nằm ở `/bin/bash`.
- `bash`: tên shell sẽ chạy script.

Nếu chạy bằng `bash ten-script.sh`, hệ thống không cần dòng shebang,
vì mình đã nói rõ dùng bash. Nếu chạy bằng `./ten-script.sh`, hệ thống
cần dòng shebang và cần quyền chạy.

### 1.4. Ba cách chạy một script, và khi nào dùng cách nào

```bash
bash chao-hoang.sh
./chao-hoang.sh
source chao-hoang.sh
```

Ví dụ file `chao-hoang.sh`:

```bash
#!/usr/bin/env bash
echo "Chào Hoàng"
ls
```

- `bash chao-hoang.sh`: đơn giản nhất khi học, không cần quyền chạy,
  không cần shebang cũng chạy được. Dùng cách này khi thử nghiệm.
- `./chao-hoang.sh`: cần `chmod +x chao-hoang.sh` trước. Cách này dùng
  khi script đã xong, muốn chạy như một chương trình thật.
- `source chao-hoang.sh`: chạy ngay trong shell hiện tại, mọi biến, mọi
  `cd` trong script đều ảnh hưởng tới shell của mình sau đó. Dùng khi
  muốn nạp biến, nạp hàm, hoặc đổi thư mục và giữ lại sau khi script xong.
  Lệnh `cd` là lệnh nội tại vì lý do này: nếu `cd` là chương trình con,
  nó đổi thư mục của chính nó rồi thoát, shell cha không đổi theo.

Lưu ý trên Termux và Android: file để trong `/sdcard/download` thường
không giữ được quyền chạy, vì bộ nhớ Android gắn ở chế độ noexec và
không giữ quyền Unix. Cách làm đúng là copy file về thư mục home của
Termux (`~/`), rồi `chmod +x` và chạy ở đó, hoặc chạy bằng `bash file.sh`.

### 1.5. Chú thích

```bash
#!/usr/bin/env bash
# Đây là chú thích, shell bỏ qua dòng này.
echo "Chào Hoàng"  # Chú thích cũng có thể đứng sau một lệnh.
```

Đầu file nên có 3 dòng chú thích: script này làm gì, chạy như thế nào,
và nó chỉ đọc hay có sửa, xóa gì không. Người đọc sau, kể cả chính mình
một tháng sau, sẽ cảm ơn 3 dòng đó.

Bài tập phần 1: viết file `chao-hoang.sh` như trên, chạy bằng cả ba cách,
ghi lại khác nhau giữa `bash` và `source` khi thêm một dòng `cd /tmp`
vào script rồi chạy lại.

---

## Phần 2. Lệnh, tham số, đường dẫn, biến môi trường

### 2.1. Cấu trúc một lệnh

```bash
ls -l /tmp
```

- `ls`: tên lệnh.
- `-l`: tùy chọn, bắt đầu bằng dấu `-`, nghĩa là "in chi tiết".
- `/tmp`: tham số, là thứ lệnh sẽ làm việc lên.

Nhiều lệnh có `--help` để xem cách dùng ngắn, và `man ten_lenh` để xem
tài liệu đầy đủ. Ví dụ `ls --help`, `man ls`. Trong `man`, bấm `q` để
thoát, `/từ-cần-tìm` để tìm.

### 2.2. Đường dẫn tuyệt đối và tương đối

- Đường dẫn tuyệt đối bắt đầu bằng `/`, ví dụ `/home/hoang/tai-lieu`.
  Nó chỉ đúng một chỗ duy nhất, đi từ gốc cây thư mục.
- Đường dẫn tương đối bắt đầu từ thư mục đang đứng, ví dụ `tai-lieu`,
  `./tap-tin.txt`, `../thu-muc-khac`.
- `.` là thư mục hiện tại, `..` là thư mục cha, `~` là thư mục home của
  mình, `-` trong `cd -` là thư mục vừa đứng trước đó.

```bash
pwd
cd /tmp
cd ..
cd ~
cd -
ls -a
```

`ls -a` hiện cả file ẩn, là file bắt đầu bằng dấu `.`, ví dụ `.bashrc`.

### 2.3. Biến môi trường và PATH

Biến môi trường là biến mà shell và mọi chương trình con đều nhìn thấy,
ví dụ `HOME`, `PATH`, `PWD`, `USER`, `SHELL`, `LANG`.

```bash
echo "$HOME"
echo "$PATH"
echo "$PWD"
```

`PATH` là danh sách các thư mục, cách nhau bằng dấu `:`, mà shell sẽ tìm
chương trình khi mình gõ tên lệnh không kèm đường dẫn. Ví dụ gõ `ls`,
shell tìm lần lượt trong các thư mục của `PATH` cho đến khi thấy file
`ls` có quyền chạy.

```bash
which ls
type ls
type cd
type echo
```

- `which ls`: in ra đường dẫn của chương trình `ls`.
- `type ls`: cho biết `ls` là chương trình ngoài.
- `type cd`: cho biết `cd` là lệnh nội tại của shell.
- `type echo`: trên bash, `echo` thường là lệnh nội tại, dù ngoài kia
  cũng có một chương trình `echo`.

### 2.4. Lịch sử lệnh và phím tắt hay dùng

- Mũi tên lên, xuống: xem lại lệnh cũ.
- `history`: xem danh sách lệnh đã gõ.
- `Ctrl + C`: dừng lệnh đang chạy.
- `Ctrl + L`: xóa màn hình, tương đương `clear`.
- `Ctrl + D`: thoát shell khi đang ở dấu nhắc trống.
- `Tab`: tự hoàn thành tên lệnh, tên file, bấm hai lần để xem gợi ý.

Bài tập phần 2: dùng `cd`, `pwd`, `ls` đi từ home vào `/tmp`, quay lại
bằng `cd -`, rồi dùng `which` và `type` để phân biệt `ls`, `cd`, `grep`,
`awk`, ghi kết quả ra giấy hoặc ra file.

---

## Phần 3. Biến trong shell

### 3.1. Khi nào cần dùng biến

Dùng biến khi một giá trị được dùng nhiều lần, hoặc khi giá trị đó sẽ đổi
theo từng lần chạy, ví dụ tên thư mục nguồn, ngày hôm nay, tên file đích.
Không dùng biến thì mỗi lần đổi một chỗ phải sửa khắp file, rất dễ sót.

### 3.2. Gán biến và lấy giá trị

```bash
ten="Hoàng"
thu_muc_nguon="/tmp/tai-lieu"
ngay_hom_nay=$(date +%Y-%m-%d)
echo "Chào $ten"
echo "Nguồn: $thu_muc_nguon"
echo "Hôm nay: $ngay_hom_nay"
```

Quy tắc gán, sai một dấu là hỏng:

- Không có khoảng trắng quanh dấu `=`. Viết `ten="Hoàng"` là đúng,
  viết `ten = "Hoàng"` là sai, shell sẽ hiểu `ten` là một lệnh.
- Lấy giá trị thì thêm `$` trước tên: `$ten`, `$thu_muc_nguon`.
- `$(date +%Y-%m-%d)` là thay thế lệnh: chạy lệnh trong `$( )`, lấy đầu
  ra của nó làm giá trị. Cách cũ dùng dấu backtick `` `date` `` cũng làm
  được việc này, nhưng `$( )` dễ đọc và dễ lồng nhau hơn.

### 3.3. Dấu ngoặc kép, ngoặc đơn, và không có ngoặc

Đây là phần quan trọng nhất của chương này, vì nó quyết định biến có bị
tách, có bị thay thế hay không.

```bash
ten="Hoàng"
echo "Chào $ten"
echo 'Chào $ten'
echo Chào $ten
```

- `"Chào $ten"`: ngoặc kép cho phép thay biến, in ra `Chào Hoàng`.
- `'Chào $ten'`: ngoặc đơn giữ nguyên chữ, in ra đúng `Chào $ten`,
  không thay biến. Dùng ngoặc đơn khi muốn giữ nguyên văn, ví dụ mẫu
  tìm kiếm chứa `$`, `*`, khoảng trắng.
- `Chào $ten` không có ngoặc: vẫn thay biến, nhưng nếu giá trị có khoảng
  trắng hoặc ký tự đặc biệt thì sẽ bị tách thành nhiều từ, gây lỗi.
  Vì vậy quy tắc mặc định là luôn viết `"$ten"`.

Ví dụ gây lỗi nếu không có ngoặc:

```bash
tap_tin="bao cao thang 10.txt"
ls $tap_tin
ls "$tap_tin"
```

Dòng đầu shell tách thành ba từ, `ls` sẽ tìm ba file không có thật.
Dòng sau shell giữ nguyên một tên file, chạy đúng.

### 3.4. Các biến đặc biệt có sẵn

```bash
#!/usr/bin/env bash
echo "Tên script: $0"
echo "Tham số thứ nhất: $1"
echo "Tham số thứ hai: $2"
echo "Số tham số: $#"
echo "Tất cả tham số: $@"
echo "Mã thoát của lệnh trước: $?"
echo "Số tiến trình của script này: $$"
```

Chạy thử:

```bash
bash vi-du-bien.sh hello world
```

- `$0`: tên script.
- `$1`, `$2`: tham số thứ nhất, thứ hai. Ngoài `$9`, muốn lấy tham số
  thứ mười trở đi phải viết `${10}`, không viết `$10` vì shell sẽ hiểu
  là `$1` rồi thêm chữ `0`.
- `$#`: đếm có bao nhiêu tham số.
- `$@`: tất cả tham số, mỗi tham số giữ nguyên vẹn khi viết `"$@"`.
- `$?`: mã thoát của lệnh vừa chạy xong, 0 là thành công, khác 0 là có
  vấn đề. Phải đọc ngay sau lệnh đó, vì bất kỳ lệnh nào chạy sau, kể cả
  `echo`, cũng làm `$?` đổi thành mã thoát của chính nó.
- `$$`: số tiến trình, hay dùng để tạo tên file tạm không trùng nhau.

### 3.5. export, biến môi trường và biến thường

```bash
ten="Hoàng"
export ten
bash -c 'echo "$ten"'
```

- Biến thường chỉ sống trong shell hiện tại.
- `export ten` biến nó thành biến môi trường, chương trình con chạy từ
  shell này cũng nhìn thấy. Dùng khi muốn truyền cấu hình xuống chương
  trình con, ví dụ `export EDITOR=nano`.
- Biến trong script con không truyền ngược lên shell cha, trừ khi dùng
  `source`.

### 3.6. Biến chỉ đọc và xóa biến

```bash
readonly PHIEN_BAN="1.0"
unset ten
```

- `readonly`: gán rồi không sửa được nữa, dùng cho hằng số.
- `unset ten`: xóa biến `ten` khỏi shell hiện tại.

Bài tập phần 3: viết script nhận hai tham số là tên và tuổi, in ra câu
chào bằng cả ngoặc kép và ngoặc đơn để tự thấy khác nhau, rồi in ra `$#`,
`$?` sau một lệnh `ls` đúng và một lệnh `ls` sai tên file.

---

## Phần 4. Bảng tra dấu và ký tự đặc biệt

Đừng học bảng này một lần rồi nhớ hết. Học theo cách: gặp dấu nào trong
một dòng thật thì tra đúng hàng đó, rồi quay lại dòng thật và đọc lại.
Bảng này để tra, không để thuộc lòng.

| Dấu | Tên gọi | Nghĩa trong ngữ cảnh thường gặp | Ví dụ |
| --- | --- | --- | --- |
| `#` | Thăng | Đầu dòng là chú thích; trong `${ten#tien_to}` là cắt tiền tố ngắn nhất | `# chú thích` |
| `$` | Đô la | Lấy giá trị biến, tham số, mã thoát, số tiến trình | `$ten`, `$1`, `$?`, `$$` |
| `"` | Ngoặc kép | Cho thay biến và thay lệnh, giữ khoảng trắng | `"Chào $ten"` |
| `'` | Ngoặc đơn | Giữ nguyên văn, không thay gì cả | `'Chào $ten'` |
| `` ` `` | Backtick | Thay lệnh kiểu cũ, nên dùng `$( )` thay cho nó | `` `date` `` |
| `$( )` | Thay lệnh | Chạy lệnh trong ngoặc, lấy đầu ra làm giá trị | `$(date +%Y-%m-%d)` |
| `${ }` | Bao biến | Bao tên biến cho rõ ranh giới, hoặc xử lý chuỗi | `${ten}`, `${10}`, `${ten:-mặc_định}` |
| `*` | Sao | Ký tự đại diện cho mọi chuỗi, dùng với tên file; trong phép nhân là nhân | `ls *.txt`, `$((2 * 3))` |
| `?` | Hỏi | Ký tự đại diện cho đúng một ký tự, dùng với tên file | `ls file?.txt` |
| `[ ]` | Ngoặc vuông | Một là kiểm tra điều kiện; hai là lớp ký tự trong tên file; ba là mảng | `[ -f file ]`, `file[12].txt`, `${mang[0]}` |
| `[[ ]]` | Ngoặc vuông kép | Kiểm tra điều kiện kiểu mới, an toàn hơn `[ ]` | `[[ -f file && $x -gt 0 ]]` |
| `{ }` | Ngoặc nhọn | Một là khối lệnh; hai là mở rộng chuỗi; ba là mảng và biến | `{a,b}.txt`, `${mang[@]}`, `{ echo a; echo b; }` |
| `( )` | Ngoặc tròn | Một là chạy lệnh trong shell con; hai là định nghĩa hàm; ba là mảng | `(cd /tmp; ls)`, `ten_ham() { ...; }`, `mang=(a b c)` |
| `;` | Chấm phẩy | Ngăn cách hai lệnh trên cùng một dòng, chạy lần lượt | `cd /tmp; ls` |
| `;;` | Hai chấm phẩy | Kết thúc một nhánh trong `case` | `1) echo một ;;` |
| `:` | Hai chấm | Một là lệnh không làm gì, luôn thành công; hai là ngăn cách trong `PATH`, trong `case` không có ý này | `:`, `PATH=/usr/bin:/bin` |
| `|` | Ống | Đưa đầu ra của lệnh trước làm đầu vào của lệnh sau | `ls \| grep txt` |
| `>` | Lớn hơn | Ghi đầu ra ra file, ghi đè file cũ | `ls > danh-sach.txt` |
| `>>` | Hai lớn hơn | Ghi đầu ra ra file, nối thêm vào cuối file | `echo thêm >> danh-sach.txt` |
| `<` | Nhỏ hơn | Lấy đầu vào từ file thay vì bàn phím | `while read dong < file` |
| `2>` | Hai lớn hơn | Chuyển hướng thông báo lỗi ra file | `ls sai 2> loi.txt` |
| `2>&1` | Hai và một | Gộp thông báo lỗi vào cùng chỗ với đầu ra thường | `ls > tat-ca.txt 2>&1` |
| `&` | Và | Một là chạy lệnh nền; hai là `&&` nghĩa là "và, nếu lệnh trước thành công mới chạy tiếp" | `sleep 10 &`, `ls && echo ok` |
| `&&` | Và và | Chạy lệnh sau chỉ khi lệnh trước thành công | `cd /tmp && ls` |
| `||` | Hoặc hoặc | Chạy lệnh sau chỉ khi lệnh trước thất bại | `ls file || echo "không có file"` |
| `!` | Cảm thán | Phủ định điều kiện; trong lịch sử lệnh là gọi lại lệnh cũ | `if ! [ -f file ]; then`, `!!` |
| `=` | Bằng | Gán biến, hoặc so sánh chuỗi trong `[ ]` và `[[ ]]` | `ten="Hoàng"`, `[ "$a" = "$b" ]` |
| `==` | Hai bằng | So sánh chuỗi trong `[[ ]]`, hoặc so sánh trong awk | `[[ "$a" == "Hoàng" ]]` |
| `-` | Gạch ngang | Tùy chọn của lệnh; `-` một mình là đầu vào chuẩn; trong `cd -` là thư mục trước | `ls -l`, `cd -` |
| `--` | Hai gạch | Báo hết tùy chọn, phần sau là tham số dù trông giống tùy chọn | `rm -- -file-la.txt` |
| `/` | Gạch chéo | Ngăn cách thư mục trong đường dẫn; trong phép chia là chia | `/tmp/tai-lieu`, `$((10 / 2))` |
| `\` | Gạch chéo ngược | Thoát ký tự đặc biệt, hoặc nối dòng | `echo "a\$b"`, `lệnh dài \` |
| `~` | Ngã | Thư mục home | `cd ~` |
| `,` | Phẩy | Ngăn cách trong mở rộng ngoặc nhọn | `{a,b,c}.txt` |
| `%` | Phần trăm | Trong mở rộng biến là cắt hậu tố; trong phép toán là chia lấy dư | `${ten%.*}`, `$((10 % 3))` |
| `@` | A còng | Trong mảng là tất cả phần tử; trong tham số là tất cả tham số | `"${mang[@]}"`, `"$@"` |

### 4.1. Ba ngữ cảnh của cùng một dấu, lấy `$` làm ví dụ

- `$ten`: lấy giá trị biến `ten`.
- `$1`: tham số thứ nhất, không phải biến tên `1`.
- `$$`: số tiến trình, không phải hai lần lấy biến.
- `$((1 + 2))`: phép toán số học, kết quả là 3.
- `$(date)`: chạy lệnh `date`, lấy đầu ra.

Cùng một dấu, khác ngữ cảnh thì khác nghĩa. Vì vậy khi bóc một dòng, luôn
hỏi: dấu này đang đứng ở đâu, trước nó là gì, sau nó là gì.

### 4.2. Ký tự đại diện với tên file, gọi là glob

```bash
ls *.txt
ls file?.txt
ls file[12].txt
ls file[!12].txt
```

- `*.txt`: mọi file kết thúc bằng `.txt`, kể cả tên dài bao nhiêu cũng được.
- `file?.txt`: `?` là đúng một ký tự, nên khớp `file1.txt`, `fileA.txt`,
  nhưng không khớp `file12.txt` vì đó là hai ký tự.
- `file[12].txt`: chỉ khớp `file1.txt` và `file2.txt`.
- `file[!12].txt`: khớp mọi ký tự trừ `1` và `2`.

Glob do shell tự mở rộng trước khi lệnh chạy. Nếu không có file nào khớp,
tùy shell mà glob giữ nguyên chữ hoặc báo lỗi, nên khi viết script, đừng
dựa vào glob khi thư mục có thể rỗng mà không kiểm tra trước.

Bài tập phần 4: tạo 5 file `file1.txt`, `file2.txt`, `file12.txt`,
`fileA.txt`, `bao-cao.txt`, chạy 4 lệnh glob trên, ghi lại file nào khớp,
file nào không, và giải thích vì sao.

---

## Phần 5. Kiểm tra điều kiện: test, `[ ]`, `[[ ]]`

### 5.1. Khi nào cần kiểm tra

Trước khi làm gì đó có hậu quả, kiểm tra trước: file có tồn tại không,
thư mục có phải là thư mục không, file có quyền đọc không, chuỗi có rỗng
không, số này có lớn hơn số kia không. Kiểm tra trước giúp script không
chạy mù, không xóa nhầm, không báo thành công giả.

### 5.2. Ba cách viết kiểm tra

```bash
test -f file.txt
[ -f file.txt ]
[[ -f file.txt ]]
```

Ba dòng trên làm cùng một việc: kiểm tra `file.txt` có phải là file
thường không. Cách `[ ]` là cách cũ, tương thích rộng, nhưng phải có
khoảng trắng sau `[` và trước `]`, và phải đặt biến trong ngoặc kép.
Cách `[[ ]]` là cách của bash, an toàn hơn với biến rỗng và ký tự đặc
biệt, và dùng được `&&`, `||` bên trong. Trong giáo trình này, khi viết
cho bash thì ưu tiên `[[ ]]`, khi cần tương thích `sh` thì dùng `[ ]`.

### 5.3. Các phép kiểm tra file hay dùng nhất

| Phép kiểm tra | Nghĩa | Ví dụ |
| --- | --- | --- |
| `-e` | Tồn tại, bất kể là file hay thư mục | `[ -e duong_dan ]` |
| `-f` | Là file thường | `[ -f tap_tin.txt ]` |
| `-d` | Là thư mục | `[ -d thu_muc ]` |
| `-r` | Có quyền đọc | `[ -r tap_tin.txt ]` |
| `-w` | Có quyền ghi | `[ -w tap_tin.txt ]` |
| `-x` | Có quyền chạy | `[ -x script.sh ]` |
| `-s` | Tồn tại và kích thước lớn hơn 0 | `[ -s tap_tin.txt ]` |
| `-L` | Là liên kết mềm | `[ -L lien-ket ]` |

### 5.4. Kiểm tra chuỗi và số

```bash
ten=""
if [ -z "$ten" ]; then echo "Tên rỗng"; fi
if [ -n "$ten" ]; then echo "Tên không rỗng"; fi
if [ "$a" = "$b" ]; then echo "Hai chuỗi bằng nhau"; fi
if [ "$a" != "$b" ]; then echo "Hai chuỗi khác nhau"; fi

so=10
if [ "$so" -eq 10 ]; then echo "Bằng 10"; fi
if [ "$so" -gt 5 ]; then echo "Lớn hơn 5"; fi
if [ "$so" -lt 20 ]; then echo "Nhỏ hơn 20"; fi
```

- `-z`: chuỗi rỗng, nhớ là z như "zero", độ dài bằng 0.
- `-n`: chuỗi không rỗng.
- So sánh chuỗi dùng `=`, `!=`.
- So sánh số dùng `-eq` (bằng), `-ne` (khác), `-lt` (nhỏ hơn),
  `-le` (nhỏ hơn hoặc bằng), `-gt` (lớn hơn), `-ge` (lớn hơn hoặc bằng).
  Không dùng `>` `<` trong `[ ]` để so sánh số, vì trong `[ ]` chúng là
  chuyển hướng, không phải so sánh.

Trong `[[ ]]`, so sánh số vẫn dùng `-eq`, `-gt`, còn so sánh chuỗi có
thể dùng `==`, và có thể dùng mẫu: `[[ "$ten" == H* ]]` nghĩa là tên
bắt đầu bằng H.

### 5.5. Kết hợp điều kiện

```bash
if [[ -f file.txt && -r file.txt ]]; then
  echo "File tồn tại và đọc được"
fi
if [[ -d thu_muc || -f thu_muc ]]; then
  echo "Đường dẫn tồn tại"
fi
if ! [[ -f file.txt ]]; then
  echo "File không tồn tại"
fi
```

- `&&`: và, cả hai đều đúng mới đúng.
- `||`: hoặc, một trong hai đúng là đúng.
- `!`: phủ định, đúng thành sai, sai thành đúng.

Bài tập phần 5: viết script kiểm tra một đường dẫn do người dùng nhập:
nếu là thư mục thì in "Đây là thư mục", nếu là file thì in "Đây là file",
nếu không tồn tại thì in "Không tồn tại", nếu tồn tại nhưng không đọc
được thì in thêm cảnh báo.

---

## Phần 6. Rẽ nhánh: if và case

### 6.1. if: khi nào dùng

Dùng `if` khi điều kiện là đúng hoặc sai, hoặc khi có nhiều điều kiện
xếp tầng. Cấu trúc đầy đủ:

```bash
if [ điều_kiện_1 ]; then
  # làm việc 1
elif [ điều_kiện_2 ]; then
  # làm việc 2
else
  # làm việc còn lại
fi
```

Dịch từng từ:

- `if` = nếu.
- `then` = thì.
- `elif` = nếu không mà, tức là "else if" viết gọn, chỉ chạy khi `if`
  trước đó sai và điều kiện của chính nó đúng.
- `else` = nếu không, không có điều kiện riêng, chạy khi mọi điều kiện
  trên đều sai. `else` có thể không có, nhưng `fi` bắt buộc phải có.
- `fi` = từ `if` viết ngược lại, nghĩa là "hết if". Đây là cách shell
  đóng khối, giống như đóng ngoặc. `fi` không phải "fail", không phải
  "lỗi".

Ví dụ thật:

```bash
#!/usr/bin/env bash
tap_tin="$1"
if [[ -f "$tap_tin" ]]; then
  echo "Đây là file thường."
elif [[ -d "$tap_tin" ]]; then
  echo "Đây là thư mục."
else
  echo "Không phải file thường, cũng không phải thư mục."
fi
```

### 6.2. case: khi nào dùng

Dùng `case` khi một giá trị có nhiều trường hợp rời rạc, ví dụ tham số
dòng lệnh là `--help`, `--save`, `--check`, hoặc menu chọn 1, 2, 3.
Dùng `case` thì gọn và dễ đọc hơn một chuỗi `if` dài.

```bash
#!/usr/bin/env bash
lua_chon="$1"
case "$lua_chon" in
  1)
    echo "Bạn chọn kiểm tra hệ thống."
    ;;
  2)
    echo "Bạn chọn kiểm tra mạng."
    ;;
  0)
    echo "Tạm biệt."
    ;;
  *)
    echo "Lựa chọn không rõ: $lua_chon"
    ;;
esac
```

Bóc từng phần:

- `case "$lua_chon" in`: xét giá trị của biến `lua_chon`.
- `1)`: trường hợp giá trị là 1, dấu `)` kết thúc mẫu, không phải đóng
  ngoặc của hàm.
- `;;`: kết thúc nhánh này, không chạy sang nhánh sau. Quên `;;` là lỗi
  hay gặp, shell sẽ báo lỗi cú pháp.
- `*)`: trường hợp còn lại, `*` ở đây là "mọi giá trị khác", dùng làm
  nhánh mặc định, luôn đặt cuối cùng.
- `esac`: từ `case` viết ngược lại, nghĩa là "hết case".

`case` cũng dùng được mẫu: `*.txt)` khớp mọi file `.txt`, `start|stop)`
khớp cả hai từ `start` và `stop`, `[0-9])` khớp một chữ số.

### 6.3. So sánh if và case

- Điều kiện đúng sai, hoặc điều kiện là một phép so sánh số, chuỗi phức
  tạp: dùng `if`.
- Một giá trị, nhiều trường hợp rời rạc, đặc biệt là tham số và menu:
  dùng `case`.
- Không dùng `if` lồng nhau quá 3 tầng, vì khó đọc. Nếu thấy mình lồng
  quá sâu, tách phần trong thành hàm, hoặc chuyển sang `case`.

Bài tập phần 6: viết một menu có 4 lựa chọn bằng `case`: 1 là in thư mục
hiện tại, 2 là in dung lượng đĩa, 3 là in ngày giờ, 0 là thoát, còn lại
là báo lựa chọn không rõ và hỏi lại.

---

## Phần 7. Vòng lặp: for, while, until

### 7.1. for: lặp qua một danh sách đã biết trước

```bash
for tap_tin in *.txt; do
  echo "Đang xử lý: $tap_tin"
done
```

- `for tap_tin in ...`: với mỗi giá trị trong danh sách, gán vào biến
  `tap_tin`, rồi chạy phần thân.
- `do`: bắt đầu phần thân vòng lặp.
- `done`: hết vòng lặp. `done` chỉ đóng `for` và `while`, không đóng `if`.

Các dạng `for` hay dùng:

```bash
for i in 1 2 3 4 5; do echo "$i"; done
for i in $(seq 1 10); do echo "$i"; done
for ((i = 1; i <= 10; i++)); do echo "$i"; done
for tap_tin in /tmp/*.log; do echo "$tap_tin"; done
```

- `seq 1 10`: tạo ra dãy số từ 1 đến 10.
- `for ((...))`: vòng lặp kiểu C, dùng khi cần đếm số, tăng dần, có điều
  kiện dừng rõ ràng bằng số.

### 7.2. while: lặp khi điều kiện còn đúng

```bash
so=1
while [[ "$so" -le 5 ]]; do
  echo "$so"
  so=$((so + 1))
done
```

- `while` = trong khi.
- Điều kiện kiểm tra trước mỗi lần lặp. Nếu lần đầu đã sai, thân vòng
  lặp không chạy lần nào.
- Trong thân phải có dòng làm điều kiện đổi dần về sai, nếu không vòng
  lặp chạy mãi không dừng. Trong ví dụ trên, `so=$((so + 1))` làm việc đó.

### 7.3. Đọc file từng dòng, cách chuẩn

```bash
while IFS= read -r dong; do
  echo "Dòng: $dong"
done < danh-sach.txt
```

Bóc từng phần, vì dòng này hay làm người mới rối:

- `while ...; do ...; done < danh-sach.txt`: phần `< danh-sach.txt` ở
  cuối là lấy đầu vào của cả vòng lặp từ file, không phải từ bàn phím.
- `IFS=`: đặt biến ngăn cách trường thành rỗng, để khoảng trắng đầu và
  cuối dòng không bị cắt bỏ.
- `read -r dong`: đọc một dòng vào biến `dong`, `-r` là đọc nguyên,
  không xử lý ký tự `\` theo cách đặc biệt.
- Cả cụm `IFS= read -r dong` trong thực tế có tác dụng là "đọc nguyên
  dòng", giữ nguyên dòng như trong file. Nhớ cụm này như một cụm, khỏi
  tách từng chữ ra học rời.

### 7.4. until và điều khiển vòng lặp

```bash
so=1
until [[ "$so" -gt 5 ]]; do
  echo "$so"
  so=$((so + 1))
done
```

- `until` = cho đến khi, ngược với `while`: lặp khi điều kiện còn sai,
  dừng khi điều kiện thành đúng.
- `break`: thoát hẳn khỏi vòng lặp ngay lập tức.
- `continue`: bỏ qua phần còn lại của lần lặp này, sang lần lặp sau.

```bash
for i in 1 2 3 4 5; do
  if [[ "$i" -eq 3 ]]; then
    continue
  fi
  echo "$i"
done
```

Ví dụ trên in ra 1, 2, 4, 5, bỏ qua 3.

Bài tập phần 7: viết script đọc file `danh-sach.txt` có mỗi dòng là một
tên file, kiểm tra từng file có tồn tại không, in ra "Có" hoặc "Không
có" kèm tên file, đếm tổng số file có và không có ở cuối.

---

## Phần 8. Hàm

### 8.1. Khi nào cần dùng hàm

Khi một việc lặp lại từ hai lần trở lên, hoặc khi một đoạn code dài cần
đặt tên cho dễ đọc, thì tách thành hàm. Hàm giúp script ngắn lại, sửa một
chỗ là sửa cho mọi chỗ dùng nó, và giúp mình đọc script theo tên việc,
không phải đọc từng dòng lệnh vụn.

### 8.2. Định nghĩa và gọi hàm

```bash
#!/usr/bin/env bash
chao() {
  echo "Chào $1"
}
chao "Hoàng"
chao "Lan"
```

- `chao() { ...; }`: định nghĩa hàm tên `chao`. Dấu `()` ở đây chỉ báo
  đây là định nghĩa hàm, không phải gọi hàm, và không ghi tham số vào
  trong đó như nhiều ngôn ngữ khác.
- Gọi hàm thì chỉ viết tên, không có `()` và không có từ khóa gì thêm:
  `chao "Hoàng"`.
- `$1` bên trong hàm là tham số của hàm, không phải tham số của script.
  Script có tham số riêng, hàm có tham số riêng, hai bộ này độc lập.

### 8.3. Trả về giá trị và mã thoát

```bash
kiem_tra_file() {
  if [[ -f "$1" ]]; then
    return 0
  else
    return 1
  fi
}
if kiem_tra_file "tap-tin.txt"; then
  echo "File có thật"
else
  echo "File không có"
fi
```

- `return 0`: hàm thành công, `return 1`: hàm thất bại. `return` chỉ trả
  về mã thoát, là số từ 0 đến 255, không trả về chuỗi.
- Muốn hàm trả về chuỗi thì in chuỗi ra bằng `echo`, rồi bên ngoài bắt
  bằng `$( )`:

```bash
lay_ngay() {
  date +%Y-%m-%d
}
ngay_hom_nay=$(lay_ngay)
echo "$ngay_hom_nay"
```

### 8.4. Biến cục bộ

```bash
dem() {
  local so_dem=0
  so_dem=$((so_dem + 1))
  echo "$so_dem"
}
```

- `local so_dem=0`: biến chỉ sống trong hàm, ra khỏi hàm là mất, không
  làm bẩn biến cùng tên ở ngoài. Dùng `local` cho mọi biến chỉ dùng tạm
  trong hàm, trừ biến cố ý muốn trả ra ngoài.

### 8.5. Hàm usage, hàm in trợ giúp

Mọi script có tham số nên có một hàm `usage` in cách dùng:

```bash
usage() {
  echo "Cách dùng: $0 --tap-tin FILE"
  echo "  --tap-tin FILE: file cần kiểm tra"
}
```

Tên `usage` là tên do mình tự đặt, không phải từ khóa của shell. Gọi nó
khi người dùng gõ `--help`, hoặc khi tham số sai, hoặc khi thiếu tham số
bắt buộc. Đây là thói quen của script viết cho người khác dùng, và cho
chính mình một tháng sau dùng lại.

Bài tập phần 8: tách bài tập phần 7 thành hai hàm: một hàm kiểm tra một
file, một hàm in kết quả tổng kết, rồi viết phần chính chỉ gọi hai hàm đó.

---

## Phần 9. Xử lý tham số dòng lệnh

### 9.1. Mẫu chuẩn: while, case, shift

Đây là mẫu dùng cho hầu hết script trong giáo trình này, và cho các
script giám sát, sao lưu, kiểm tra mà Hoàng đã viết trước đây.

```bash
#!/usr/bin/env bash
usage() {
  echo "Cách dùng: $0 [--tap-tin FILE] [--help]"
}
tap_tin=""
while [[ $# -gt 0 ]]; do
  case "$1" in
    --help|-h)
      usage
      exit 0
      ;;
    --tap-tin)
      shift
      if [[ $# -eq 0 ]]; then
        echo "Thiếu tên file sau --tap-tin."
        usage
        exit 1
      fi
      tap_tin="$1"
      ;;
    --*)
      echo "Tùy chọn không rõ: $1"
      usage
      exit 1
      ;;
    *)
      echo "Tham số không rõ: $1"
      usage
      exit 1
      ;;
  esac
  shift
done
echo "File cần kiểm tra: $tap_tin"
```

Bóc từng phần:

- `while [[ $# -gt 0 ]]`: trong khi còn tham số chưa xử lý thì lặp tiếp.
  `$#` là số tham số còn lại, mỗi lần xử lý xong một tham số thì nó giảm.
- `case "$1"`: xét tham số đầu tiên còn lại.
- `--tap-tin)`: nếu tham số là `--tap-tin`, thì nhánh này chạy.
- `shift`: bỏ tham số đầu tiên đi, tham số thứ hai trở thành tham số đầu
  tiên mới. Trong nhánh `--tap-tin`, `shift` lần đầu là để bỏ chính
  `--tap-tin` đi, rồi lấy giá trị sau nó làm tên file. `shift 2` là bỏ
  hai tham số cùng lúc, dùng cho tùy chọn kèm giá trị.
- `shift` cuối vòng lặp, sau `esac`: bỏ tham số vừa xử lý xong, để vòng
  lặp sang tham số tiếp theo. Quên `shift` này là vòng lặp chạy mãi.
- `--*)`: mẫu bắt mọi tùy chọn bắt đầu bằng `--` mà mình không định nghĩa.
- `*)`: mẫu bắt mọi thứ còn lại.

### 9.2. getopts, cách ngắn cho tùy chọn một chữ

```bash
while getopts "f:h" tuy_chon; do
  case "$tuy_chon" in
    f) tap_tin="$OPTARG" ;;
    h) usage; exit 0 ;;
    *) usage; exit 1 ;;
  esac
done
```

- `getopts "f:h"`: `f` là tùy chọn cần giá trị, dấu `:` sau `f` báo điều
  đó, `h` là tùy chọn không cần giá trị.
- `OPTARG`: giá trị đi kèm tùy chọn vừa đọc được.
- `getopts` gọn hơn cho tùy chọn một chữ như `-f`, `-h`, nhưng không xử
  lý được tùy chọn dài như `--tap-tin`. Vì vậy script cho người dùng
  cuối thường dùng mẫu `while` + `case` + `shift` ở mục 9.1.

### 9.3. Tham số vị trí và tham số còn lại

```bash
nguon="$1"
dich="$2"
shift 2
echo "Còn lại: $@"
```

Sau `shift 2`, `$1` cũ đã mất, `$@` là các tham số còn lại sau hai tham
số đầu. Dùng khi script nhận hai tham số bắt buộc đầu tiên, rồi nhận
thêm một danh sách file tùy ý ở sau.

Bài tập phần 9: viết lại script phần 8, thêm tham số `--tap-tin FILE`
và `--help`, chạy thử với `--help`, với `--tap-tin` mà không kèm tên file,
với một tùy chọn lạ, ghi lại thông báo trong từng trường hợp.

---

## Phần 10. Vào ra, chuyển hướng, đường ống

### 10.1. Ba luồng vào ra chuẩn

- `0`, stdin: đầu vào, mặc định là bàn phím.
- `1`, stdout: đầu ra thường, mặc định là màn hình.
- `2`, stderr: đầu ra lỗi, mặc định cũng là màn hình, nhưng là một luồng
  riêng, nên có thể tách ra file riêng.

```bash
ls /tmp > danh-sach.txt
ls /thu-muc-khong-co 2> loi.txt
ls /tmp /thu-muc-khong-co > tat-ca.txt 2>&1
```

- `>`: ghi đè file, file cũ mất nội dung. Dùng khi muốn tạo file mới
  hoặc thay toàn bộ.
- `>>`: nối thêm vào cuối file, giữ nội dung cũ.
- `2>`: chỉ chuyển hướng lỗi.
- `2>&1`: gộp lỗi vào cùng chỗ với đầu ra thường. Thứ tự quan trọng:
  viết `> file 2>&1` là đúng, viết `2>&1 > file` là sai ý, vì lúc gộp
  thì đầu ra thường vẫn còn đang chỉ ra màn hình.

### 10.2. Đường ống, pipe

```bash
ls /tmp | grep txt
cat log.txt | sort | uniq -c | sort -nr | head -10
```

- `|`: lấy đầu ra của lệnh trước, đưa làm đầu vào của lệnh sau.
- Chuỗi pipe trên làm một việc rất hay gặp: đếm và xếp hạng. `sort` xếp
  các dòng giống nhau kề nhau, `uniq -c` đếm mỗi dòng xuất hiện mấy lần,
  `sort -nr` xếp theo số đếm giảm dần, `head -10` lấy 10 dòng đầu.
- Pipe chỉ chuyển stdout, không chuyển stderr, trừ khi gộp trước bằng
  `2>&1`.

### 10.3. echo, printf và read

```bash
echo "Chào Hoàng"
printf "Tên: %s, tuổi: %s\n" "Hoàng" "20"
read -p "Nhập tên của bạn: " ten
echo "Chào $ten"
```

- `echo`: in ra, tự thêm xuống dòng ở cuối. Đủ cho hầu hết thông báo.
  `echo` trong shell giống `print()` trong Python, chỉ khác là shell
  không cần dấu ngoặc đơn.
- `printf`: in theo định dạng, `%s` là chuỗi, `%d` là số, `\n` là xuống
  dòng. Dùng khi cần căn cột, căn số, in bảng cho đẹp.
- `read -p "Nhắc: " bien`: in lời nhắc rồi đọc một dòng từ bàn phím
  vào biến. `read -s`: đọc mà không hiện chữ lên màn hình, dùng cho
  mật khẩu, nhưng trong script thật thì không nên đọc mật khẩu rồi lưu
  lung tung, xem Phần 18.

### 10.4. tee và /dev/null

```bash
ls /tmp | tee danh-sach.txt
ls /tmp > /dev/null
```

- `tee`: vừa in ra màn hình, vừa ghi vào file. Dùng khi muốn xem và lưu
  cùng lúc.
- `/dev/null`: hố đen, ghi gì vào đó cũng mất, dùng khi cố ý bỏ đầu ra
  không cần xem, ví dụ `lệnh > /dev/null 2>&1`.

Bài tập phần 10: tạo file log giả có 20 dòng, trong đó có lặp lại một số
IP, dùng pipe `sort | uniq -c | sort -nr` để tìm IP xuất hiện nhiều nhất,
ghi kết quả vừa ra màn hình vừa ra file bằng `tee`.

---

## Phần 11. Xử lý văn bản: grep, sed, awk, cut, sort

Nguyên tắc phân lớp, rất quan trọng để khỏi rối: `grep`, `sed`, `awk`,
`cut`, `sort` không phải từ khóa của shell, chúng là chương trình ngoài,
shell chỉ gọi chúng và nhận kết quả. Học chúng như học cách dùng một cái
công cụ, không phải học cú pháp shell.

### 11.1. grep: tìm dòng chứa mẫu

```bash
grep "Failed" auth.log
grep -i "failed" auth.log
grep -v "Failed" auth.log
grep -c "Failed" auth.log
grep -n "Failed" auth.log
grep -q "Failed" auth.log && echo "Có dòng lỗi"
```

- Không tùy chọn: in ra các dòng chứa đúng mẫu.
- `-i`: không phân biệt hoa thường.
- `-v`: in ra các dòng không chứa mẫu, tức là đảo ngược.
- `-c`: chỉ đếm số dòng khớp, không in dòng.
- `-n`: in kèm số dòng.
- `-q`: im lặng, chỉ trả mã thoát 0 nếu có dòng khớp, 1 nếu không.
  Dùng `-q` trong `if`, vì lúc đó mình chỉ cần biết có hay không, không
  cần xem dòng.

### 11.2. cut, sort, uniq, wc, head, tail

```bash
cut -d: -f1 /etc/passwd
sort ten.txt
sort -n so.txt
sort -nr so.txt
uniq -c ten-da-sort.txt
wc -l log.txt
head -20 log.txt
tail -20 log.txt
tail -f log.txt
```

- `cut -d: -f1`: cắt mỗi dòng theo dấu `:` và lấy cột 1. `-d` là dấu
  ngăn cách, `-f` là cột cần lấy.
- `sort`: xếp dòng theo thứ tự chữ cái. `sort -n`: xếp theo số.
  `sort -nr`: xếp theo số giảm dần.
- `uniq -c`: đếm dòng trùng nhau, nhưng chỉ đếm được khi các dòng giống
  nhau đã kề nhau, nên luôn `sort` trước khi `uniq`.
- `wc -l`: đếm số dòng. `wc -w`: đếm số từ. `wc -c`: đếm số byte.
- `head -20`: 20 dòng đầu. `tail -20`: 20 dòng cuối.
- `tail -f`: theo dõi file trực tiếp, dòng mới ghi vào là hiện ra ngay,
  dùng để xem log đang chạy, bấm `Ctrl + C` để dừng.

### 11.3. awk: lấy cột, tính toán, lọc theo điều kiện

`awk` tự chia mỗi dòng thành các cột, gọi là trường, đánh số từ `$1`,
`$2`, ..., `$NF` là cột cuối cùng, `NF` là số cột của dòng đó, `$0` là
cả dòng.

Ví dụ file `log-mau.txt`:

```
Failed password for root from 192.168.1.10
Failed password for hoang from 192.168.1.10
Accepted password for hoang from 192.168.1.5
```

```bash
awk '{ print $4 }' log-mau.txt
awk '{ print $6 }' log-mau.txt
awk '/Failed/ { print $6 }' log-mau.txt
awk '{ print $6 }' log-mau.txt | sort | uniq -c | sort -nr
```

- `print $4`: in cột 4, là tên người dùng trong ví dụ này.
- `print $6`: in cột 6, là IP trong ví dụ này.
- `/Failed/ { ... }`: chỉ làm việc với dòng chứa `Failed`.
- Chuỗi pipe cuối: lấy IP của mọi dòng, đếm, xếp hạng, IP nào đăng nhập
  thất bại nhiều nhất lên đầu.

Khi nào dùng `awk`:

- Cần lấy một cột cố định trong dòng có cấu trúc đều: dùng `awk`.
- Cần đếm, cộng, tính trung bình theo cột: dùng `awk`.
- Chỉ cần tìm dòng chứa một từ: dùng `grep`, đừng dùng `awk`.

Đổi dấu ngăn cách:

```bash
awk -F: '{ print $1 }' /etc/passwd
```

`-F:` nghĩa là ngăn cách cột bằng dấu `:`, không phải khoảng trắng mặc
định. Nhớ rằng một số phiên bản `awk` cũ không hiểu biểu thức `{2,}`
kiểu khoảng, nếu gặp lỗi thì viết dài ra thành `[A-Za-z][A-Za-z]+`
thay vì `[A-Za-z]{2,}`.

### 11.4. sed: thay thế và lọc dòng

```bash
sed 's/cũ/mới/' file.txt
sed 's/cũ/mới/g' file.txt
sed -n '1,10p' file.txt
sed '/mẫu/d' file.txt
```

- `s/cũ/mới/`: thay lần đầu tiên xuất hiện trong mỗi dòng.
- Thêm `g` ở cuối: thay mọi lần xuất hiện trong mỗi dòng, `g` là global.
- `-n '1,10p'`: chỉ in dòng 1 đến 10.
- `/mẫu/d`: xóa các dòng chứa mẫu, `d` là delete. Lưu ý `sed` không
  sửa file gốc trừ khi thêm `-i`, và dùng `-i` thì phải sao lưu trước.

Bài tập phần 11: tạo file log giả 20 dòng gồm `Failed` và `Accepted`,
dùng `grep -c` đếm tổng số `Failed`, dùng pipe `grep | awk | sort |
uniq -c | sort -nr` để xếp hạng IP thất bại nhiều nhất, rồi dùng `cut`
hoặc `awk -F` để lấy tên người dùng từ file `/etc/passwd` trên máy mình.

---

## Phần 12. File, thư mục và quyền

### 12.1. Các lệnh file cơ bản

```bash
mkdir -p thu-muc/con
cp file.txt ban-sao.txt
cp -r thu-muc thu-muc-sao
mv cu.txt moi.txt
rm file.txt
rm -r thu-muc
ls -l
```

- `mkdir -p`: tạo thư mục, kể cả thư mục cha chưa có, không báo lỗi nếu
  thư mục đã có.
- `cp -r`: copy cả thư mục, `-r` là recursive, đệ quy.
- `mv`: đổi tên hoặc di chuyển, không phải copy.
- `rm -r`: xóa cả thư mục, nguy hiểm, xem Phần 18 trước khi dùng trong
  script.
- `ls -l`: in chi tiết, gồm quyền, chủ sở hữu, kích thước, ngày sửa.

### 12.2. Đọc quyền từ `ls -l`

```
-rw-r--r-- 1 hoang hoang 1234 Oct 26 10:00 file.txt
drwxr-xr-x 2 hoang hoang 4096 Oct 26 10:00 thu-muc
```

- Ký tự đầu: `-` là file thường, `d` là thư mục, `l` là liên kết mềm.
- 9 ký tự sau chia 3 nhóm, mỗi nhóm 3 ký tự: chủ sở hữu, nhóm, người
  khác. Mỗi nhóm là `r` đọc, `w` ghi, `x` chạy, `-` là không có quyền đó.
- `r` = 4, `w` = 2, `x` = 1, cộng lại thành số: `755` là `rwxr-xr-x`,
  `644` là `rw-r--r--`, `600` là `rw-------`.

```bash
chmod +x script.sh
chmod 755 script.sh
chmod 644 file.txt
chown hoang:hoang file.txt
```

- `chmod +x`: thêm quyền chạy cho mọi người.
- `chmod 755`: đặt quyền chính xác, dùng khi muốn quyền đúng tuyệt đối.
- `chown`: đổi chủ sở hữu, thường cần quyền cao hơn.

### 12.3. find: tìm file theo điều kiện

```bash
find /tmp -name "*.log"
find /tmp -type f -mtime -7
find /tmp -type f -size +10M
find . -type f -newermt "2026-10-26 02:00" ! -newermt "2026-10-26 03:00"
```

- `-name "*.log"`: tên khớp mẫu, nhớ đặt mẫu trong ngoặc kép để shell
  không mở rộng glob trước.
- `-type f`: chỉ file thường, `-type d`: chỉ thư mục.
- `-mtime -7`: sửa trong 7 ngày qua.
- `-size +10M`: lớn hơn 10 megabyte.
- `-newermt`: sửa sau một thời điểm, kết hợp hai điều kiện để lấy file
  trong một cửa sổ thời gian.

### 12.4. Nén và giải nén

```bash
tar -czf sao-luu.tar.gz thu-muc
tar -xzf sao-luu.tar.gz
gzip file.txt
gunzip file.txt.gz
zip -r sao-luu.zip thu-muc
unzip sao-luu.zip
```

- `tar -czf`: `c` là tạo, `z` là nén bằng gzip, `f` là tên file đích.
- `tar -xzf`: `x` là giải nén.
- Nén xong luôn kiểm tra file đích tồn tại và lớn hơn 0, rồi mới coi
  là sao lưu thành công.

Bài tập phần 12: tạo một thư mục có 3 file, sao lưu bằng `tar -czf`
theo tên có ngày hôm nay, kiểm tra bằng `[ -s file ]`, giải nén ra một
thư mục khác và so sánh bằng `diff -r`.

---

## Phần 13. Tiến trình, hệ thống, chạy định kỳ

### 13.1. Xem và quản lý tiến trình

```bash
ps aux
ps aux | grep ten-tien-trinh
top
htop
kill 1234
kill -9 1234
jobs
fg
bg
```

- `ps aux`: liệt kê mọi tiến trình, gồm PID, người chạy, CPU, bộ nhớ,
  lệnh.
- `top`, `htop`: xem trực tiếp, cập nhật liên tục, `htop` dễ nhìn hơn
  nếu máy đã cài.
- `kill 1234`: gửi tín hiệu dừng nhẹ nhàng tới PID 1234, cho tiến trình
  cơ hội dọn dẹp rồi thoát.
- `kill -9 1234`: ép dừng ngay, không cho dọn dẹp, chỉ dùng khi `kill`
  thường không được.
- Script giám sát không bao giờ tự `kill` tiến trình lạ chỉ vì thấy lạ,
  nó chỉ báo cáo. Giết nhầm tiến trình hệ thống có thể làm máy lỗi.

### 13.2. Chạy nền và tách khỏi terminal

```bash
sleep 30 &
nohup ./script-dai.sh > log.txt 2>&1 &
disown
```

- `&` ở cuối: chạy nền, shell trả dấu nhắc ngay, không chờ lệnh xong.
- `nohup`: lệnh vẫn chạy khi đóng terminal, đầu ra ghi vào file.
- `jobs`: xem các việc nền của shell hiện tại.
- `fg`: đưa việc nền lên chạy trước mặt. `bg`: cho việc đang dừng chạy
  tiếp ở nền.

### 13.3. Tài nguyên hệ thống

```bash
df -h
du -sh /tmp/*
free -h
uptime
uname -a
cat /proc/meminfo
cat /proc/cpuinfo | head -20
```

- `df -h`: dung lượng đĩa còn trống, `-h` là in dạng người đọc được,
  ví dụ `10G` thay vì một dãy số byte.
- `du -sh`: dung lượng một thư mục chiếm, `-s` là tổng, `-h` là dạng đọc
  được.
- `free -h`: bộ nhớ RAM còn trống.
- `uptime`: máy chạy được bao lâu, tải trung bình.
- `uname -a`: thông tin hệ điều hành và nhân.

### 13.4. Chạy định kỳ bằng cron

```bash
crontab -e
crontab -l
```

Một dòng cron có 5 trường thời gian, rồi đến lệnh:

```
phút giờ ngày-trong-tháng tháng ngày-trong-tuần lệnh
0 6 * * * /home/hoang/sao-luu.sh >> /home/hoang/sao-luu.log 2>&1
```

- Ví dụ trên chạy lúc 6 giờ 0 phút mỗi ngày.
- `*` là mọi giá trị.
- Trong cron, đường dẫn phải ghi tuyệt đối, vì cron không có cùng môi
  trường và thư mục làm việc như khi mình gõ tay.
- Script chạy bằng cron nên tự ghi log, vì mình không ngồi xem màn hình
  lúc nó chạy.

Bài tập phần 13: viết script in ngày giờ, dung lượng đĩa và bộ nhớ vào
một file log, chạy bằng tay một lần, rồi đặt cron chạy mỗi giờ một lần
trong ngày hôm nay để thử, sau đó xóa dòng cron thử đó đi.

---

## Phần 14. Mạng trong shell, chỉ trên máy và mạng của mình

Nguyên tắc trước khi học phần này: chỉ dùng các lệnh dưới đây trên máy
của mình, mạng nhà mình, hoặc máy lab mình được phép. Không quét, không
bắt gói tin, không thử đăng nhập vào máy, mạng của người khác.

### 14.1. Xem cấu hình mạng của chính mình

```bash
ip a
ip route
ip neigh
ss -tn
ss -tnlp
ss -tnup
ping -c 4 8.8.8.8
```

- `ip a`: địa chỉ IP của các card mạng trên máy mình.
- `ip route`: máy mình đi ra Internet bằng cổng nào.
- `ip neigh`: bảng hàng xóm, tức các máy đã nói chuyện gần đây trong
  mạng LAN nhà mình, gồm IP và địa chỉ MAC.
- `ss -tn`: kết nối TCP, `-t` là TCP, `-n` là không phân giải tên, in IP
  dạng số cho nhanh và rõ.
- `ss -tnlp`: thêm `-l` là chỉ cổng đang nghe, `-p` là kèm tiến trình
  nào đang giữ cổng đó.
- `ss -tnup`: thêm `-u` là UDP, `-p` là kèm tiến trình.
- `ping -c 4`: gửi 4 gói ping, `-c` là số gói, không ghi `-c` thì ping
  chạy mãi cho đến khi bấm `Ctrl + C`.

Trên Termux và Linux, `ip` là công cụ chính, thuộc bộ `iproute2`.
Trên Windows, lệnh tương đương là `ipconfig`, còn `traceroute` trên
Linux tương đương `tracert` trên Windows.

### 14.2. Tải file và gọi API đơn giản

```bash
curl -I https://example.com
curl -o file.html https://example.com
wget https://example.com/file.zip
curl -s https://api.example.com/data | head -20
```

- `curl -I`: chỉ lấy tiêu đề phản hồi, không tải thân trang.
- `curl -o`: tải về và đặt tên file đích.
- `wget`: tải file, mặc định giữ tên gốc.
- `curl -s`: im lặng, không in tiến trình tải, tiện khi đưa vào pipe.

### 14.3. Chẩn đoán đường đi và đo tốc độ

```bash
traceroute 8.8.8.8
mtr 8.8.8.8
```

- `traceroute`: in từng chặng đường gói tin đi qua tới đích.
- `mtr`: kết hợp ping và traceroute, cập nhật liên tục, xem chặng nào
  mất gói nhiều nhất.
- Trên Termux, `traceroute` có thể cần `pkg install traceroute`, tương
  tự `whois` cần `pkg install whois`.

### 14.4. Bắt gói tin, ở mức hiểu, không lạm dụng

```bash
tcpdump -i eth0 -c 100 -w capture.pcap
tcpdump -r capture.pcap -nn -q | head -20
```

- Bắt gói tin cần quyền root và cần `tcpdump`, trên Termux cài bằng
  `pkg install tcpdump`, trên Linux cài bằng gói của hệ thống.
- `-c 100`: chỉ bắt 100 gói rồi dừng, tránh đầy ổ đĩa.
- `-w`: ghi ra file pcap, không in ra màn hình.
- `-r`: đọc lại file pcap đã bắt.
- Bắt gói tin trên máy mình để học và để giám sát máy mình là việc chính
  đáng. Bắt gói tin của người khác, trên mạng không thuộc về mình, là
  việc không làm.

Bài tập phần 14: chạy `ip a`, `ip route`, `ss -tnlp` trên máy mình, ghi
lại card mạng, cổng mặc định, và các cổng đang nghe kèm tiến trình giữ
chúng, rồi so sánh với lần chạy hôm sau để xem có cổng mới nào không.

---

## Phần 15. Lỗi, mã thoát và gỡ lỗi

### 15.1. Mã thoát

```bash
ls /tmp
echo $?
ls /thu-muc-khong-co
echo $?
```

- Mã thoát 0: thành công.
- Mã thoát khác 0: có vấn đề. Mỗi chương trình tự định nghĩa mã khác 0
  của nó, nhưng quy ước chung là khác 0 là lỗi.
- Trong script của mình, tự đặt quy ước và giữ nhất quán, ví dụ trong
  các script giám sát trước đây: 0 là sạch, 1 là lỗi, 2 là có thay đổi
  quan trọng cần xem lại. Ghi quy ước này ở đầu file, trong chú thích.

### 15.2. Kiểm tra cú pháp và chạy dò từng dòng

```bash
bash -n script.sh
bash -x script.sh
```

- `bash -n`: chỉ kiểm tra cú pháp, không chạy, báo lỗi cú pháp nếu có.
  Luôn chạy `bash -n` trước khi chạy thật, nhất là sau khi sửa file.
- `bash -x`: chạy và in từng lệnh trước khi thực hiện, kèm giá trị biến
  đã thay vào. Đây là cách gỡ lỗi nhanh nhất khi script chạy sai mà
  không rõ sai ở dòng nào.

### 15.3. Các chế độ an toàn của bash

```bash
set -e
set -u
set -o pipefail
set -x
```

- `set -e`: lệnh nào thất bại thì dừng script ngay, không chạy tiếp mù.
- `set -u`: dùng biến chưa gán thì báo lỗi ngay, thay vì coi như rỗng.
- `set -o pipefail`: trong một pipe, nếu một lệnh giữa pipe thất bại thì
  cả pipe coi là thất bại, thay vì chỉ tính lệnh cuối.
- `set -x`: tương đương `bash -x`, in từng lệnh ra khi chạy.
- Các chế độ này tốt cho script quan trọng, nhưng khi học thì đừng bật
  cả bốn cùng lúc ngay từ đầu, vì thông báo lỗi sẽ nhiều và khó đọc.
  Bật từng cái một, hiểu từng cái, rồi mới kết hợp.

### 15.4. trap, dọn dẹp khi thoát

```bash
tam="/tmp/tap-tin-tam-$$"
trap 'rm -f "$tam"' EXIT
echo "làm việc" > "$tam"
```

- `trap 'lệnh' EXIT`: khi script thoát, dù thoát bình thường hay vì lỗi,
  thì lệnh trong `trap` vẫn chạy. Dùng để xóa file tạm, đóng kết nối,
  trả lại trạng thái sạch.
- Tạo file tạm bằng `$$` để tên không trùng với lần chạy khác.

### 15.5. Ghi log đúng cách

```bash
log_file="chay-$(date +%Y%m%d).log"
echo "[$(date)] Bắt đầu" >> "$log_file"
echo "[$(date)] Xong, mã thoát: $?" >> "$log_file"
```

- Log luôn kèm thời gian, để sau này biết việc đó xảy ra lúc nào.
- Log lỗi ra file riêng hoặc gộp bằng `2>&1`, đừng để lỗi chỉ hiện trên
  màn hình rồi mất khi đóng terminal.
- Khi script chạy bằng cron, log là thứ duy nhất mình có để biết nó đã
  chạy và chạy ra sao.

Bài tập phần 15: cố ý viết một script có lỗi cú pháp, chạy `bash -n`
để xem báo lỗi, sửa lại, rồi chạy `bash -x` để xem từng dòng chạy ra sao.

---

## Phần 16. Quy trình viết một script thực tế từ đầu đến cuối

Đây là quy trình dùng cho mọi script sau này, dù script ngắn hay dài.

### Bước 1. Kể việc bằng tiếng Việt, theo đúng thứ tự làm bằng tay

Ví dụ việc "kiểm tra đăng nhập thất bại trong ngày":

1. Chọn file log cần đọc.
2. Đếm tổng số dòng `Failed`.
3. Lấy IP từ các dòng `Failed`, đếm mỗi IP mấy lần, xếp hạng.
4. In ra tổng số và top 5 IP.
5. Nếu tổng số vượt ngưỡng, ví dụ 20, thì báo cần xem lại và thoát mã 2.

### Bước 2. Dịch từng bước thành một hoặc hai dòng lệnh

- Bước 1: `log_file="/var/log/auth.log"`, nhưng trên máy không có file
  này thì dùng file log giả để thử.
- Bước 2: `grep -c "Failed" "$log_file"`.
- Bước 3: `grep "Failed" "$log_file" | awk '{ print $6 }' | sort |
  uniq -c | sort -nr`.
- Bước 4: `echo` kết quả.
- Bước 5: `if [[ "$tong" -gt 20 ]]; then ...; exit 2; fi`.

### Bước 3. Viết khung script, 6 phần theo thứ tự

1. Shebang: `#!/usr/bin/env bash`.
2. Chú thích: làm gì, chạy ra sao, chỉ đọc hay có sửa.
3. Biến: đặt tên tiếng Việt, giá trị mặc định rõ ràng.
4. Hàm: `usage`, và các hàm làm từng việc nhỏ.
5. Xử lý tham số: mẫu `while` + `case` + `shift` ở Phần 9.
6. Phần chính: gọi các hàm theo đúng thứ tự đã kể ở Bước 1, rồi `exit`
   với mã đã quy ước.

### Bước 4. Chạy thử từng mảnh nhỏ

- Chạy từng lệnh trong terminal trước, xem kết quả đúng ý chưa, rồi mới
  đưa vào file.
- Chạy `bash -n` kiểm tra cú pháp.
- Chạy trên dữ liệu giả trước, rồi mới chạy trên dữ liệu thật.
- Mỗi lần sửa chỉ sửa một dòng, chạy lại, xem dòng đó đã đúng chưa,
  rồi mới sửa dòng sau.

### Bước 5. Giữ một bản đang chạy được

Trước khi thêm tính năng mới, copy file đang chạy được sang tên khác,
ví dụ `script.sh.bak`, hoặc commit lên Git. Thêm tính năng mới mà hỏng
thì còn bản cũ để quay lại, không phải viết lại từ đầu.

### Bước 6. Viết bài kiểm tra nhỏ cho chính script của mình

- Trường hợp bình thường: chạy với dữ liệu bình thường, kết quả đúng.
- Trường hợp không có gì: file rỗng, thư mục rỗng, không có kết nối,
  script vẫn chạy, không báo lỗi giả, không crash.
- Trường hợp bất thường: IP lạ, file lạ, cổng lạ, script báo đúng và
  thoát đúng mã đã quy ước.

Bài tập phần 16: chọn một việc mình làm hàng ngày trên terminal, kể 5
bước bằng tiếng Việt, rồi làm theo 6 bước trên để ra một script chạy
được thật, dù script đó chỉ có 20 dòng.

---

## Phần 17. Bốn script mẫu hoàn chỉnh

Các script dưới đây là khung hoàn chỉnh, có thể copy, chạy thử trên dữ
liệu giả, rồi sửa tên file, ngưỡng, thư mục cho đúng máy của mình. Đừng
chạy mù trên dữ liệu thật khi chưa đọc hết và chưa thử trên dữ liệu giả.

### 17.1. Đếm đăng nhập thất bại và xếp hạng IP

```bash
#!/usr/bin/env bash
# dem-loi-dang-nhap.sh: đếm dòng Failed trong file log và xếp hạng IP.
# Chỉ đọc file, không sửa gì. Cách dùng: ./dem-loi-dang-nhap.sh log.txt
log_file="${1:-log-mau.txt}"
if [[ ! -f "$log_file" ]]; then
  echo "Không có file: $log_file"
  exit 1
fi
tong=$(grep -c "Failed" "$log_file")
echo "Tổng số dòng Failed: $tong"
echo "Top IP thất bại nhiều nhất:"
grep "Failed" "$log_file" | awk '{ print $6 }' | sort | uniq -c | sort -nr | head -5
if [[ "$tong" -gt 20 ]]; then
  echo "Cần xem lại, số lần thất bại vượt ngưỡng 20."
  exit 2
fi
exit 0
```

### 17.2. Sao lưu một thư mục theo ngày

```bash
#!/usr/bin/env bash
# sao-luu.sh: nén một thư mục thành file tar.gz theo ngày, chỉ tạo file
# mới, không xóa file cũ. Cách dùng: ./sao-luu.sh /tmp/tai-lieu /tmp/sao-luu
nguon="$1"
dich_dir="$2"
if [[ -z "$nguon" || -z "$dich_dir" ]]; then
  echo "Cách dùng: $0 THU_MUC_NGUON THU_MUC_DICH"
  exit 1
fi
if [[ ! -d "$nguon" ]]; then
  echo "Thư mục nguồn không có: $nguon"
  exit 1
fi
mkdir -p "$dich_dir"
ngay=$(date +%Y%m%d)
tap_tin_dich="$dich_dir/sao-luu-$ngay.tar.gz"
tar -czf "$tap_tin_dich" -C "$(dirname "$nguon")" "$(basename "$nguon")"
if [[ -s "$tap_tin_dich" ]]; then
  echo "Sao lưu xong: $tap_tin_dich"
  exit 0
else
  echo "Sao lưu thất bại, file đích rỗng hoặc không có."
  exit 1
fi
```

### 17.3. Kiểm tra hệ thống nhanh

```bash
#!/usr/bin/env bash
# kiem-tra-he-thong.sh: in thư mục hiện tại, dung lượng đĩa, bộ nhớ,
# và thời gian máy chạy. Chỉ đọc, không sửa gì.
echo "Thư mục hiện tại: $(pwd)"
echo "Dung lượng đĩa:"
df -h
echo "Bộ nhớ:"
free -h
echo "Máy chạy:"
uptime
```

### 17.4. Giám sát mạng cơ bản trên máy mình

```bash
#!/usr/bin/env bash
# giam-sat-mang.sh: liệt kê cổng đang nghe, kết nối hiện tại, và lưu
# lượng từng card, ghi vào một file log theo ngày. Chỉ đọc, không chặn.
log_file="mang-$(date +%Y%m%d-%H%M%S).log"
{
  echo "Lúc: $(date)"
  echo "Cổng đang nghe:"
  ss -tnlp
  echo "Kết nối hiện tại:"
  ss -tn
  echo "Lưu lượng:"
  cat /proc/net/dev
} > "$log_file"
echo "Đã ghi log vào $log_file"
```

Bốn script trên dùng đúng các khái niệm đã học: biến, kiểm tra file,
`if`, `for` không cần thiết ở đây vì `grep` và `awk` đã làm phần lặp,
chuyển hướng, thay lệnh, và mã thoát. Khi đọc được bốn script này trôi
chảy, tức là mình đã nắm được khung của một script thực tế.

---

## Phần 18. An toàn và bảo mật khi viết script

### 18.1. Những việc không bao giờ làm trong script

- Không viết `rm -rf "$thu_muc/"` khi `$thu_muc` có thể rỗng, vì rỗng
  thì lệnh thành `rm -rf /`, xóa cả hệ thống. Luôn kiểm tra
  `[[ -n "$thu_muc" && "$thu_muc" != "/" ]]` trước khi xóa, và khi học
  thì tránh `rm -rf` trong script, xóa bằng tay sau khi đã xem kỹ.
- Không ghi đè file quan trọng bằng `>` mà không sao lưu trước.
- Không đưa mật khẩu, token, khóa riêng vào script, vào log, vào Git.
  Mật khẩu và token để ở nơi lưu trữ an toàn riêng, script chỉ đọc từ
  đó khi thật cần, và không in ra màn hình.
- Không chạy script tải từ Internet mà chưa đọc hết, đặc biệt là dạng
  `curl ... | bash`, vì chạy là tin toàn bộ nội dung đó.
- Không quét, không bắt gói tin, không thử đăng nhập vào máy và mạng
  không thuộc về mình, không chia sẻ kết quả quét đó cho người khác.

### 18.2. Những thói quen an toàn nên giữ

- Luôn đặt biến trong ngoặc kép: `"$bien"`.
- Luôn kiểm tra file, thư mục tồn tại trước khi đọc, ghi, xóa.
- Luôn chạy thử trên dữ liệu giả, thư mục giả trước.
- Luôn ghi log, để sau này biết script đã làm gì, lúc nào.
- Luôn sao lưu trước khi sửa, xóa, hoặc chạy script thay đổi hệ thống.
- Script giám sát thì ưu tiên chỉ đọc. Muốn chặn, sửa, xóa thì viết
  thành một script riêng, chạy riêng, sau khi đã xem kỹ báo cáo của
  script giám sát.

### 18.3. Quyền của file script

- Script chỉ mình dùng thì đặt `chmod 700`, chỉ mình đọc và chạy được.
- Script có chứa đường dẫn, tên máy, cấu hình riêng thì không đẩy lên
  repo công khai mà chưa rà soát.
- Trước khi đẩy bất kỳ file nào lên GitHub, xem lại bằng `git status`
  và `git diff`, chắc chắn không có mật khẩu, token, file riêng tư.

---

## Phần 19. Sau shell thì học gì tiếp

### 19.1. Khi nào dùng shell, khi nào dùng ngôn ngữ khác

- Việc lặp lại theo bước, gọi các lệnh có sẵn, xử lý file và log, giám
  sát hệ thống và mạng: shell là lựa chọn nhanh nhất, gọn nhất.
- Việc cần xử lý dữ liệu phức tạp, làm việc với API có cấu trúc JSON
  sâu, làm ứng dụng có giao diện, làm việc với cơ sở dữ liệu lớn: Python
  sẽ dễ đọc, dễ bảo trì hơn shell. Học Python sau shell không phải là
  bỏ shell, mà là thêm một công cụ vào hộp đồ nghề.
- Shell không phải là ngôn ngữ lập trình chung để làm ứng dụng như
  Python hay C, mà là ngôn ngữ kịch bản, tức scripting language, chuyên
  để tự động hóa việc của hệ điều hành. Scripting language vẫn là một
  loại ngôn ngữ lập trình, có biến, có if, có vòng lặp, có hàm, chỉ là
  nó sinh ra để tự động hóa, không phải để làm mọi thứ.

### 19.2. Hướng đi tiếp theo, theo thứ tự

1. Viết 10 script thật cho việc hàng ngày của mình, mỗi script một việc,
   lưu lại, dùng lại hàng tuần.
2. Học Python cơ bản, rồi viết lại một script shell phức tạp bằng
   Python để tự thấy chỗ nào Python gọn hơn, chỗ nào shell gọn hơn.
3. Học Git cơ bản: `clone`, `add`, `commit`, `push`, `pull`, để lưu
   script lên repo, lấy về máy khác dùng tiếp.
4. Học cron và systemd timer để chạy script định kỳ một cách đàng hoàng,
   có log, có báo lỗi.
5. Học GitHub Actions ở mức dùng workflow có sẵn: workflow cũng là các
   bước chạy shell theo thứ tự, nên người đã biết shell sẽ đọc workflow
   rất nhanh.

---

## Phụ lục A. 20 từ tiếng Anh hay gặp nhất

| Từ | Đọc | Nghĩa tiếng Việt | Gặp ở đâu |
| --- | --- | --- | --- |
| if | if | nếu | rẽ nhánh |
| then | then | thì | sau if |
| else | else | nếu không | nhánh còn lại |
| elif | elif | nếu không mà | if nhiều tầng |
| fi | fi | hết if, từ if viết ngược | đóng if |
| for | for | với mỗi | vòng lặp qua danh sách |
| while | while | trong khi | vòng lặp theo điều kiện |
| until | until | cho đến khi | vòng lặp ngược while |
| do | do | làm, bắt đầu thân lặp | sau for, while |
| done | done | hết vòng lặp | đóng for, while |
| case | case | trường hợp | rẽ nhánh theo giá trị |
| esac | esac | hết case, từ case viết ngược | đóng case |
| echo | echo | in ra | in thông báo |
| read | read | đọc vào | đọc bàn phím, đọc file |
| exit | exit | thoát, kèm mã thoát | kết thúc script |
| return | return | trả về mã thoát | kết thúc hàm |
| shift | shift | dịch tham số sang trái | xử lý tham số |
| test | test | kiểm tra | kiểm tra file, số, chuỗi |
| source | source | nạp vào shell hiện tại | nạp biến, hàm |
| usage | usage | cách dùng | hàm in trợ giúp |

Dịch bằng lời của mình, rồi dùng tên biến tiếng Việt trong script, thì
tiếng Anh không còn là rào cản nữa, vì mình chỉ cần nhớ đúng 20 từ này.

---

## Phụ lục B. Bảng tra nhanh

### B.1. Mã thoát tự đặt trong giáo trình này

- `0`: thành công, hoặc không thấy bất thường.
- `1`: lỗi, ví dụ thiếu tham số, không có file, không ghi được log.
- `2`: có thay đổi hoặc dấu hiệu quan trọng cần xem lại, ví dụ IP mới,
  cổng mới, số lần thất bại vượt ngưỡng.

### B.2. Lệnh tra cứu nhanh

- `ten_lenh --help`: xem cách dùng ngắn.
- `man ten_lenh`: xem tài liệu đầy đủ, bấm `q` để thoát.
- `type ten_lenh`: xem lệnh là nội tại hay chương trình ngoài.
- `bash -n script.sh`: kiểm tra cú pháp, không chạy.
- `bash -x script.sh`: chạy và in từng dòng để gỡ lỗi.

### B.3. Công thức pipe hay dùng

- Đếm và xếp hạng: `... | sort | uniq -c | sort -nr | head -10`.
- Lấy cột rồi đếm: `grep mẫu file | awk '{ print $6 }' | sort |
  uniq -c | sort -nr`.
- Xem log trực tiếp: `tail -f file.log`.
- Tìm file trong cửa sổ thời gian: `find . -type f -newermt "2026-10-26
  02:00" ! -newermt "2026-10-26 03:00"`.

---

## Phụ lục C. Lộ trình 30 ngày và checklist tự đánh giá

Lộ trình này là gợi ý, không phải bài tập bắt buộc mỗi ngày. Nếu một
ngày bận thì dời sang hôm sau, đừng dồn hai ngày thành một ngày, vì dồn
là đúng kiểu làm mình rối mà giáo trình này muốn tránh.

- Ngày 1-3: Phần 0 và 1, chạy được script chào đầu tiên bằng cả ba cách.
- Ngày 4-6: Phần 2 và 3, viết script nhận tham số, dùng biến và ngoặc
  đúng, không bị tách từ.
- Ngày 7-9: Phần 4 và 5, tra được dấu trong một dòng thật, viết được kiểm
  tra file, chuỗi, số.
- Ngày 10-12: Phần 6, viết được menu bằng `case`, không lồng `if` quá
  sâu.
- Ngày 13-15: Phần 7, đọc được file từng dòng bằng `while IFS= read -r`,
  lặp qua file bằng `for`.
- Ngày 16-18: Phần 8 và 9, tách được hàm, viết được `usage`, xử lý được
  tham số bằng `while` + `case` + `shift`.
- Ngày 19-21: Phần 10 và 11, dùng được pipe đếm và xếp hạng, lấy được cột
  bằng `awk`, tìm được dòng bằng `grep`.
- Ngày 22-24: Phần 12 và 13, sao lưu được bằng `tar`, tìm được file bằng
  `find`, xem được tiến trình và tài nguyên.
- Ngày 25-27: Phần 14 và 15, giám sát được mạng máy mình bằng `ss` và
  `ip neigh`, gỡ lỗi được bằng `bash -n` và `bash -x`.
- Ngày 28-30: Phần 16, 17, 18, viết được một script thật từ kể bước đến
  chạy thử trên dữ liệu giả, rồi dùng thật hàng tuần.

### Checklist tự đánh giá, đánh dấu khi làm được thật

- [ ] Tôi viết được script có shebang, chạy được bằng `bash` và `./`.
- [ ] Tôi phân biệt được lệnh nội tại và chương trình ngoài bằng `type`.
- [ ] Tôi gán biến đúng, không có khoảng trắng quanh `=`, và luôn viết
  `"$bien"` trong ngoặc kép.
- [ ] Tôi tra được nghĩa của một dấu trong một dòng thật, không cần nhớ
  hết bảng dấu.
- [ ] Tôi viết được kiểm tra file, chuỗi, số bằng `[ ]` và `[[ ]]`.
- [ ] Tôi viết được `if` có `elif`, `else`, và đóng đúng bằng `fi`.
- [ ] Tôi viết được menu bằng `case`, đóng đúng bằng `esac`, không quên
  `;;`.
- [ ] Tôi viết được `for` qua file, và `while IFS= read -r` để đọc file
  từng dòng.
- [ ] Tôi tách được hàm, dùng `local`, và phân biệt `return` với `echo`
  để trả chuỗi.
- [ ] Tôi xử lý được tham số `--tap-tin FILE` và `--help` bằng mẫu
  `while` + `case` + `shift`, không bị vòng lặp vô hạn.
- [ ] Tôi dùng được pipe đếm và xếp hạng, và lấy được cột bằng `awk`.
- [ ] Tôi sao lưu được bằng `tar`, kiểm tra được file đích bằng `-s`.
- [ ] Tôi giám sát được mạng máy mình bằng `ss`, `ip neigh`, và chỉ làm
  trên máy và mạng của mình.
- [ ] Tôi gỡ lỗi được bằng `bash -n` và `bash -x`, và ghi log kèm thời gian.
- [ ] Tôi viết được một script thật, từ kể bước tiếng Việt đến chạy thử
  trên dữ liệu giả, rồi dùng thật.

Hết giáo trình. Khi làm xong checklist trên, cậu đã không còn ở mức
"đọc được mà chưa viết được" nữa, mà đã ở mức "viết được script thật
cho việc thật của mình". Việc còn lại chỉ là viết thêm, viết nhiều hơn,
và mỗi lần viết lại gọn hơn, an toàn hơn lần trước một chút.
