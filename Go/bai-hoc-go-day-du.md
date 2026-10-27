# Bài học Go đầy đủ, chi tiết, dễ hiểu

Viết bởi Capybara cho Hoàng Dươngg — học từng bài một, mỗi bài chỉ một khái niệm mới.

Cách học file này: đừng đọc một lèo từ đầu tới cuối. Học xong bài nào ở trong chat thì mở đúng bài đó ở đây để ôn lại. Mỗi bài đều có code chạy được trên Go Playground (`https://go.dev/play/`), bóc từng dòng, kết quả chạy, phần nào tự đổi được, phần nào bắt buộc, và lỗi hay gặp.

Quy tắc chung, nhớ suốt cả file này:

- Tên biến, tên hàm, câu chữ in ra: cậu tự đặt, tự đổi được, miễn là đổi ở mọi chỗ dùng nó.
- Kiểu dữ liệu (`string`, `int`), tên gói (`fmt`, `strings`), dấu `&` trong `Scanln`, vị trí dấu `{`: bắt buộc, đổi sai là máy báo lỗi.
- Go phân biệt chữ hoa, chữ thường. `loiChao` và `loichao` là hai tên khác nhau.
- Chương trình luôn bắt đầu từ `main`, không phải từ dòng đầu tiên của file. Hàm viết trước hay sau `main` đều chạy được, để trước chỉ để đọc cho dễ.

---

## Bài 1. In ra màn hình

Khái niệm mới duy nhất: in một dòng chữ ra màn hình.

Từ tiếng Anh:

- `package` = gói.
- `import` = mượn, nhập về dùng.
- `func` = viết tắt của `function` = hàm.
- `main` = chính, hàm chính.
- `Println` = `Print` (in) + `ln` (line, dòng). In xong thì xuống dòng mới.

Code:

```go
package main

import "fmt"

func main() {
    fmt.Println("Xin chao Capybara")
}
```

Bóc từng dòng:

- `package main`: khai báo file này thuộc gói `main`, là gói của chương trình chạy được. Không phải là "lấy gói main".
- `import "fmt"`: mượn gói `fmt` về để in. `fmt` là gói in ấn, định dạng.
- `func main() {`: mở hàm chính. Mọi chương trình Go đều bắt đầu chạy từ đây.
- `fmt.Println("Xin chao Capybara")`: in chữ trong dấu ngoặc kép ra màn hình, rồi xuống dòng.
- `}`: đóng hàm `main`. Dòng lệnh nằm giữa `{` và `}` mới chạy khi `main` chạy.

Kết quả:

```
Xin chao Capybara
Program exited.
```

`Program exited.` không phải lỗi. Nó chỉ báo chương trình chạy xong bình thường.

Tự đổi được: chữ trong ngoặc kép, ví dụ đổi thành `"Tam biet Capybara"` thì in ra chữ đó.
Bắt buộc: `package main`, `import "fmt"`, `func main()`, dấu ngoặc kép quanh chữ.

Lỗi hay gặp: quên dấu ngoặc kép, quên `import "fmt"` mà vẫn dùng `fmt.Println`.

---

## Bài 2. Biến

Khái niệm mới duy nhất: biến, tức là một cái hộp đựng một giá trị.

Từ tiếng Anh:

- `variable` = biến, viết tắt là `var` khi khai báo kiểu dài.
- `string` = chuỗi, chuỗi ký tự, tức là chữ.
- `int` = viết tắt của `integer` = số nguyên, số không có phần thập phân.

Code:

```go
package main

import "fmt"

func main() {
    loiChao := "Xin chao Capybara"
    fmt.Println(loiChao)
}
```

Bóc từng dòng:

- `loiChao := "Xin chao Capybara"`: tạo một hộp mới tên `loiChao`, bỏ chữ `"Xin chao Capybara"` vào hộp.
- `:=` đọc là "tạo hộp mới rồi gán". Đây là cách ngắn gọn nhất để tạo biến trong hàm.
- `loiChao` là `loi` + `Chao`, viết theo kiểu camelCase: từ đầu viết thường, từ sau viết hoa chữ đầu để tách từ, vì tên biến không được có dấu cách.
- `fmt.Println(loiChao)`: in cái trong hộp `loiChao`, không có ngoặc kép, vì đây là tên hộp, không phải chữ trực tiếp. Máy sẽ nhìn vào trong hộp để lấy giá trị.

Kết quả:

```
Xin chao Capybara
Program exited.
```

Một hộp chỉ giữ một giá trị tại một thời điểm. Muốn giữ nhiều giá trị cùng lúc thì dùng danh sách, xem Bài 11.

Cách khai báo khác, cùng ý nghĩa:

```go
var ten string
ten = "Hoang"
```

`var` tạo hộp rỗng trước, rồi gán sau. `:=` tạo hộp và bỏ giá trị vào luôn trong một dòng. Trong hàm thì dùng `:=` nhiều hơn.

Tự đổi được: tên hộp `loiChao`, giá trị trong ngoặc kép.
Bắt buộc: dùng đúng tên hộp ở mọi chỗ, đúng chính tả hoa thường.

Lỗi hay gặp: khai báo hộp rồi không dùng, máy báo `declared and not used`. Ví dụ khai báo `loichao` mà in `loiChao` thì máy coi là hai hộp khác nhau, một hộp không được dùng, một hộp chưa được khai báo.

---

## Bài 3. Gán lại giá trị

Khái niệm mới duy nhất: đổi giá trị trong hộp đã có, không tạo hộp mới.

Code:

```go
package main

import "fmt"

func main() {
    loiChao := "Xin chao Capybara"
    fmt.Println(loiChao)
    loiChao = "Tam biet Capybara"
    fmt.Println(loiChao)
}
```

Bóc:

- Dòng `:=` tạo hộp mới, bỏ giá trị đầu tiên vào.
- Dòng `loiChao = "Tam biet Capybara"` không tạo hộp mới, chỉ thay lõi trong cùng một hộp. Dấu `=` ở đây là gán, không phải giá trị mặc định.
- Một hộp lần lượt giữ hai giá trị, không phải hai hộp, không phải cùng lúc hai giá trị.

Kết quả:

```
Xin chao Capybara
Tam biet Capybara
Program exited.
```

Ba dấu dễ nhầm, nhớ bằng một câu:

- `:=` tạo hộp mới rồi gán.
- `=` đổi giá trị hộp đã có.
- `==` so sánh, xem hai bên có bằng nhau không, không đổi gì cả. `==` sẽ học kỹ ở Bài 5.

Lỗi hay gặp: dùng `:=` lần thứ hai cho cùng một hộp trong cùng một khối lệnh, máy sẽ báo hộp đã khai báo rồi. Muốn đổi giá trị thì dùng `=` thôi.

---

## Bài 4. Phép tính với số

Khái niệm mới duy nhất: bốn phép tính số học.

Các dấu:

- `+` cộng.
- `-` trừ.
- `*` nhân.
- `/` chia.

Code:

```go
package main

import "fmt"

func main() {
    dem := 10
    fmt.Println(dem)
    dem = dem + 2
    fmt.Println(dem)
    dem = dem - 3
    fmt.Println(dem)
    dem = dem * 2
    fmt.Println(dem)
    dem = dem / 3
    fmt.Println(dem)
}
```

Bóc:

- `dem` là biến số nguyên, không phải hàm. Các dòng `dem = ...` chỉ tính rồi cất lại vào hộp, không in gì cả. Chỉ `fmt.Println` mới in.
- `dem = dem + 2` nghĩa là lấy giá trị cũ trong hộp, cộng 2, rồi bỏ kết quả mới vào lại cùng hộp đó.

Kết quả, tính từng bước: 10, rồi 12, rồi 9, rồi 18, rồi 6.

```
10
12
9
18
6
Program exited.
```

Thứ tự tính trong một biểu thức: `*` và `/` tính trước `+` và `-`, giống toán học. Ví dụ `2 + 3 * 4` bằng 14, không phải 20. Muốn đổi thứ tự thì dùng ngoặc tròn: `(2 + 3) * 4` bằng 20.

Chia số nguyên: `7 / 2` bằng 3, không phải 3.5, vì `int` bỏ phần lẻ đi. Muốn có phần thập phân thì phải dùng số thực `float64`, học sau, không cần vội ở đây.

Lời khuyên ở giai đoạn này: mỗi dòng chỉ làm một phép tính cho dễ đọc, đừng gộp nhiều phép tính vào một dòng.

Ví dụ rút gọn, chỉ in kết quả cuối:

```go
dem := 10
dem = dem + 2
dem = dem - 3
dem = dem * 2
dem = dem / 3
fmt.Println(dem) // 6
```


---

## Bài 5. If và Else: rẽ nhánh

Khái niệm mới duy nhất: `if`, chạy theo điều kiện đúng hay sai.

Từ tiếng Anh:

- `if` = nếu.
- `else` = ngược lại, nếu không thì.

Code:

```go
package main

import "fmt"

func main() {
    loiChao := "Xin chao Capybara"
    if loiChao == "Xin chao Capybara" {
        fmt.Println("Dung roi")
    } else {
        fmt.Println("Sai roi")
    }
    fmt.Println(loiChao)
}
```

Bóc:

- `if loiChao == "Xin chao Capybara"`: nếu giá trị trong hộp bằng đúng chữ đó thì chạy khối `if`.
- `==` là so sánh bằng. Đừng nhầm với `=` là gán.
- `{` của `if` phải nằm cùng dòng với điều kiện. Viết `{` xuống dòng dưới là máy báo lỗi `unexpected newline, expected { after if clause`.
- `} else {` phải nằm trên một dòng. Lý do: Go tự chèn dấu chấm phẩy sau dòng kết thúc bằng `}`, nếu xuống dòng thì câu `if` coi như kết thúc rồi, `else` bị mồ côi.
- `if` và `else` là hai mặt đối nhau, không phải nghịch đảo toán học. Điều kiện đúng thì chỉ chạy `if`, sai thì chỉ chạy `else`, luôn luôn đúng một trong hai nhánh, không chạy cả hai, không bỏ cả hai.

Kết quả:

```
Dung roi
Xin chao Capybara
Program exited.
```

Các phép so sánh dùng trong `if` và `for`:

- `==` bằng.
- `!=` khác.
- `<` nhỏ hơn.
- `>` lớn hơn.
- `<=` nhỏ hơn hoặc bằng, bao gồm cả điểm cuối.
- `>=` lớn hơn hoặc bằng, bao gồm cả điểm cuối.

Ví dụ hướng đi, nhớ kỹ:

- Tăng dần: bắt đầu nhỏ, dùng `<` hoặc `<=`, và tăng lên. Ví dụ `dem := 0; dem < 3`.
- Giảm dần: bắt đầu lớn, dùng `>` hoặc `>=`, và giảm xuống. Ví dụ `dem := 10; dem > 4`, và phải dùng `dem = dem - 1`, không dùng `+ 1` vì tăng lên thì càng xa điều kiện dừng.

Có thể viết `else if` để xét nhiều trường hợp:

```go
diem := 8
if diem >= 9 {
    fmt.Println("Gioi!")
} else if diem >= 5 {
    fmt.Println("Dat")
} else {
    fmt.Println("Chua dat")
}
```

---

## Bài 6. For: vòng lặp

Khái niệm mới duy nhất: `for`, lặp lại một khối lệnh cho tới khi điều kiện sai.

Từ tiếng Anh: `for` = cho, lặp cho tới khi.

Go chỉ có một vòng lặp là `for`, không có `while` riêng. Dạng `while` của ngôn ngữ khác chính là dạng `for` chỉ có điều kiện dưới đây.

Code:

```go
package main

import "fmt"

func main() {
    dem := 0
    for dem < 3 {
        fmt.Println("Xin chao Capybara")
        dem = dem + 1
    }
}
```

Bóc, chạy chậm từng vòng:

- Vòng 1: `dem` là 0, 0 < 3 đúng, in một lần, `dem` thành 1.
- Vòng 2: `dem` là 1, 1 < 3 đúng, in một lần, `dem` thành 2.
- Vòng 3: `dem` là 2, 2 < 3 đúng, in một lần, `dem` thành 3.
- Kiểm tra lại: 3 < 3 sai, vòng lặp tự dừng, không cần lệnh dừng riêng.

Kết quả: in 3 lần.

```
Xin chao Capybara
Xin chao Capybara
Xin chao Capybara
Program exited.
```

Giải thích bằng ngón tay cho dễ nhớ: chào một cái, giơ một ngón, chào tiếp, giơ tiếp, đủ 3 ngón thì dừng.

Điểm quan trọng:

- Chạy tới dấu `}` cuối của `for` thì quay lại kiểm tra điều kiện, không phải chạy tiếp xuống dưới.
- `dem = dem + 1` không in gì cả, nó chỉ lặng lẽ đẩy điều kiện tiến về phía dừng.
- Quên dòng tăng `dem` thì `dem` mãi là 0, điều kiện mãi đúng, vòng lặp chạy vô hạn.

Đổi điểm bắt đầu thì đổi số lần lặp, với `for dem < 3`:

- Bắt đầu 0 thì lặp 3 lần: 0, 1, 2.
- Bắt đầu 1 thì lặp 2 lần: 1, 2.
- Bắt đầu 2 thì lặp 1 lần: 2.
- Bắt đầu 3 hoặc 4 thì lặp 0 lần, vì ngay lần kiểm tra đầu tiên đã sai.

Ví dụ giảm dần:

```go
dem := 10
for dem > 4 {
    fmt.Println(dem)
    dem = dem - 1
}
```

In ra 10, 9, 8, 7, 6, 5, rồi dừng khi `dem` thành 4. Chú ý: với `> 4` thì không có vòng lặp nào ở giá trị 4, giá trị còn lại sau khi thoát là 4. Với `>= 4` thì có vòng lặp ở 4, rồi `dem` thành 3 mới dừng.

Dạng `for` truyền thống, viết gọn cả ba phần trong một dòng, học sau dạng điều kiện cũng được:

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

`i++` nghĩa là `i = i + 1`, viết tắt. Dạng này cũng in 0, 1, 2, 3, 4.

---

## Bài 7. Hàm có tham số

Khái niệm mới duy nhất: tham số, tức là chỗ trống trong hàm, lúc gọi mới lấp giá trị thật vào.

Từ tiếng Anh:

- `parameter` = tham số, tên chỗ trống lúc viết hàm.
- `argument` = đối số, giá trị thật lúc gọi hàm.

Code:

```go
package main

import "fmt"

func chao(ten string) {
    fmt.Println("Xin chao " + ten)
}

func main() {
    chao("Hoang")
    chao("Capybara")
}
```

Bóc:

- `func chao(ten string)`: định nghĩa hàm tên `chao`, có một tham số tên `ten`, kiểu `string`. Trong Go, tên viết trước, kiểu viết sau.
- `ten` là chỗ trống, không phải giá trị cố định, không phải mặc định.
- `"Xin chao "` có dấu cách ở cuối, để khi nối với tên thì không bị dính chữ thành `Xin chaoHoang`.
- Dấu `+` giữa hai chuỗi là nối chữ, không phải cộng số.
- `chao("Hoang")`: gọi hàm, lấp `"Hoang"` vào chỗ trống `ten`, in ra `Xin chao Hoang`.
- `chao("Capybara")`: gọi lại cùng một hàm, lấp giá trị khác vào, in ra `Xin chao Capybara`.

Kết quả:

```
Xin chao Hoang
Xin chao Capybara
Program exited.
```

Tên hàm và tên tham số đều tự đặt, nhưng phải là một từ liền, không có dấu cách, không bắt đầu bằng chữ số. Ví dụ `func xinchao(accnd string)` là đúng, `accnd` là tên tham số tự đặt, dùng nhất quán trong thân hàm là được.

Đoạn định nghĩa hàm một mình thì chưa chạy gì cả, phải có `main` gọi nó thì mới chạy. Ví dụ cậu tự viết `func xinchao(accnd string) { fmt.Println("Hehe " + accnd) }` là đúng cấu trúc, chỉ cần thêm `main` gọi `xinchao("Hoang")` là chạy.

Quy tắc ngoặc: mở `(` thì phải đóng `)`, mở `{` thì phải đóng `}`. Quên một dấu đóng là máy báo lỗi ngay.

---

## Bài 8. Hàm trả về

Khái niệm mới duy nhất: `return`, trả kết quả về cho chỗ gọi, thay vì in trong hàm.

Từ tiếng Anh: `return` = trả về, trả lại.

Code, chính là bài cậu hẹn tối học tiếp cho chắc:

```go
package main

import "fmt"

func tong(a int, b int) int {
    return a + b
}

func main() {
    ketQua := tong(3, 4)
    fmt.Println(ketQua)
    fmt.Println(tong(10, 20))
}
```

Bóc từng phần, chậm nhất có thể:

- `func tong(a int, b int)`: hàm tên `tong`, có hai tham số `a` và `b`, đều kiểu `int`.
- `a` và `b` không phải giá trị mặc định, không phải số cố định. Giá trị thật lấy từ lúc gọi hàm.
- Chữ `int` đứng sau dấu `)` là kiểu trả về, báo trước hàm này sẽ trả về một số nguyên.
- `return a + b`: tính `a + b`, rồi đưa kết quả đó về cho chỗ gọi, và hàm dừng luôn ở đó.
- `a + b` một mình, không có `return`, thì chỉ tính trong đầu hàm, bên ngoài không nhìn thấy kết quả.

Chạy chậm trong `main`:

1. Chương trình bắt đầu từ `main`, không phải từ `tong`.
2. Gặp `ketQua := tong(3, 4)`, máy nhảy vào `tong`, lúc này `a` là 3, `b` là 4, `a + b` là 7, `return 7` đưa 7 về.
3. Về tới `main`, `ketQua := 7` tạo hộp mới tên `ketQua`, bỏ 7 vào, rồi in ra 7.
4. Gặp `fmt.Println(tong(10, 20))`, máy lại nhảy vào `tong`, `a` là 10, `b` là 20, trả về 30, in thẳng 30 ra, không tạo hộp, không cất lại.

Kết quả:

```
7
30
Program exited.
```

Khác nhau giữa hai cách dùng kết quả:

- Cất vào hộp trước rồi in sau: `ketQua := tong(3, 4)` rồi `fmt.Println(ketQua)`. Dùng khi sau này còn cần kết quả đó nhiều lần.
- In thẳng luôn: `fmt.Println(tong(10, 20))`. Dùng khi chỉ cần in một lần rồi thôi, in xong là kết quả mất luôn, muốn dùng lại phải gọi lại.

Gọi hàm thôi mà không cất, không in, ví dụ chỉ viết `tong(3, 4)` một mình, thì máy tính xong rồi bỏ kết quả đi, cậu sẽ không thấy gì cả.

Đổi dấu trong `return`:

- `return a + b` trả về tổng.
- `return a - b` trả về hiệu.
- `return a * b` trả về tích.
- `return a / b` trả về thương nguyên, ví dụ 10 / 3 bằng 3.

Đổi dấu thì nên đổi luôn tên hàm cho đúng nghĩa, ví dụ hàm trừ thì đừng để tên là `tong`, vì máy thì vẫn chạy, nhưng người đọc sẽ bị nhầm.

Go không có giá trị mặc định cho tham số. Gọi hàm mà thiếu đối số thì máy báo lỗi thiếu đối số, bắt buộc phải truyền đủ.

Ví dụ cậu tự đổi tên theo mẫu này, cấu trúc vẫn đúng:

```go
func duaHau(a int, b int) int {
    return a + b
}

func main() {
    traiCay := duaHau(1, 2)
    fmt.Println(traiCay)      // 3
    fmt.Println(duaHau(9, 10)) // 19
}
```

Chú ý: đoạn trong `main` một mình thì chưa chạy được, ngoài `main` vẫn phải có định nghĩa hàm `duaHau` như trên.


---

## Bài 9. Chuỗi: string

Khái niệm mới: không phải một khái niệm hoàn toàn mới, mà là gọi tên và dùng sâu hơn thứ cậu đã dùng từ Bài 1. `string` chỉ là nhãn báo hộp đựng chữ, không phải đựng số.

Nhắc lại ba điểm cơ bản:

- So sánh chữ bằng `==`: `ten == "Hoang"` đúng thì chạy nhánh `if`, sai thì chạy `else`.
- Chuỗi rỗng là `""`, tức là trong ngoặc kép không có chữ nào. In chuỗi rỗng ra thì chỉ thấy một dòng trống.
- Chữ trông giống số vẫn là chữ: `"12" + "3"` nối chữ thành `"123"`, trong khi `12 + 3` cộng số thành `15`. Phân biệt bằng ngoặc kép: có ngoặc kép là chữ, không có ngoặc kép mà là số viết trực tiếp thì là số, còn tên biến không có ngoặc kép thì máy nhìn vào giá trị trong hộp.

Code cơ bản:

```go
package main

import "fmt"

func main() {
    ten := "Hoang"
    if ten == "Hoang" {
        fmt.Println("Dung roi")
    } else {
        fmt.Println("Sai roi")
    }
    rong := ""
    fmt.Println("Chuoi rong la:")
    fmt.Println(rong)
    fmt.Println("12" + "3")
    fmt.Println(12 + 3)
}
```

Kết quả:

```
Dung roi
Chuoi rong la:

123
15
Program exited.
```

Dòng trống ở giữa là do in chuỗi rỗng `rong`.

Ba công cụ nâng cao của chuỗi, học sau phần cơ bản:

1. Đếm độ dài bằng `len`. `len` là viết tắt của `length`, nghĩa là độ dài.

```go
ten := "Capybara"
fmt.Println(len(ten)) // 8
```

Vị trí trong chuỗi đếm từ 0: trong `"Capybara"`, 0 là C, 1 là a, 2 là p, 3 là y, 4 là b, 5 là a, 6 là r, 7 là a.

Lưu ý quan trọng với tiếng Việt có dấu: `len` đếm theo byte, không phải theo ký tự nhìn thấy. Chữ có dấu sẽ đếm nhiều hơn số ký tự. Vì vậy ở giai đoạn này, khi tập đếm thì dùng ví dụ không dấu như `"Capybara"` cho dễ, chuyện đếm đúng ký tự tiếng Việt để sau học về `rune`.

2. Cắt chuỗi bằng chỉ số, dạng `[bắt đầu:kết thúc]`, lấy từ vị trí bắt đầu tới trước vị trí kết thúc:

```go
ten := "Capybara"
fmt.Println(ten[0:3]) // Cap
fmt.Println(ten[3:])  // ybara
fmt.Println(ten[:3])  // Cap
fmt.Println(ten[:])   // Capybara
fmt.Println(ten[0:5]) // Capyb
fmt.Println(ten[2:5]) // pyb
fmt.Println(ten[0:8]) // Capybara
```

Quy tắc: vị trí bắt đầu phải lớn hơn hoặc bằng 0, vị trí kết thúc phải nhỏ hơn hoặc bằng độ dài, bắt đầu phải nhỏ hơn hoặc bằng kết thúc. `[3:0]` là sai, `[0:9]` với chuỗi dài 8 là vượt giới hạn, máy sẽ dừng chương trình và báo lỗi. Bỏ phần sau dấu `:` nghĩa là tới cuối chuỗi, bỏ phần trước dấu `:` nghĩa là từ đầu chuỗi.

3. Tìm chữ trong chuỗi bằng `strings.Contains`. Chú ý tên đúng: gói là `strings`, có chữ `s`, hàm là `Contains`, viết hoa chữ `C`, không phải `string.contains`.

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    ten := "Capybara"
    fmt.Println(strings.Contains(ten, "para")) // true
    fmt.Println(strings.Contains(ten, "xyz"))  // false
    if strings.Contains(ten, "para") {
        fmt.Println("Co chu para")
    } else {
        fmt.Println("Khong co chu para")
    }
}
```

Bóc:

- `import "strings"` là mượn gói `strings`, giống như `import "fmt"` mượn gói `fmt`. Nhiều gói thì viết trong một khối `import (...)`.
- `strings.Contains(chuỗi lớn, chuỗi nhỏ)`: kiểm tra chuỗi lớn có chứa chuỗi nhỏ không, chỉ trả về `true` (có) hoặc `false` (không), không trả ra chính chuỗi nhỏ đó. Giống như kiểm tra trong túi có chìa khóa không, chỉ trả lời có hoặc không, không đưa chìa khóa ra.
- Thứ tự quan trọng: chuỗi lớn viết trước, chuỗi nhỏ viết sau. Đảo lại là sai ý nghĩa.
- Phân biệt hoa thường: `Contains(ten, "CAP")` với `ten` là `"Capybara"` sẽ ra `false`, vì `CAP` viết hoa khác `Cap`.
- `ten` ở đây không phải tên mặc định của Go, tên và giá trị đều tự đặt. Đổi `ten` thành `"Hoang"` thì tìm `"para"` sẽ ra `false`.

Kết quả của code trên:

```
true
false
Co chu para
Program exited.
```

---

## Bài 10. Nhập dữ liệu

Khái niệm mới duy nhất: chương trình không ghi sẵn giá trị trong code nữa, mà hỏi rồi chờ người dùng gõ vào.

Từ tiếng Anh:

- `var` = viết tắt của `variable` = biến.
- `Scanln` = `Scan` (quét, đọc) + `ln` (line, dòng). Đọc một dòng gõ vào, dừng ở phím Enter, giống như `Println` là in một dòng rồi xuống dòng.

Code bản gõ thật, chạy trên máy thật hoặc Termux, không chạy gõ tương tác được trên Go Playground:

```go
package main

import "fmt"

func main() {
    var ten string
    fmt.Println("Nhap ten cua ban:")
    fmt.Scanln(&ten)
    fmt.Println("Xin chao " + ten)
}
```

Chạy chậm:

1. In câu hỏi `"Nhap ten cua ban:"`.
2. Tới dòng `fmt.Scanln(&ten)` thì chương trình dừng lại chờ, người dùng gõ ví dụ `Hoang` rồi nhấn Enter.
3. `Scanln` bỏ chữ vừa gõ vào hộp `ten`.
4. In ra `Xin chao Hoang`.

Bóc từng dấu:

- `var ten string`: tạo hộp rỗng tên `ten`, kiểu `string`, chưa có giá trị. `ten` tự đặt, `string` là kiểu bắt buộc. Đổi `string` thành `int` thì hộp đó thành hộp số.
- `&ten`: dấu `&` bắt buộc ở đây, nó đưa địa chỉ của hộp cho `Scanln`, để `Scanln` biết bỏ chữ vừa gõ vào đúng hộp nào. Thiếu `&` là lỗi.
- `&` chỉ dùng ở `Scanln`, không dùng ở khai báo (`var ten string` không có `&`), không dùng ở in ra (`fmt.Println(ten)` không có `&`, vì in là lấy giá trị trong hộp ra, không phải đưa địa chỉ).
- Câu hỏi `"Nhap ten cua ban:"` không phải mặc định, cậu tự viết, đổi chữ khác cũng được, thậm chí bỏ đi thì chương trình vẫn chạy, chỉ là người dùng không biết phải gõ gì.

Caveat về Go Playground, nói rõ mức độ chắc chắn: theo tìm hiểu trên mạng, Go Playground không hỗ trợ gõ tương tác, nên `Scanln` kiểu thông thường ở đó sẽ không chờ cậu gõ được, chương trình có thể thoát ngay hoặc treo. Chưa kiểm chứng tận tay mọi trường hợp, nên ở đây cậu cứ hiểu bài này bằng cách đọc trước, còn gõ thật thì cần Termux hoặc máy thật có cài Go.

Bản chạy được trên Playground, dùng lại cấu trúc cậu đã chạy thành công buổi sáng, dòng `ten := "Capybara"` đóng vai "người dùng vừa gõ":

```go
package main

import "fmt"

func chao(ten string) {
    fmt.Println("Xin chao " + ten)
}

func main() {
    ten := "Capybara"
    chao(ten)
}
```

Muốn chào người khác thì chỉ đổi chữ trong ngoặc kép ở dòng `ten := ...`.

Lỗi hay gặp, đúng mẫu cậu từng viết:

```go
var ilovecapybara
fmt.Println("Nhap ten cua ban:")
fmt.Scanln(&ten)
fmt.Println("ilovecapybara" + ten)
```

Hai lỗi sẽ làm chương trình không chạy được:

1. `var ilovecapybara` không có kiểu. Dùng `var` mà không gán giá trị ban đầu thì bắt buộc phải ghi kiểu: `var ilovecapybara string`.
2. Tên không khớp: khai báo `ilovecapybara` mà `Scanln` và in lại dùng `ten`, thì `ten` chưa khai báo, còn `ilovecapybara` khai báo rồi mà không dùng.

Bản sửa, giữ đúng tên cậu đặt:

```go
var ilovecapybara string
fmt.Println("Nhap ten cua ban:")
fmt.Scanln(&ilovecapybara)
fmt.Println("Xin chao " + ilovecapybara)
```

---

## Bài 11. Danh sách: slice

Khái niệm mới duy nhất: danh sách, một hộp lớn chứa nhiều giá trị cùng lúc.

Code:

```go
package main

import "fmt"

func main() {
    tenList := []string{"Hoang", "Capybara", "Duongg"}
    fmt.Println(len(tenList))
    fmt.Println(tenList[0])

    dem := 0
    for dem < len(tenList) {
        fmt.Println(tenList[dem])
        dem = dem + 1
    }

    tenList = append(tenList, "Moi")
    fmt.Println(len(tenList))
    fmt.Println(tenList[3])
}
```

Bóc:

- `[]string` nghĩa là danh sách các chuỗi. Dấu `[]` là danh sách, `string` là kiểu của từng món trong danh sách. Đổi thành `[]int` thì thành danh sách các số.
- `{"Hoang", "Capybara", "Duongg"}` là liệt kê các món, cách nhau bằng dấu phẩy, trong dấu `{}`. Đúng như cậu nhận xét: danh sách chuỗi chính là liệt kê chuỗi lại với nhau.
- `len(tenList)` đếm số món, lúc đầu là 3.
- `tenList[0]` lấy món đầu tiên, vị trí đếm từ 0 giống cắt chuỗi: `[0]` là `"Hoang"`, `[1]` là `"Capybara"`, `[2]` là `"Duongg"`.
- Vòng lặp `for dem < len(tenList)` dùng lại Bài 6, mỗi lần in `tenList[dem]` rồi tăng `dem` lên 1, nên in lần lượt cả danh sách mà không cần viết riêng từng dòng in.
- `append(tenList, "Moi")`: `append` nghĩa là thêm vào, gắn thêm một món vào cuối danh sách. Phải gán lại bằng `tenList = append(...)` thì danh sách mới thật sự dài thêm, sau đó `len` thành 4 và `tenList[3]` là `"Moi"`.

Kết quả:

```
3
Hoang
Hoang
Capybara
Duongg
4
Moi
Program exited.
```

`tenList` là tên tự đặt, ghép `ten` với `List`, đổi thành `danhSach` cũng được, nhưng phải đổi ở tất cả các chỗ dùng nó, không chỉ đổi ở dòng khai báo.

Phân biệt mảng và slice, chỉ cần nhớ một câu ở đây:

- Mảng: kích thước cố định, ví dụ `var arr [3]int = [3]int{1, 2, 3}`.
- Slice: mảng động, dùng nhiều nhất trong thực tế, ví dụ `s := []int{1, 2, 3}` rồi `append` thêm được.

Map, giới thiệu ngắn, học sâu sau:

```go
tuoi := map[string]int{
    "Duongg":   18,
    "Capybara": 3,
}
tuoi["Moi"] = 20
fmt.Println(tuoi["Duongg"]) // 18
delete(tuoi, "Moi")
```

`map[string]int` là từ điển: khóa là chuỗi, giá trị là số nguyên. Tra bằng khóa, không tra bằng vị trí số như danh sách.

Duyệt danh sách bằng `range`, học sau dạng `for` đếm cũng được, nhưng ghi ở đây để cậu nhận ra khi gặp trong file cũ:

```go
ds := []string{"Tao", "Cam", "Chuoi"}
for i, v := range ds {
    fmt.Println(i, v)
}
```

`i` là chỉ số, `v` là giá trị tại chỉ số đó.

---

## Bài 12. Struct: gom dữ liệu của một thứ

Khái niệm mới duy nhất: `struct`, gom nhiều dữ liệu khác nhau của cùng một thứ vào một kiểu mới.

Từ tiếng Anh: `struct` = viết tắt của `structure` = cấu trúc.

Code:

```go
package main

import "fmt"

type Nguoi struct {
    Ten  string
    Tuoi int
}

func main() {
    d := Nguoi{Ten: "Duongg", Tuoi: 18}
    fmt.Println(d.Ten)
    d.Tuoi = 19
    fmt.Println(d.Tuoi)
}
```

Bóc:

- `type Nguoi struct { ... }`: định nghĩa một kiểu mới tên `Nguoi`, gồm hai trường: `Ten` kiểu `string`, `Tuoi` kiểu `int`.
- `Nguoi{Ten: "Duongg", Tuoi: 18}`: tạo một giá trị kiểu `Nguoi`, ghi rõ từng trường bằng `TênTrường: giá trị`.
- `d.Ten`: lấy trường `Ten` của `d`, dấu chấm là "của".
- `d.Tuoi = 19`: đổi trường `Tuoi` trong cùng một giá trị, giống gán lại biến.

Method, tức là hàm gắn với struct, học sau struct cũng được:

```go
func (n Nguoi) GioiThieu() string {
    return "Toi la " + n.Ten
}
```

Gọi bằng `d.GioiThieu()`. Phần `(n Nguoi)` trước tên hàm là báo hàm này thuộc về kiểu `Nguoi`, `n` là tên tự đặt cho giá trị đang gọi.


---

## Bài 13. Con trỏ: pointer

Khái niệm mới duy nhất: con trỏ, tức là địa chỉ của hộp, không phải giá trị trong hộp.

Từ tiếng Anh: `pointer` = con trỏ, cái chỉ tới.

Code:

```go
package main

import "fmt"

func main() {
    x := 10
    p := &x
    fmt.Println(*p)
    *p = 20
    fmt.Println(x)
}
```

Bóc:

- `&x`: địa chỉ của hộp `x`. Cậu đã gặp dấu này ở `Scanln(&ten)`, cùng một ý nghĩa: đưa địa chỉ để người khác bỏ giá trị vào đúng hộp.
- `p := &x`: hộp `p` không chứa 10, mà chứa địa chỉ của `x`.
- `*p`: giá trị tại địa chỉ mà `p` đang chỉ tới, tức là 10.
- `*p = 20`: đổi giá trị tại địa chỉ đó thành 20, nên `x` cũng thành 20, vì `p` đang chỉ tới đúng hộp `x`.

Kết quả:

```
10
20
Program exited.
```

Dùng trong hàm để đổi thật, không phải đổi bản copy:

```go
func tang(n *int) {
    *n = *n + 1
}

func main() {
    y := 5
    tang(&y)
    fmt.Println(y) // 6
}
```

- Tham số `n *int` nghĩa là `n` nhận địa chỉ của một hộp số nguyên, không nhận số nguyên trực tiếp.
- Gọi `tang(&y)` là đưa địa chỉ của `y` vào, hàm đổi giá trị tại địa chỉ đó, nên `y` ở ngoài cũng đổi theo.

Đừng nhầm: `*` ở đây không phải phép nhân, nó là "giá trị tại địa chỉ". Ngữ cảnh quyết định nghĩa của dấu, giống như `+` vừa là cộng số, vừa là nối chữ.

---

## Bài 14. Xử lý lỗi

Khái niệm mới duy nhất: lỗi được trả về như một giá trị bình thường, không có `try/catch`.

Từ tiếng Anh:

- `error` = lỗi.
- `nil` = không có gì, rỗng, không lỗi.

Code:

```go
package main

import (
    "errors"
    "fmt"
)

func chia(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("khong chia duoc cho 0!")
    }
    return a / b, nil
}

func main() {
    kq, err := chia(10, 0)
    if err != nil {
        fmt.Println("Loi:", err)
    } else {
        fmt.Println(kq)
    }
}
```

Bóc:

- `func chia(a, b int) (int, error)`: hàm trả về hai giá trị cùng lúc, giá trị thứ nhất là thương, giá trị thứ hai là lỗi. Đây là dạng trả về nhiều giá trị mà file cũ có nhắc tới.
- `return 0, errors.New(...)`: khi `b` bằng 0 thì trả về 0 làm giá trị giả, và một lỗi thật để báo.
- `return a / b, nil`: khi không lỗi thì trả về thương thật, và `nil` nghĩa là không có lỗi.
- `kq, err := chia(10, 0)`: nhận cả hai giá trị trả về vào hai hộp.
- `if err != nil`: nếu lỗi khác `nil`, tức là có lỗi thật, thì in lỗi ra. Ngược lại thì in kết quả.

Quy tắc vàng: luôn kiểm tra `err != nil` ngay sau khi gọi hàm có trả về lỗi, đừng dùng kết quả khi chưa kiểm tra lỗi.

Bỏ qua một giá trị không cần bằng dấu `_`:

```go
_, du := chia(7, 3)
```

Ở đây `_` nghĩa là "tôi không cần giá trị này", chỉ lấy số dư. Trong ví dụ chia ở file cũ, `chia(7, 3)` trả về thương 2 và dư 1, nếu chỉ cần dư thì dùng `_` cho thương.

---

## Bài 15. Goroutine, Channel, WaitGroup: chạy song song

Khái niệm mới: chạy nhiều việc cùng lúc, không chờ nhau theo thứ tự.

Từ tiếng Anh:

- `go` = đi, bắt đầu chạy.
- `goroutine` = một luồng chạy nhẹ của Go.
- `channel` = kênh, ống để goroutine gửi và nhận tin cho nhau.
- `WaitGroup` = nhóm chờ, đợi tất cả việc xong mới đi tiếp.

### 15.1. Goroutine

Chỉ cần thêm chữ `go` trước lời gọi hàm là lời gọi đó chạy song song:

```go
go xinchao("1")
go xinchao("2")
go xinchao("3")
```

Mỗi dòng là một goroutine riêng, chạy cùng hàm `xinchao` với đối số khác nhau. Thứ tự in ra không chắc chắn, có thể là 2, 1, 3, không giống gọi thường theo thứ tự 1, 2, 3.

Ví dụ đầy đủ, có chờ để goroutine kịp in:

```go
package main

import (
    "fmt"
    "time"
)

func dem(ten string) {
    for i := 1; i <= 3; i++ {
        fmt.Println(ten, i)
        time.Sleep(200 * time.Millisecond)
    }
}

func main() {
    go dem("A")
    go dem("B")
    dem("C")
    time.Sleep(time.Second)
}
```

Kết quả: A, B, C in xen kẽ nhau, không chờ nhau. Một goroutine chỉ tốn khoảng 2KB bộ nhớ, nên mở hàng nghìn goroutine vẫn nhẹ, khác với luồng hệ điều hành nặng hơn nhiều.

Điểm phải nhớ, và cũng là lỗi hay gặp nhất: nếu `main` kết thúc ngay sau các dòng `go ...`, chương trình có thể thoát trước khi goroutine kịp in xong. Vì vậy phải có cơ chế chờ, ví dụ `time.Sleep` ở trên, hoặc `WaitGroup` ở dưới cho chắc chắn hơn.

### 15.2. Channel

Channel là ống để goroutine gửi kết quả cho nhau, và việc gửi, nhận sẽ tự đợi nhau.

```go
package main

import "fmt"

func tinh(n int, ch chan int) {
    ch <- n * n
}

func main() {
    ch := make(chan int)

    go tinh(5, ch)
    go tinh(7, ch)

    fmt.Println(<-ch)
    fmt.Println(<-ch)
}
```

Bóc:

- `chan int` là kênh chỉ chở số nguyên.
- `make(chan int)` là tạo một kênh mới.
- `ch <- n * n`: gửi giá trị vào kênh, mũi tên chỉ vào kênh.
- `<-ch`: nhận giá trị từ kênh, mũi tên chỉ ra khỏi kênh.
- Hai lần nhận sẽ ra 25 và 49, nhưng thứ tự nào trước thì không chắc, vì hai goroutine chạy song song.

### 15.3. WaitGroup

`WaitGroup` là cách chờ chắc chắn hơn `Sleep`, đợi tất cả goroutine xong mới đi tiếp:

```go
package main

import (
    "fmt"
    "sync"
)

func viec(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Println("Viec", id, "xong!")
}

func main() {
    var wg sync.WaitGroup
    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go viec(i, &wg)
    }
    wg.Wait()
    fmt.Println("Hoan thanh!")
}
```

Bóc:

- `wg.Add(1)`: cộng thêm một việc cần chờ.
- `go viec(i, &wg)`: chạy việc song song, đưa địa chỉ của `wg` vào để việc đó báo xong được.
- `defer wg.Done()`: `defer` nghĩa là hoãn lại, chạy khi hàm kết thúc. Ở đây là báo xong một việc khi hàm `viec` kết thúc.
- `wg.Wait()`: đứng chờ cho tới khi tất cả việc đã báo xong, rồi mới in `"Hoan thanh!"`.

---

## Bài 16. HTTP Server: backend thật

Khái niệm mới: dùng Go làm máy chủ web, nhận yêu cầu từ trình duyệt và trả lời.

Code chỉ mười dòng chính:

```go
package main

import (
    "fmt"
    "net/http"
)

func chao(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Chao cau! Server Go dang chay!")
}

func main() {
    http.HandleFunc("/", chao)
    fmt.Println("Server chay o :8080")
    http.ListenAndServe(":8080", nil)
}
```

Bóc, ở mức đủ hiểu, không cần hiểu sâu từng kiểu:

- `net/http` là gói làm web của Go, có sẵn, không cần cài thêm.
- `func chao(w http.ResponseWriter, r *http.Request)`: hàm xử lý mỗi yêu cầu tới. `w` là nơi viết câu trả lời ra, `r` là yêu cầu gửi tới.
- `fmt.Fprintln(w, ...)`: in vào `w`, tức là gửi câu trả lời về cho trình duyệt, thay vì in ra màn hình như `fmt.Println`.
- `http.HandleFunc("/", chao)`: đăng ký đường dẫn `/` sẽ do hàm `chao` xử lý.
- `http.ListenAndServe(":8080", nil)`: mở máy chủ ở cổng 8080 và chờ yêu cầu tới.

Chạy chương trình này rồi mở trình duyệt vào `localhost:8080` là thấy câu chào. Đây là bước đầu của backend, sau này mới tới đọc ghi file, cơ sở dữ liệu, và API CRUD.

---

## Bài 17. Tổ chức project và học tiếp

Đây không phải một khái niệm code mới, mà là cách xếp code khi project lớn dần, để cậu không bị ngợp.

- Một chương trình nhỏ thì một file `main.go` là đủ.
- Project lớn hơn thì tách thành nhiều file, nhiều gói. Mỗi gói là một thư mục, có một `package` riêng, không phải lúc nào cũng là `package main`.
- `go mod` là cách khai báo tên module của project và quản lý thư viện ngoài. Khi bắt đầu project thật thì mới cần học kỹ, ở đây chỉ cần biết tên gọi.
- Kiểm thử bằng `go test`: viết hàm kiểm tra xem hàm của mình có trả về đúng kết quả không, thay vì chỉ nhìn bằng mắt. Ví dụ kiểm tra `tong(3, 4)` có bằng 7 không.
- Các chủ đề học sau, theo thứ tự gợi ý: `go mod`, test, `context`, `mutex` (khóa để tránh hai goroutine sửa cùng một hộp cùng lúc), đọc ghi file, cơ sở dữ liệu, rồi tự viết một API nhỏ CRUD danh sách việc cần làm.

Lộ trình tóm tắt từ file cũ, giữ nguyên thứ tự:

1. Cơ bản: các bài 1 tới 11 ở trên.
2. Đọc code thật, xem cách người ta viết.
3. Tự viết một API nhỏ: CRUD danh sách việc cần làm.
4. Học thêm: `go mod`, test, `context`, `mutex`.

Tài liệu chính chủ để tra cứu thêm: `go.dev/tour`, là tour tương tác, có tiếng Việt.

---

## Phụ lục A. Bảng tra dấu

Dùng khi cậu nhìn một dòng code mà không biết dấu đó làm gì. Tìm dấu, đọc một dòng là đủ.

- `:=` tạo hộp mới rồi gán.
- `=` đổi giá trị hộp đã có.
- `==` so sánh bằng, không đổi gì.
- `!=` so sánh khác.
- `<`, `>`, `<=`, `>=` so sánh nhỏ hơn, lớn hơn, nhỏ hơn hoặc bằng, lớn hơn hoặc bằng.
- `+`, `-`, `*`, `/` cộng, trừ, nhân, chia. Với chuỗi thì `+` là nối chữ.
- `()` bao tham số, bao đối số, bao kiểu trả về nhiều giá trị.
- `{}` bao thân hàm, thân `if`, thân `for`, bao danh sách và struct.
- `[]` bao danh sách, bao chỉ số. `[]string` là danh sách chuỗi, `tenList[0]` là món đầu tiên.
- `""` bao chữ trực tiếp.
- `&` địa chỉ của hộp, bắt buộc trong `Scanln`.
- `*` giá trị tại địa chỉ, trong con trỏ. Không nhầm với `*` là phép nhân khi đứng giữa hai số.
- `<-` gửi hoặc nhận qua channel, hướng mũi tên cho biết chiều.
- `_` bỏ qua một giá trị không cần.

---

## Phụ lục B. Bảng tra từ tiếng Anh

Mỗi từ chỉ dịch một nghĩa dùng trong bài này, đủ để cậu đọc code mà không bị rối.

- `package` = gói.
- `import` = mượn.
- `func` / `function` = hàm.
- `main` = chính.
- `variable` / `var` = biến.
- `string` = chuỗi, chữ.
- `integer` / `int` = số nguyên.
- `float64` = số thực, số có phần thập phân.
- `bool` = đúng hoặc sai, `true` / `false`.
- `length` / `len` = độ dài.
- `if` = nếu.
- `else` = nếu không thì.
- `for` = lặp.
- `range` = duyệt từng món.
- `return` = trả về.
- `parameter` = tham số, chỗ trống lúc viết hàm.
- `argument` = đối số, giá trị thật lúc gọi hàm.
- `append` = thêm vào cuối.
- `struct` / `structure` = cấu trúc, gom dữ liệu.
- `pointer` = con trỏ, địa chỉ.
- `error` = lỗi.
- `nil` = không có gì, không lỗi.
- `channel` = kênh.
- `compute` = tính toán.
- `contains` = chứa, có chứa hay không.

---

## Phụ lục C. Lỗi hay gặp và cách sửa nhanh

- `declared and not used`: hộp khai báo rồi mà không dùng, hoặc tên dùng lại gõ sai hoa thường. Kiểm tra lại tên hộp ở mọi chỗ.
- `undefined`: tên chưa khai báo, hoặc hàm chưa định nghĩa. Kiểm tra xem hàm đó có viết ở ngoài `main` chưa, tên có gõ đúng không.
- `unexpected newline, expected { after if clause`: dấu `{` của `if` bị xuống dòng. Đưa `{` lên cùng dòng với điều kiện.
- `else` bị mồ côi: viết `}` xuống dòng rồi mới viết `else` ở dòng sau. Viết `} else {` trên một dòng.
- Vòng lặp chạy mãi không dừng: quên dòng tăng hoặc giảm biến đếm. Thêm `dem = dem + 1` hoặc `dem = dem - 1` cho đúng hướng.
- Gọi hàm mà không thấy kết quả: hàm trả về nhưng cậu không cất vào hộp, không in ra. Thêm `ketQua := ...` rồi in, hoặc in thẳng `fmt.Println(...)`.
- `Scanln` không chờ gõ trên Playground: đó là giới hạn của Playground, không phải lỗi code của cậu. Đọc hiểu bài đó trước, gõ thật trên máy có cài Go sau.
- Cắt chuỗi vượt giới hạn: chỉ số kết thúc lớn hơn độ dài chuỗi. Kiểm tra lại `len` trước khi cắt.
- Chia số nguyên ra số lẻ bị mất: đó là tính chất của `int`, không phải lỗi. Muốn có phần thập phân thì dùng `float64`.

---

*Viết cho Hoàng Dươngg, học chậm mà chắc, mỗi bài một khái niệm, bóc từng dòng rồi mới sang bài tiếp theo.*
