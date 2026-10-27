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

---

## Phần bổ sung: các bài nâng cao viết đầy đủ

Phần này viết tiếp theo yêu cầu của Hoàng Dươngg: viết hết, đầy đủ, giải thích chi tiết, dễ hiểu. Mỗi bài dưới đây vẫn giữ nguyên nhịp: một khái niệm mới là chính, dịch từ tiếng Anh, bóc từng dòng, có kết quả chạy, nói rõ phần nào tự đổi được.

---

## Bài 18. Switch: rẽ nhánh theo nhiều giá trị

Khái niệm mới duy nhất: `switch`, xét một giá trị với nhiều trường hợp, gọn hơn viết nhiều `if else if`.

Từ tiếng Anh:

- `switch` = chuyển, chuyển theo.
- `case` = trường hợp.
- `default` = mặc định, trường hợp còn lại.

Code:

```go
package main

import "fmt"

func main() {
    thu := 3
    switch thu {
    case 2:
        fmt.Println("Thu hai")
    case 3:
        fmt.Println("Thu ba")
    default:
        fmt.Println("Ngay khac")
    }
}
```

Bóc:

- `switch thu`: lấy giá trị trong hộp `thu` ra để so sánh.
- `case 2:`: nếu `thu` bằng 2 thì chạy dòng dưới nó.
- `case 3:`: nếu `thu` bằng 3 thì chạy dòng dưới nó. Ở đây `thu` là 3 nên in ra `Thu ba`.
- `default:`: nếu không khớp trường hợp nào thì chạy ở đây.
- Khác với nhiều ngôn ngữ khác, Go không cần viết `break` sau mỗi `case`. Chạy xong một `case` là tự dừng, không chạy tràn sang `case` sau. Muốn chạy tràn cố tình thì mới dùng từ khóa `fallthrough`, rất hiếm khi cần, ở đây không cần nhớ vội.

Kết quả:

```
Thu ba
Program exited.
```

Dạng `switch` không có giá trị, tương đương viết nhiều `if`:

```go
diem := 8
switch {
case diem >= 9:
    fmt.Println("Gioi!")
case diem >= 5:
    fmt.Println("Dat")
default:
    fmt.Println("Chua dat")
}
```

Ở dạng này, mỗi `case` là một điều kiện, máy xét từ trên xuống dưới, gặp điều kiện đúng đầu tiên thì chạy rồi dừng.

Tự đổi được: giá trị `thu`, các số trong `case`, câu in ra.
Bắt buộc: dấu `:` sau mỗi `case`, từ `default` nếu muốn có trường hợp còn lại.

---

## Bài 19. Hằng số, Bool, Số thực và ép kiểu

Bài này gom các kiểu dữ liệu cơ bản còn lại, mỗi kiểu chỉ một ví dụ ngắn, để cậu tra cứu khi gặp.

### Hằng số

Từ tiếng Anh: `const` = viết tắt của `constant` = hằng số, không đổi được.

```go
const PI = 3.14
fmt.Println(PI)
```

Khai báo bằng `const` thì sau đó không được gán lại. Viết `PI = 3.15` sau đó là lỗi. Biến thì đổi được, hằng thì không.

### Bool

`bool` là kiểu đúng hoặc sai, chỉ có hai giá trị: `true` (đúng) và `false` (sai).

```go
dangHoc := true
if dangHoc {
    fmt.Println("Dang hoc Go")
}
```

`if` nhận trực tiếp một giá trị `bool`, không cần viết `dangHoc == true`, vì bản thân nó đã là đúng hoặc sai rồi.

### Số thực float64

`float64` là số có phần thập phân, ví dụ 3.14, 2.5, -0.5. Số 64 là độ chính xác, ở đây không cần đào sâu, chỉ cần biết tên gọi.

```go
a := 7.0
b := 2.0
fmt.Println(a / b) // 3.5
```

Chú ý: `7 / 2` với `int` ra 3, nhưng `7.0 / 2.0` với `float64` ra 3.5. Kiểu quyết định kết quả chia.

### Ép kiểu

Ép kiểu là đổi một giá trị từ kiểu này sang kiểu khác, viết tên kiểu trước rồi bao giá trị trong ngoặc tròn:

```go
x := 7
y := 2
fmt.Println(float64(x) / float64(y)) // 3.5
fmt.Println(int(3.9)) // 3
```

- `float64(x)` đổi số nguyên 7 thành số thực 7.0.
- `int(3.9)` đổi số thực 3.9 thành số nguyên 3, bỏ phần thập phân đi, không làm tròn lên.

Lỗi hay gặp: cộng `int` với `float64` trực tiếp mà không ép kiểu, máy sẽ báo kiểu không khớp. Phải ép một bên về cùng kiểu với bên còn lại trước.

---

## Bài 20. Interface: hợp đồng chung

Khái niệm mới duy nhất: `interface`, tức là một hợp đồng: kiểu nào có đủ các hàm trong hợp đồng thì đều dùng được chung một chỗ.

Từ tiếng Anh: `interface` = giao diện, hợp đồng.

Ví dụ đời thường: ổ cắm điện là một hợp đồng. Quạt, đèn, sạc điện thoại đều cắm được vào cùng một ổ, vì chúng đều có chung một chuẩn cắm, dù bên trong mỗi thứ hoạt động khác nhau.

Code:

```go
package main

import "fmt"

type Keu interface {
    KeuLen() string
}

type Cho struct {
    Ten string
}

func (c Cho) KeuLen() string {
    return c.Ten + " gau gau"
}

type Meo struct {
    Ten string
}

func (m Meo) KeuLen() string {
    return m.Ten + " meo meo"
}

func choKeu(k Keu) {
    fmt.Println(k.KeuLen())
}

func main() {
    choKeu(Cho{Ten: "Vang"})
    choKeu(Meo{Ten: "Mun"})
}
```

Bóc, chậm từng bước:

- `type Keu interface { KeuLen() string }`: định nghĩa hợp đồng tên `Keu`, hợp đồng này chỉ yêu cầu một điều: phải có hàm `KeuLen()` trả về chuỗi.
- `Cho` và `Meo` là hai struct khác nhau, nhưng cả hai đều có hàm `KeuLen()` đúng chữ ký như hợp đồng, nên cả hai đều tự động thỏa hợp đồng `Keu`, không cần khai báo thêm là "tôi implements interface này".
- `func choKeu(k Keu)`: hàm này nhận bất cứ giá trị nào thỏa hợp đồng `Keu`, không quan tâm nó là chó hay mèo, chỉ cần nó biết kêu lên.
- Gọi `choKeu` với chó thì in ra chó kêu, gọi với mèo thì in ra mèo kêu, cùng một hàm, hai kiểu khác nhau.

Kết quả:

```
Vang gau gau
Mun meo meo
Program exited.
```

Interface rỗng, `any`, là hợp đồng không yêu cầu gì cả, nên kiểu nào cũng thỏa:

```go
func inBatCu(x any) {
    fmt.Println(x)
}
```

`any` là tên mới của `interface{}`. Dùng khi thật sự cần nhận bất cứ kiểu gì, nhưng dùng nhiều quá thì mất kiểm tra kiểu, nên chỉ dùng khi cần thiết.

Tự đổi được: tên interface, tên struct, tên hàm trong hợp đồng, miễn là ba chỗ phải khớp nhau: định nghĩa hợp đồng, hàm của struct, và chỗ gọi.
Bắt buộc: chữ ký hàm trong struct phải giống hệt chữ ký trong hợp đồng, cả tên hàm, tham số và kiểu trả về.

---

## Bài 21. Generics: hàm dùng được nhiều kiểu

Khái niệm mới duy nhất: generics, viết một hàm mà dùng được cho nhiều kiểu, thay vì viết lại hàm giống nhau cho từng kiểu.

Từ tiếng Anh: `generic` = chung chung, dùng chung cho nhiều kiểu.

Vấn đề trước khi có generics: muốn cộng hai số nguyên thì viết hàm cho `int`, muốn cộng hai số thực thì phải viết lại hàm y hệt cho `float64`, rất lặp lại.

Code:

```go
package main

import "fmt"

func cong[T int | float64](a, b T) T {
    return a + b
}

func main() {
    fmt.Println(cong(3, 4))       // 7
    fmt.Println(cong(2.5, 3.5))   // 6
}
```

Bóc:

- `[T int | float64]`: khai báo một kiểu chung tên `T`, `T` tự đặt, giống như tên tham số. Dấu `|` nghĩa là "hoặc": `T` có thể là `int` hoặc `float64`.
- `(a, b T)`: cả hai tham số đều kiểu `T`, tức là cùng một kiểu trong một lần gọi, không được một bên `int` một bên `float64` trong cùng một lần gọi.
- `T` ở sau dấu `)` là kiểu trả về, cũng là `T`, tức là trả về cùng kiểu với đầu vào.
- Gọi `cong(3, 4)`: máy tự hiểu `T` là `int`, trả về `int` là 7.
- Gọi `cong(2.5, 3.5)`: máy tự hiểu `T` là `float64`, trả về `float64` là 6.

Kết quả:

```
7
6
Program exited.
```

Generics cũng dùng được cho struct và danh sách, ví dụ một hộp đựng bất cứ kiểu gì:

```go
type Hop[T any] struct {
    GiaTri T
}
```

Ở đây `Hop[int]` là hộp đựng số nguyên, `Hop[string]` là hộp đựng chuỗi, cùng một định nghĩa mà dùng được nhiều kiểu.

Lời khuyên: ở giai đoạn này chỉ cần hiểu generics là "viết một lần, dùng nhiều kiểu", đừng cố dùng generics cho mọi hàm. Hàm chỉ dùng một kiểu thì viết bình thường cho dễ đọc, khi nào thấy mình viết lại cùng một hàm cho nhiều kiểu thì mới nghĩ tới generics.


---

## Bài 22. Đọc và ghi file

Khái niệm mới duy nhất: chương trình đọc và ghi file trên máy, thay vì chỉ in ra màn hình và biến mất khi tắt chương trình.

Từ tiếng Anh:

- `os` = operating system, hệ điều hành, gói làm việc với file và hệ thống.
- `ReadFile` = đọc file.
- `WriteFile` = ghi file.

Code ghi rồi đọc lại:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    noiDung := "Xin chao Capybara"
    err := os.WriteFile("chao.txt", []byte(noiDung), 0644)
    if err != nil {
        fmt.Println("Loi ghi:", err)
        return
    }

    doc, err := os.ReadFile("chao.txt")
    if err != nil {
        fmt.Println("Loi doc:", err)
        return
    }
    fmt.Println(string(doc))
}
```

Bóc:

- `os.WriteFile("chao.txt", []byte(noiDung), 0644)`: ghi vào file tên `chao.txt`. Tham số thứ hai là nội dung, phải đổi chuỗi thành danh sách byte bằng `[]byte(...)`, vì file lưu theo byte, không lưu trực tiếp theo chuỗi. Tham số thứ ba `0644` là quyền của file, ở đây chỉ cần nhớ số này là quyền đọc ghi bình thường, chưa cần học sâu về quyền file.
- `err := os.WriteFile(...)`: hàm ghi file trả về lỗi, nếu ghi được thì lỗi là `nil`, nếu không ghi được thì lỗi khác `nil`. Kiểm tra lỗi ngay sau đó, giống Bài 14.
- `return` trong `main` ở đây là dừng chương trình luôn, vì ghi lỗi thì đọc tiếp cũng vô nghĩa.
- `os.ReadFile("chao.txt")`: đọc toàn bộ file về, trả về danh sách byte và lỗi.
- `string(doc)`: đổi danh sách byte đọc được trở lại thành chuỗi để in ra.

Kết quả nếu chạy trên máy thật:

```
Xin chao Capybara
```

Chú ý về Go Playground: Playground không có file thật trên máy cậu để ghi và đọc như trên, nên bài này đọc hiểu trước, chạy thật trên máy có cài Go hoặc Termux sau. Đường dẫn file ở ví dụ trên là đường dẫn tương đối, tức là file nằm cùng thư mục với chương trình đang chạy.

Đọc từng dòng với `bufio`, dùng khi file lớn, không muốn đọc hết một lần vào bộ nhớ:

```go
file, err := os.Open("chao.txt")
if err != nil {
    fmt.Println("Loi mo:", err)
    return
}
defer file.Close()
```

- `os.Open` mở file để đọc, khác `ReadFile` là đọc hết một lần.
- `defer file.Close()`: `defer` nghĩa là hoãn lại, câu này sẽ chạy khi hàm kết thúc, ở đây là đóng file lại khi đọc xong, để không bị rò rỉ file mở. Quên đóng file thì lâu dài máy sẽ hết chỗ mở file.

---

## Bài 23. JSON: định dạng trao đổi dữ liệu

Khái niệm mới duy nhất: JSON, định dạng chữ để chương trình này gửi dữ liệu cho chương trình khác, đặc biệt là cho API và web.

Từ tiếng Anh: `JSON` = JavaScript Object Notation, ký hiệu đối tượng của JavaScript, nhưng nay mọi ngôn ngữ đều dùng.

Ví dụ một giá trị JSON:

```json
{"ten": "Duongg", "tuoi": 18}
```

Trong Go, struct và JSON đi với nhau bằng thẻ `json`:

```go
package main

import (
    "encoding/json"
    "fmt"
)

type Nguoi struct {
    Ten  string `json:"ten"`
    Tuoi int    `json:"tuoi"`
}

func main() {
    d := Nguoi{Ten: "Duongg", Tuoi: 18}

    chuoi, err := json.Marshal(d)
    if err != nil {
        fmt.Println("Loi:", err)
        return
    }
    fmt.Println(string(chuoi))

    var d2 Nguoi
    err = json.Unmarshal([]byte(`{"ten": "Capybara", "tuoi": 3}`), &d2)
    if err != nil {
        fmt.Println("Loi:", err)
        return
    }
    fmt.Println(d2.Ten, d2.Tuoi)
}
```

Bóc:

- `` `json:"ten"` `` là thẻ, viết trong dấu backtick, ngay sau kiểu của trường. Thẻ này báo: khi đổi sang JSON thì trường `Ten` sẽ thành khóa `"ten"` viết thường, không phải `"Ten"`.
- `json.Marshal(d)`: `Marshal` nghĩa là đóng gói, đổi struct thành danh sách byte JSON.
- `string(chuoi)`: đổi danh sách byte JSON thành chuỗi để in ra.
- `json.Unmarshal(...)`: `Unmarshal` nghĩa là mở gói, đổi JSON thành struct. Tham số thứ hai phải là địa chỉ của hộp nhận, nên viết `&d2`, giống dấu `&` trong `Scanln`.
- Dấu backtick quanh chuỗi JSON ở ví dụ là chuỗi thô, trong đó dấu ngoặc kép không cần thoát, dùng cho tiện khi viết JSON mẫu trong code.

Kết quả:

```
{"ten":"Duongg","tuoi":18}
Capybara 3
Program exited.
```

Chú ý: trường trong struct muốn xuất ra JSON thì tên trường phải viết hoa chữ đầu, ví dụ `Ten`, `Tuoi`. Viết thường `ten`, `tuoi` thì gói `json` sẽ không nhìn thấy trường đó và bỏ qua.

---

## Bài 24. Go mod và nhiều file, nhiều gói

Bài này không có code chạy mới phức tạp, mà là cách xếp project cho đúng, để cậu không bị rối khi project lớn lên.

- `go mod init ten-module`: lệnh tạo file `go.mod`, khai báo tên module của project. Ví dụ trong thư mục project, chạy `go mod init vidu` thì tạo ra file `go.mod` ghi tên module là `vidu`.
- File `go.mod` còn ghi phiên bản Go và thư viện ngoài project đang dùng. Khi cậu dùng thư viện ngoài, chạy `go get ten-thu-vien` thì `go.mod` tự cập nhật.
- Một project có nhiều file `.go` cùng một thư mục thì các file đó phải cùng một `package`, trừ file `main.go` thuộc `package main`. Các hàm ở file này gọi được hàm ở file khác cùng gói mà không cần import thêm, vì chúng cùng một gói.
- Muốn dùng gói khác trong cùng project thì `import` theo đường dẫn module, ví dụ `import "vidu/tinh"`, trong đó `vidu` là tên module, `tinh` là tên thư mục gói.
- Quy tắc tên: tên hàm, tên kiểu, tên trường viết hoa chữ đầu thì gói khác dùng được, gọi là exported, xuất ra ngoài. Viết thường chữ đầu thì chỉ dùng trong cùng gói, gọi là unexported. Giống như trường `Ten` xuất được ra JSON, trường `ten` thì không, cùng một nguyên tắc: hoa là mở, thường là đóng.
- Cấu trúc project nhỏ gợi ý:

```
myapp/
  go.mod
  main.go
  tinh/
    tong.go
```

Trong đó `main.go` thuộc `package main`, gọi hàm trong gói `tinh`, còn `tong.go` thuộc `package tinh`.

Lỗi hay gặp: hai file cùng thư mục mà khai báo `package` khác nhau, máy sẽ báo gói không khớp. Sửa bằng cách cho cùng một `package` hoặc tách thư mục ra.

---

## Bài 25. Kiểm thử: go test

Khái niệm mới duy nhất: test, viết chương trình kiểm tra chương trình của mình, thay vì chỉ nhìn bằng mắt.

Từ tiếng Anh: `test` = kiểm tra, thử.

File code chính, ví dụ `tong.go`:

```go
package tinh

func Tong(a, b int) int {
    return a + b
}
```

File test, tên phải kết thúc bằng `_test.go`, ví dụ `tong_test.go`, cùng thư mục, cùng gói:

```go
package tinh

import "testing"

func TestTong(t *testing.T) {
    ketQua := Tong(3, 4)
    if ketQua != 7 {
        t.Errorf("Tong(3, 4) = %d, muon 7", ketQua)
    }
}
```

Bóc:

- `import "testing"`: mượn gói kiểm thử có sẵn của Go.
- `func TestTong(t *testing.T)`: hàm test phải bắt đầu bằng `Test`, chữ `T` viết hoa, tham số là `*testing.T`, tức là địa chỉ của đối tượng kiểm thử.
- `t.Errorf(...)`: báo test thất bại, in ra thông báo. `%d` là chỗ để điền số nguyên vào, giống như in có định dạng.
- Chạy test bằng lệnh `go test ./...` trong thư mục project, dấu `./...` nghĩa là test tất cả các gói trong project.
- Kết quả khi đúng: `ok`, khi sai: `FAIL` kèm thông báo cậu viết trong `Errorf`.

Lợi ích: sau này cậu sửa code, chỉ cần chạy lại `go test` là biết ngay chỗ nào bị hỏng, không cần chạy tay từng ví dụ một. Test tốt nhất là test các trường hợp biên: số 0, số âm, số lớn, và trường hợp lỗi như chia cho 0.

Bảng test, viết gọn nhiều trường hợp trong một hàm test, học sau dạng test đơn cũng được:

```go
func TestTongBang(t *testing.T) {
    cacTruongHop := []struct {
        a, b, muon int
    }{
        {3, 4, 7},
        {0, 0, 0},
        {-1, 1, 0},
    }
    for _, tc := range cacTruongHop {
        ketQua := Tong(tc.a, tc.b)
        if ketQua != tc.muon {
            t.Errorf("Tong(%d, %d) = %d, muon %d", tc.a, tc.b, ketQua, tc.muon)
        }
    }
}
```

Ở đây `_, tc := range ...`: dấu `_` bỏ qua chỉ số, chỉ lấy giá trị từng trường hợp, giống dấu `_` bỏ qua giá trị không cần ở Bài 14.


---

## Bài 26. Context: hủy việc và giới hạn thời gian

Khái niệm mới duy nhất: `context`, tức là bối cảnh của một việc, dùng để báo hủy hoặc giới hạn thời gian cho việc đó.

Từ tiếng Anh: `context` = bối cảnh, ngữ cảnh.

Ví dụ đời thường: cậu nhờ bạn đi mua đồ, nhưng dặn trước là "quá 10 phút mà chưa mua được thì thôi, quay về". Cái dặn đó chính là context có thời hạn.

Code:

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func lamViec(ctx context.Context) {
    select {
    case <-time.After(2 * time.Second):
        fmt.Println("Lam xong")
    case <-ctx.Done():
        fmt.Println("Bi huy:", ctx.Err())
    }
}

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 1*time.Second)
    defer cancel()
    lamViec(ctx)
}
```

Bóc, chậm từng phần:

- `context.Background()`: bối cảnh gốc, trống, không có thời hạn, không có hủy. Đây là điểm bắt đầu, thường dùng ở `main`.
- `context.WithTimeout(..., 1*time.Second)`: tạo bối cảnh mới có thời hạn 1 giây, dựa trên bối cảnh gốc. Trả về hai thứ: bối cảnh mới `ctx`, và hàm hủy `cancel`.
- `defer cancel()`: hoãn việc gọi `cancel` tới khi hàm `main` kết thúc, để giải phóng tài nguyên, dù việc đã xong hay đã hết hạn.
- `ctx.Done()`: một kênh, khi bối cảnh bị hủy hoặc hết hạn thì kênh này sẽ đóng lại, bên nhận sẽ biết là phải dừng.
- `select`: chọn nhánh nào sẵn sàng trước thì chạy nhánh đó, giống như đứng ở ngã ba, đường nào mở trước thì đi đường đó.
- Ở ví dụ này, việc cần 2 giây mới xong, nhưng thời hạn chỉ 1 giây, nên nhánh `ctx.Done()` thắng trước, in ra bị hủy, không chờ tới 2 giây.

Kết quả:

```
Bi huy: context deadline exceeded
Program exited.
```

`context deadline exceeded` nghĩa là đã quá thời hạn.

Các dạng context thường dùng:

- `context.Background()`: bối cảnh gốc ở `main` hoặc ở test.
- `context.TODO()`: bối cảnh tạm, dùng khi chưa biết dùng bối cảnh nào, sau này thay bằng bối cảnh thật.
- `context.WithCancel`: tạo bối cảnh có thể hủy bằng tay bằng hàm `cancel`, không có thời hạn tự động.
- `context.WithTimeout`: tạo bối cảnh có thời hạn, hết hạn thì tự hủy.
- `context.WithValue`: gắn thêm một giá trị vào bối cảnh, ví dụ mã người dùng, để hàm sâu bên trong lấy ra dùng mà không cần truyền thêm tham số.

Quy tắc quan trọng: `context.Context` luôn là tham số đầu tiên của hàm, không bao giờ cất nó vào struct. Truyền context từ trên xuống dưới, hàm nào nhận được thì truyền tiếp xuống hàm con.

---

## Bài 27. Mutex: khóa để không sửa cùng lúc

Khái niệm mới duy nhất: `mutex`, tức là khóa, để hai goroutine không sửa cùng một hộp cùng lúc.

Từ tiếng Anh: `mutex` = viết tắt của `mutual exclusion` = loại trừ lẫn nhau, chỉ một người được vào một lúc.

Ví dụ đời thường: một nhà vệ sinh chỉ có một người vào được một lúc, người vào thì khóa cửa lại, ra thì mở cửa cho người tiếp theo. Mutex cũng vậy, chỉ một goroutine được giữ khóa một lúc.

Code:

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var mu sync.Mutex
    dem := 0

    var wg sync.WaitGroup
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            mu.Lock()
            dem = dem + 1
            mu.Unlock()
        }()
    }
    wg.Wait()
    fmt.Println(dem)
}
```

Bóc:

- `var mu sync.Mutex`: tạo một cái khóa tên `mu`.
- `mu.Lock()`: khóa lại, goroutine nào tới trước thì giữ khóa, goroutine tới sau phải đứng chờ ở dòng này cho tới khi khóa được mở.
- `dem = dem + 1`: đoạn này nằm giữa khóa và mở khóa, gọi là đoạn quan trọng, chỉ một goroutine được chạy đoạn này một lúc, nên `dem` không bị hai goroutine đọc và ghi đè lên nhau.
- `mu.Unlock()`: mở khóa ra, cho goroutine tiếp theo vào.
- Nếu không có khóa, 1000 goroutine cùng tăng `dem` thì kết quả có thể nhỏ hơn 1000, vì hai goroutine có thể cùng đọc `dem` là 500, cùng cộng 1 thành 501, rồi cùng ghi 501, mất mất một lần tăng. Đây gọi là race condition, điều kiện đua nhau.
- `go func() { ... }()`: đây là hàm ẩn danh chạy ngay dưới dạng goroutine, tức là định nghĩa hàm và gọi nó ngay trong cùng một dòng, không cần đặt tên hàm riêng.

Kết quả đúng khi có khóa:

```
1000
Program exited.
```

Cách kiểm tra race: chạy test với lệnh `go test -race ./...`, máy sẽ báo chỗ nào có race, để cậu sửa bằng mutex hoặc bằng channel.

Một lựa chọn khác: thay vì dùng mutex để bảo vệ hộp chung, có thể dùng channel để chỉ một goroutine duy nhất được phép sửa hộp đó, các goroutine khác gửi yêu cầu qua channel. Nguyên tắc của Go là: đừng giao tiếp bằng cách chia sẻ bộ nhớ, hãy chia sẻ bộ nhớ bằng cách giao tiếp. Ở giai đoạn này chỉ cần nhớ câu đó, chưa cần áp dụng hoàn hảo.

---

## Bài 28. Defer, Panic, Recover

Ba từ này đi chung với nhau, nhưng mỗi từ là một khái niệm nhỏ, học chung một bài cho tiện.

### Defer

`defer` nghĩa là hoãn lại, câu lệnh có `defer` sẽ chạy khi hàm kết thúc, dù hàm kết thúc bình thường hay vì lỗi.

```go
func main() {
    defer fmt.Println("Chay cuoi cung")
    fmt.Println("Chay dau tien")
}
```

Kết quả in ra ngược thứ tự viết: `Chay dau tien` trước, `Chay cuoi cung` sau, vì câu có `defer` bị hoãn tới cuối hàm.

Dùng nhiều nhất cho: đóng file sau khi mở, mở khóa mutex sau khi khóa, gọi `cancel` của context sau khi tạo, báo `Done` của WaitGroup sau khi bắt đầu việc.

Nếu có nhiều `defer` trong một hàm thì chúng chạy theo thứ tự ngược lại, cái `defer` sau cùng chạy trước, giống như xếp chồng đĩa, lấy đĩa trên cùng trước.

### Panic

`panic` nghĩa là hoảng loạn, chương trình gặp lỗi không thể tiếp tục được thì tự dừng và in ra lỗi.

```go
panic("co loi nghiem trong")
```

Ví dụ hay gặp nhất: truy cập danh sách vượt chỉ số, ví dụ `tenList[10]` khi danh sách chỉ có 3 món, máy sẽ tự `panic` và dừng chương trình.

### Recover

`recover` nghĩa là hồi phục, bắt lại cơn `panic` để chương trình không chết hẳn, chỉ dùng trong hàm có `defer`.

```go
func anToan() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Bat duoc panic:", r)
        }
    }()
    panic("co loi")
    fmt.Println("Dong nay khong chay")
}
```

Bóc:

- Hàm ẩn danh có `defer` sẽ chạy khi hàm `anToan` kết thúc, kể cả khi đang `panic`.
- Trong hàm `defer` đó, `recover()` lấy lại giá trị `panic` đã ném ra. Nếu không có `panic` thì `recover()` trả về `nil`.
- Nếu bắt được `panic` thì chương trình không chết, chỉ in ra thông báo rồi tiếp tục ở hàm gọi.

Quy tắc: đừng dùng `panic` thay cho lỗi bình thường. Lỗi bình thường thì trả về `error` như Bài 14, chỉ `panic` khi lỗi thật sự không thể tiếp tục được, ví dụ cấu hình sai ngay từ đầu.

---

## Bài 29. Gói lỗi: bọc lỗi và kiểm tra lỗi

Bài này là phần nâng cao của Bài 14, học sau khi đã quen với `error` cơ bản.

### Bọc lỗi bằng fmt.Errorf và %w

`fmt.Errorf` giống `fmt.Println` nhưng tạo ra một lỗi, không in ra. `%w` là chỗ để bọc một lỗi khác vào trong lỗi mới, giữ lại lỗi gốc để sau này kiểm tra được.

```go
import "fmt"

func docFile() error {
    _, err := os.ReadFile("khong-co.txt")
    if err != nil {
        return fmt.Errorf("khong doc duoc file: %w", err)
    }
    return nil
}
```

Khi in lỗi này ra, cậu sẽ thấy cả hai lớp: lỗi ngoài là "không đọc được file", lỗi trong là lỗi gốc của hệ điều hành, ví dụ file không tồn tại.

### Kiểm tra lỗi bằng errors.Is và errors.As

- `errors.Is(err, loiMucTieu)`: kiểm tra xem lỗi này, hoặc lỗi được bọc trong nó, có phải là lỗi mục tiêu không. Dùng khi cậu muốn biết lỗi có phải là một loại cụ thể không, ví dụ file không tồn tại.
- `errors.As(err, &loiCuThe)`: lấy lỗi cụ thể ra khỏi lớp bọc, để đọc thêm thông tin trong lỗi đó.

```go
import "errors"

if errors.Is(err, os.ErrNotExist) {
    fmt.Println("File khong ton tai")
}
```

`os.ErrNotExist` là lỗi mẫu của hệ điều hành cho trường hợp file không tồn tại. Dùng `errors.Is` thì dù lỗi đã bị bọc nhiều lớp bằng `%w`, cậu vẫn kiểm tra được đúng loại.

---

## Bài 30. Cơ sở dữ liệu: khái niệm và CRUD

Bài này giải thích khái niệm trước, code minh họa bằng bộ nhớ trước, rồi mới tới cơ sở dữ liệu thật, để cậu không bị ngợp bởi driver và câu lệnh SQL cùng lúc.

### CRUD là gì

CRUD là bốn việc cơ bản với dữ liệu, viết tắt của bốn từ tiếng Anh:

- `Create` = tạo mới.
- `Read` = đọc ra.
- `Update` = cập nhật.
- `Delete` = xóa đi.

Mọi ứng dụng quản lý danh sách, từ danh sách việc cần làm tới danh sách người dùng, đều xoay quanh bốn việc này.

### CRUD bằng danh sách trong bộ nhớ, qua HTTP

Đây là project nhỏ gợi ý ở Bài 17, dùng lại struct, slice, HTTP server và JSON, không cần cơ sở dữ liệu thật mà vẫn chạy được:

```go
type Viec struct {
    ID    int    `json:"id"`
    Ten   string `json:"ten"`
    Xong  bool   `json:"xong"`
}

var danhSach []Viec
var idTiepTheo = 1
```

- Create: nhận JSON từ trình duyệt, tạo một `Viec` mới, gán `ID` tăng dần, `append` vào danh sách.
- Read: trả danh sách về dưới dạng JSON, hoặc trả một việc theo `ID`.
- Update: tìm việc theo `ID`, đổi trường `Ten` hoặc `Xong` rồi ghi lại vào danh sách.
- Delete: tìm việc theo `ID`, xóa khỏi danh sách bằng cách cắt slice: `danhSach = append(danhSach[:i], danhSach[i+1:]...)`.

Dấu `...` ở cuối trong ví dụ xóa nghĩa là "trải danh sách ra thành từng món riêng", để `append` nhận từng món, không nhận cả danh sách làm một món.

Nhược điểm của bộ nhớ: tắt chương trình là mất hết dữ liệu, vì danh sách chỉ nằm trong RAM. Muốn giữ lại sau khi tắt thì phải lưu xuống file như Bài 22, hoặc lưu xuống cơ sở dữ liệu như dưới đây.

### Cơ sở dữ liệu thật với database/sql

Gói `database/sql` của Go không tự nói chuyện trực tiếp với một loại cơ sở dữ liệu cụ thể, nó cần một driver, tức là trình điều khiển, cho loại đó. Ví dụ driver cho Postgres khác driver cho MySQL, khác driver cho SQLite, nhưng cách dùng `database/sql` thì giống nhau.

Các bước cơ bản, ở mức khái niệm:

```go
db, err := sql.Open("postgres", "chuoi-ket-noi")
if err != nil {
    fmt.Println("Loi mo DB:", err)
    return
}
defer db.Close()
```

- `sql.Open` mở kết nối, nhưng chưa kiểm tra thật là kết nối được hay không, muốn kiểm tra thật thì gọi `db.Ping()`.
- Create: `db.Exec("INSERT INTO viec (ten, xong) VALUES ($1, $2)", ten, xong)`. Dấu `$1`, `$2` là chỗ trống cho giá trị, để tránh lỗi bảo mật khi người dùng gõ chữ đặc biệt vào, không được nối chuỗi trực tiếp vào câu SQL.
- Read: `db.Query("SELECT id, ten, xong FROM viec")`, rồi duyệt từng dòng kết quả bằng `rows.Next()` và `rows.Scan(&id, &ten, &xong)`.
- Update: `db.Exec("UPDATE viec SET xong = $1 WHERE id = $2", true, id)`.
- Delete: `db.Exec("DELETE FROM viec WHERE id = $2", id)`.

Lưu ý: câu SQL trong ví dụ dùng `$1`, `$2` là kiểu của Postgres. MySQL dùng dấu `?` thay cho `$1`, `$2`. Đổi loại cơ sở dữ liệu thì đổi driver và đổi chỗ trống, còn logic CRUD thì giữ nguyên.

Khi nào dùng file, khi nào dùng cơ sở dữ liệu: dữ liệu ít, một người dùng, không cần tìm kiếm phức tạp thì file là đủ. Dữ liệu nhiều, nhiều người dùng cùng lúc, cần tìm kiếm, lọc, sắp xếp nhanh và an toàn thì dùng cơ sở dữ liệu.

---

## Phụ lục D. Bảng tra nhanh các bài bổ sung

- `switch` / `case` / `default`: rẽ nhánh theo nhiều giá trị, Go không cần `break`.
- `const`: hằng số, không gán lại được.
- `bool`: đúng hoặc sai.
- `float64`: số thực.
- Ép kiểu: `float64(x)`, `int(3.9)`.
- `interface`: hợp đồng, kiểu nào có đủ hàm trong hợp đồng thì dùng chung được.
- `any`: interface rỗng, nhận bất cứ kiểu gì.
- Generics `[T int | float64]`: viết một lần, dùng nhiều kiểu.
- `os.ReadFile` / `os.WriteFile`: đọc và ghi file hết một lần.
- `defer file.Close()`: đóng file khi hàm kết thúc.
- `json.Marshal` / `json.Unmarshal`: đóng gói và mở gói JSON.
- `go mod init`: tạo module, `go test ./...`: chạy tất cả test.
- `context.WithTimeout` / `WithCancel`: bối cảnh có thời hạn hoặc có thể hủy.
- `sync.Mutex` / `Lock` / `Unlock`: khóa để không sửa cùng lúc.
- `defer`: hoãn tới cuối hàm. `panic`: dừng vì lỗi nghiêm trọng. `recover`: bắt lại panic trong defer.
- `fmt.Errorf` với `%w`: bọc lỗi. `errors.Is` / `errors.As`: kiểm tra và lấy lỗi gốc.
- CRUD: Create, Read, Update, Delete. `database/sql` cần driver riêng cho từng loại cơ sở dữ liệu.

---

*Cập nhật bổ sung: đã viết đầy đủ các bài nâng cao còn thiếu — switch, hằng số và kiểu, interface, generics, file, JSON, go mod, test, context, mutex, defer/panic/recover, bọc lỗi, và cơ sở dữ liệu với CRUD — theo cùng nhịp một bài một khái niệm, bóc từng dòng, dễ hiểu nhưng vẫn đầy đủ.*

---

## Phần nâng cao làm project thật: học dần

Phần này viết theo yêu cầu của Hoàng Dươngg: viết hết phần nâng cao để làm project thật, đầy đủ, giải thích chi tiết, dễ hiểu. Đọc phần này khác phần trước ở một điểm: đừng đọc một lèo. Học dần từng bài, làm xong bài nào thì chạy thử bài đó rồi mới sang bài sau. Project mẫu ở cuối phần này dùng lại tất cả các bài trước, nên cậu sẽ thấy các bài cũ xuất hiện lại trong project thật, không phải học lý thuyết suông.

---

## Bài 31. Thời gian, sắp xếp và biểu thức chính quy

Ba công cụ này gặp trong hầu hết project thật, mỗi công cụ chỉ một ví dụ ngắn.

### Thời gian với time

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    bayGio := time.Now()
    fmt.Println(bayGio)
    fmt.Println(bayGio.Format("2006-01-02 15:04:05"))

    ngayMai := bayGio.Add(24 * time.Hour)
    fmt.Println(ngayMai)
}
```

Bóc:

- `time.Now()` lấy thời gian hiện tại.
- `Format("2006-01-02 15:04:05")` là cách Go định dạng thời gian, rất lạ lúc đầu: Go không dùng `YYYY-MM-DD` như nhiều ngôn ngữ khác, mà dùng một ngày mẫu cố định là 2 tháng 1 năm 2006 lúc 15 giờ 4 phút 5 giây, rồi cậu thay các số trong ngày mẫu đó bằng định dạng cậu muốn. Nhớ ngày mẫu này là nhớ được cách định dạng của Go.
- `Add(24 * time.Hour)` cộng thêm 24 giờ, tức là sang ngày mai. `time.Hour`, `time.Minute`, `time.Second` là các đơn vị thời gian có sẵn.

### Sắp xếp với sort

```go
package main

import (
    "fmt"
    "sort"
)

func main() {
    so := []int{5, 2, 9, 1}
    sort.Ints(so)
    fmt.Println(so) // [1 2 5 9]

    ten := []string{"Capybara", "Duongg", "An"}
    sort.Strings(ten)
    fmt.Println(ten) // [An Capybara Duongg]
}
```

- `sort.Ints` sắp xếp số tăng dần, `sort.Strings` sắp xếp chữ theo thứ tự bảng chữ cái.
- Sắp xếp này sửa trực tiếp danh sách gốc, không tạo danh sách mới. Muốn giữ danh sách gốc thì copy ra trước bằng `copy`.

### Biểu thức chính quy với regexp

Biểu thức chính quy, viết tắt là regex, là mẫu để kiểm tra xem một chuỗi có đúng dạng không, ví dụ email có đúng dạng không, số điện thoại có đủ số không.

```go
package main

import (
    "fmt"
    "regexp"
)

func main() {
    mauEmail := regexp.MustCompile(`^[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,}$`)
    fmt.Println(mauEmail.MatchString("duongg@vidu.com")) // true
    fmt.Println(mauEmail.MatchString("duongg@vidu"))     // false
}
```

Bóc, chỉ ở mức đủ dùng:

- Dấu backtick bao mẫu, để trong mẫu không phải thoát dấu.
- `^` là bắt đầu chuỗi, `$` là kết thúc chuỗi, tức là cả chuỗi phải khớp mẫu, không phải chỉ một phần.
- `[a-z0-9._%+-]+` nghĩa là một hoặc nhiều ký tự trong nhóm đó, dấu `+` nghĩa là một hoặc nhiều.
- `@` là ký tự @ thật, `\.` là dấu chấm thật, vì dấu chấm trong regex có nghĩa riêng nên phải thoát bằng `\`.
- `MustCompile` biên dịch mẫu ngay, nếu mẫu sai thì chương trình sẽ panic ngay lúc chạy, để cậu phát hiện sớm. Khi mẫu lấy từ người dùng nhập vào thì dùng `Compile` và kiểm tra lỗi thay vì `MustCompile`.

Lời khuyên: regex mạnh nhưng khó đọc, chỉ dùng khi cần kiểm tra định dạng, đừng dùng regex để thay cho việc cắt chuỗi đơn giản.

---

## Bài 32. Mẫu chạy song song nâng cao

Bài này là phần nâng cao của Bài 15, vẫn chỉ dùng goroutine, channel và WaitGroup, nhưng xếp chúng thành mẫu quen thuộc trong project thật.

### Worker Pool: một nhóm công nhân làm việc từ hàng đợi

Ví dụ đời thường: một quán có 3 nhân viên, khách xếp hàng vào, nhân viên nào rảnh thì nhận khách tiếp theo, không phải mỗi khách lại tuyển một nhân viên mới.

```go
package main

import (
    "fmt"
    "sync"
)

func congNhan(id int, viec <-chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for v := range viec {
        fmt.Println("Cong nhan", id, "lam viec", v)
    }
}

func main() {
    viec := make(chan int)
    var wg sync.WaitGroup

    for i := 1; i <= 3; i++ {
        wg.Add(1)
        go congNhan(i, viec, &wg)
    }

    for v := 1; v <= 9; v++ {
        viec <- v
    }
    close(viec)
    wg.Wait()
}
```

Bóc:

- `viec <-chan int`: tham số kênh chỉ nhận, mũi tên chỉ vào `chan`, nghĩa là hàm này chỉ được nhận việc từ kênh, không được gửi vào.
- `for v := range viec`: nhận việc từ kênh cho tới khi kênh đóng lại, mỗi lần nhận được một việc thì làm việc đó.
- `close(viec)`: đóng kênh sau khi gửi hết việc, để các công nhân biết là hết việc và thoát vòng lặp. Quên đóng kênh thì công nhân sẽ chờ mãi.
- Ba công nhân chạy song song, việc nào được làm bởi công nhân nào thì không chắc chắn, nhưng tổng cộng 9 việc đều được làm xong.

### Pipeline: chuyền việc qua nhiều công đoạn

Pipeline là chuyền dữ liệu qua nhiều công đoạn, mỗi công đoạn là một goroutine, công đoạn sau nhận kết quả của công đoạn trước.

```go
func nhanDoi(dauVao <-chan int) <-chan int {
    dauRa := make(chan int)
    go func() {
        for v := range dauVao {
            dauRa <- v * 2
        }
        close(dauRa)
    }()
    return dauRa
}
```

- Hàm này nhận kênh đầu vào, tạo kênh đầu ra mới, chạy một goroutine để nhân đôi từng số nhận được rồi gửi sang kênh đầu ra, xong thì đóng kênh đầu ra.
- Ghép nhiều công đoạn lại với nhau là thành pipeline, ví dụ đọc file, rồi xử lý, rồi ghi ra, mỗi công đoạn chạy song song với nhau.

### sync.Once và atomic

- `sync.Once`: đảm bảo một việc chỉ chạy đúng một lần, dù nhiều goroutine cùng gọi. Dùng cho việc khởi tạo chỉ cần làm một lần, ví dụ mở kết nối cơ sở dữ liệu lần đầu.
- `atomic`: các phép tính nguyên tử, tức là một phép tính không bị ngắt giữa chừng bởi goroutine khác. Ví dụ `atomic.AddInt64(&dem, 1)` tăng biến đếm một cách an toàn mà không cần mutex, dùng cho biến đếm đơn giản. Biến đếm phức tạp hơn, có nhiều bước, thì vẫn dùng mutex cho chắc.

---

## Bài 33. Web với Gin: framework phổ biến nhất

Gin là framework web phổ biến nhất của Go, viết sẵn router, xử lý JSON, middleware, để cậu không phải viết lại từ đầu bằng `net/http` thuần.

Cài Gin, trong thư mục project đã có `go.mod`:

```bash
go get github.com/gin-gonic/gin
```

Code server Gin tối thiểu:

```go
package main

import "github.com/gin-gonic/gin"

func main() {
    r := gin.Default()

    r.GET("/chao", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "loiChao": "Xin chao Capybara",
        })
    })

    r.Run(":8080")
}
```

Bóc:

- `gin.Default()`: tạo router có sẵn log và xử lý lỗi cơ bản. Muốn tự cấu hình từ đầu thì dùng `gin.New()`, nhưng ở đây dùng `Default` cho nhanh.
- `r.GET("/chao", ...)`: đăng ký đường dẫn `/chao` với phương thức GET, tức là lấy dữ liệu.
- `c *gin.Context`: `Context` của Gin là bối cảnh của một yêu cầu, chứa cả yêu cầu gửi tới và nơi viết câu trả lời, gộp lại một chỗ cho tiện.
- `c.JSON(200, gin.H{...})`: trả về JSON với mã trạng thái 200, tức là thành công. `gin.H` là viết tắt của map với khóa chuỗi và giá trị bất cứ kiểu gì, dùng để viết JSON nhanh.
- `r.Run(":8080")`: chạy server ở cổng 8080, giống `ListenAndServe` ở Bài 16.

Nhận JSON gửi tới, với validation cơ bản:

```go
type ViecMoi struct {
    Ten string `json:"ten" binding:"required"`
}

r.POST("/viec", func(c *gin.Context) {
    var moi ViecMoi
    if err := c.ShouldBindJSON(&moi); err != nil {
        c.JSON(400, gin.H{"loi": err.Error()})
        return
    }
    c.JSON(201, gin.H{"ten": moi.Ten})
})
```

- `binding:"required"` là thẻ báo trường này bắt buộc phải có, thiếu thì Gin báo lỗi.
- `ShouldBindJSON(&moi)`: đọc JSON gửi tới và bỏ vào struct `moi`. Lỗi thì trả về mã 400, tức là yêu cầu sai.
- Mã 201 nghĩa là tạo mới thành công, khác mã 200 là thành công chung chung.

Middleware, tức là lớp xử lý chạy trước hoặc sau mỗi yêu cầu, ví dụ ghi log, kiểm tra đăng nhập:

```go
r.Use(func(c *gin.Context) {
    fmt.Println("Co yeu cau toi:", c.Request.URL.Path)
    c.Next()
})
```

`c.Next()` nghĩa là đi tiếp tới hàm xử lý chính, sau đó quay lại middleware để làm việc sau, ví dụ ghi thời gian xử lý.

Khi nào dùng Gin, khi nào dùng `net/http` thuần: project nhỏ, chỉ vài route thì `net/http` thuần là đủ và nhẹ. Project lớn, nhiều route, cần validation, middleware, nhóm route thì Gin tiết kiệm thời gian hơn nhiều.

---

## Bài 34. Băm mật khẩu và JWT

Hai việc này đi chung với nhau trong đăng nhập: băm mật khẩu để không lưu mật khẩu thật, JWT để cấp vé sau khi đăng nhập đúng.

### Băm mật khẩu bằng bcrypt

Không bao giờ lưu mật khẩu thật vào cơ sở dữ liệu. Lưu bản băm, tức là mật khẩu đã qua một hàm một chiều, không đảo ngược lại được.

Cài thư viện:

```bash
go get golang.org/x/crypto/bcrypt
```

Code:

```go
package main

import (
    "fmt"
    "golang.org/x/crypto/bcrypt"
)

func main() {
    matKhau := "mat-khau-cua-toi"
    bam, err := bcrypt.GenerateFromPassword([]byte(matKhau), bcrypt.DefaultCost)
    if err != nil {
        fmt.Println("Loi:", err)
        return
    }
    fmt.Println(string(bam))

    err = bcrypt.CompareHashAndPassword(bam, []byte(matKhau))
    fmt.Println("Dung khong:", err == nil)

    err = bcrypt.CompareHashAndPassword(bam, []byte("sai-roi"))
    fmt.Println("Dung khong:", err == nil)
}
```

Bóc:

- `GenerateFromPassword`: tạo bản băm từ mật khẩu thật, `bcrypt.DefaultCost` là độ khó mặc định, đủ an toàn cho project vừa và nhỏ. Mỗi lần chạy thì bản băm ra khác nhau, vì bcrypt tự thêm muối ngẫu nhiên vào, nên đừng ngạc nhiên khi chạy hai lần ra hai chuỗi khác nhau mà vẫn cùng một mật khẩu.
- `CompareHashAndPassword`: so sánh mật khẩu người dùng gõ với bản băm đã lưu, đúng thì lỗi là `nil`, sai thì lỗi khác `nil`. Không bao giờ so sánh bằng `==` trực tiếp, vì bản băm mỗi lần mỗi khác.
- Khi đăng ký thì băm rồi lưu bản băm, khi đăng nhập thì lấy bản băm đã lưu ra so sánh, không lưu và không in mật khẩu thật ra đâu cả.

### JWT: vé đăng nhập

JWT, đọc là "jot", là một chuỗi vé có ba phần nối bằng dấu chấm: header, payload, chữ ký. Server ký vé bằng một khóa bí mật chỉ server biết, rồi đưa vé cho người dùng. Lần sau người dùng gửi vé lại, server kiểm tra chữ ký, đúng thì tin là người dùng đó, không cần hỏi lại mật khẩu.

Các bước, ở mức khái niệm, chưa cần code chi tiết từng thư viện vì mỗi thư viện JWT viết hơi khác nhau:

1. Đăng nhập đúng mật khẩu thì server tạo JWT, trong payload ghi `user_id` và thời gian hết hạn, ví dụ hết hạn sau 24 giờ, rồi ký bằng khóa bí mật.
2. Server trả JWT về cho người dùng, người dùng lưu vé đó.
3. Mỗi yêu cầu sau, người dùng gửi vé trong header `Authorization: Bearer <vé>`.
4. Middleware kiểm tra vé: chữ ký đúng không, còn hạn không, đúng thì lấy `user_id` ra và cho đi tiếp, sai hoặc hết hạn thì trả về mã 401, tức là chưa xác thực.

Lưu ý bảo mật, rất quan trọng:

- Khóa ký JWT phải để trong biến môi trường, không ghi trực tiếp vào code, không push lên GitHub.
- Vé hết hạn thì phải đăng nhập lại, đừng để vé sống mãi.
- JWT không phải để lưu thông tin nhạy cảm, vì payload chỉ mã hóa base64, ai cũng đọc được, chỉ có chữ ký là không giả được.


---

## Bài 35. Middleware xác thực và phân quyền

Bài này nối Bài 33 và Bài 34 lại: middleware kiểm tra JWT trước khi cho vào route cần đăng nhập, và phân quyền sau khi đã biết người dùng là ai.

Code middleware Gin, ở mức khái niệm, phần kiểm tra chữ ký JWT chi tiết tùy theo thư viện cậu chọn, ở đây viết dạng giả mã rõ ràng để cậu hiểu luồng trước:

```go
func authMiddleware() gin.HandlerFunc {
    return func(c *gin.Context) {
        token := c.GetHeader("Authorization")
        if token == "" {
            c.JSON(401, gin.H{"loi": "thieu token"})
            c.Abort()
            return
        }

        userID, err := kiemTraToken(token)
        if err != nil {
            c.JSON(401, gin.H{"loi": "token khong hop le"})
            c.Abort()
            return
        }

        c.Set("userID", userID)
        c.Next()
    }
}
```

Bóc:

- `c.GetHeader("Authorization")`: lấy vé từ header. Thực tế vé thường có dạng `Bearer <vé>`, cậu cần cắt bỏ chữ `Bearer ` ở đầu trước khi kiểm tra, ở đây viết gọn lại cho dễ hiểu luồng.
- `c.JSON(401, ...)` rồi `c.Abort()`: trả về lỗi 401 và dừng luôn, không đi tiếp tới hàm xử lý chính. Quên `Abort` là middleware trả lỗi rồi mà hàm chính vẫn chạy tiếp, rất nguy hiểm.
- `c.Set("userID", userID)`: cất `userID` vào context của yêu cầu, để hàm xử lý chính lấy ra dùng mà không cần kiểm tra lại token.
- `c.Next()`: cho đi tiếp tới hàm xử lý chính.

Đăng ký middleware cho nhóm route cần đăng nhập, route công khai thì không cần:

```go
congKhai := r.Group("/cong-khai")
congKhai.POST("/dang-nhap", dangNhap)

canDangNhap := r.Group("/api")
canDangNhap.Use(authMiddleware())
canDangNhap.GET("/viec", layDanhSachViec)
canDangNhap.POST("/viec", taoViecMoi)
```

Phân quyền, sau xác thực: xác thực là biết người dùng là ai, phân quyền là biết người dùng đó được làm gì. Ví dụ người dùng chỉ được sửa và xóa việc của chính mình, không được sửa việc của người khác. Cách làm: trong hàm xử lý, lấy `userID` từ context, rồi so sánh với `userID` của việc đó trong cơ sở dữ liệu, khác nhau thì trả về mã 403, tức là đã xác thực rồi nhưng không có quyền.

Mã trạng thái hay nhầm:

- 401: chưa xác thực, thiếu vé hoặc vé sai.
- 403: đã xác thực rồi, nhưng không có quyền làm việc đó.
- 404: không tìm thấy tài nguyên.
- 400: yêu cầu sai định dạng.
- 500: lỗi phía server, đừng trả chi tiết lỗi nội bộ ra cho người dùng, chỉ ghi log ở server.

---

## Bài 36. Transaction và Migration trong cơ sở dữ liệu

Hai khái niệm này làm cho cơ sở dữ liệu an toàn khi có nhiều việc phải làm cùng nhau, và khi cấu trúc bảng thay đổi theo thời gian.

### Transaction: làm cùng nhau, hoặc không làm gì cả

Ví dụ đời thường: chuyển tiền từ tài khoản A sang tài khoản B phải làm hai việc: trừ tiền ở A và cộng tiền ở B. Nếu trừ xong mà cộng lỗi thì tiền sẽ biến mất, nên hai việc này phải gói lại thành một transaction: cả hai cùng thành công thì mới lưu, một việc lỗi thì hủy cả hai, quay về như chưa làm gì.

Code khái niệm với `database/sql`:

```go
tx, err := db.Begin()
if err != nil {
    fmt.Println("Loi bat dau transaction:", err)
    return
}
defer tx.Rollback()

_, err = tx.Exec("UPDATE tai_khoan SET so_du = so_du - 100 WHERE id = 1")
if err != nil {
    fmt.Println("Loi tru tien:", err)
    return
}

_, err = tx.Exec("UPDATE tai_khoan SET so_du = so_du + 100 WHERE id = 2")
if err != nil {
    fmt.Println("Loi cong tien:", err)
    return
}

err = tx.Commit()
if err != nil {
    fmt.Println("Loi commit:", err)
    return
}
```

Bóc:

- `db.Begin()`: bắt đầu transaction, từ đây mọi câu lệnh dùng `tx`, không dùng `db` trực tiếp nữa.
- `defer tx.Rollback()`: hoãn việc hủy transaction tới cuối hàm. Nếu hàm kết thúc mà chưa `Commit` thành công thì `Rollback` sẽ hủy mọi thay đổi, quay về trạng thái ban đầu. Nếu đã `Commit` thành công rồi thì `Rollback` sau đó không làm gì thêm.
- `tx.Commit()`: lưu tất cả thay đổi trong transaction xuống thật. Chỉ gọi `Commit` khi mọi bước trước đó đều không lỗi.

### Migration: phiên bản của cấu trúc bảng

Migration là các file SQL đánh số thứ tự, mỗi file mô tả một lần thay đổi cấu trúc bảng, ví dụ tạo bảng, thêm cột, đổi kiểu cột. Thay vì sửa bảng bằng tay trên từng máy, cậu chạy migration theo thứ tự là mọi máy đều có cùng cấu trúc bảng.

Ví dụ hai file migration:

- `000001_tao_bang_viec.up.sql`: câu lệnh tạo bảng khi đi lên.
- `000001_tao_bang_viec.down.sql`: câu lệnh xóa bảng khi đi xuống, để quay lại phiên bản trước.

```sql
-- 000001_tao_bang_viec.up.sql
CREATE TABLE viec (
    id SERIAL PRIMARY KEY,
    ten TEXT NOT NULL,
    xong BOOLEAN NOT NULL DEFAULT FALSE,
    created_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

Công cụ migration phổ biến của Go là `golang-migrate`, chạy bằng lệnh, không cần viết code Go cho migration. Nguyên tắc: migration chỉ đi lên, không sửa file migration cũ đã chạy trên máy khác, muốn sửa thì tạo file migration mới. Sửa file cũ đã chạy rồi sẽ làm các máy lệch nhau và rất khó gỡ.

---

## Bài 37. ORM và sqlx: khi nào dùng, khi nào không

ORM là thư viện biến bảng thành struct và câu SQL thành hàm, để cậu không phải viết SQL tay nhiều. `sqlx` thì nhẹ hơn, vẫn viết SQL tay nhưng quét kết quả vào struct cho tiện.

So sánh nhanh, để cậu chọn đúng lúc:

- Viết SQL tay với `database/sql`: kiểm soát hoàn toàn, nhanh, rõ ràng, nhưng viết nhiều code quét kết quả.
- `sqlx`: vẫn viết SQL tay, nhưng quét kết quả vào struct bằng một dòng, ví dụ `db.Select(&danhSach, "SELECT * FROM viec")`, tiện hơn `database/sql` mà vẫn giữ được SQL.
- ORM, ví dụ GORM: viết ít code nhất, ví dụ `db.Create(&viec)`, `db.Where("id = ?", id).First(&viec)`, nhưng khi câu truy vấn phức tạp thì ORM sinh ra SQL khó đoán, khó tối ưu, và cậu phải học thêm cách dùng của ORM đó.

Ví dụ GORM ở mức khái niệm, để cậu nhận ra khi gặp:

```go
type Viec struct {
    ID   uint   `gorm:"primaryKey"`
    Ten  string
    Xong bool
}

db.Create(&Viec{Ten: "Hoc Go", Xong: false})

var v Viec
db.First(&v, 1)
```

- `gorm:"primaryKey"` là thẻ báo trường này là khóa chính.
- `db.Create` chèn một dòng mới, `db.First` lấy dòng đầu tiên khớp điều kiện.

Lời khuyên cho project đầu tiên: dùng SQL tay hoặc `sqlx` trước, để cậu hiểu thật SQL đang chạy cái gì. Khi project lớn lên, câu truy vấn lặp lại nhiều, thì mới cân nhắc ORM để tiết kiệm thời gian, và vẫn phải kiểm tra SQL mà ORM sinh ra khi gặp vấn đề hiệu năng.

---

## Bài 38. Kiểm thử nâng cao: httptest, mock, benchmark

Bài này là phần nâng cao của Bài 25, dành cho project thật có HTTP và cơ sở dữ liệu.

### Test HTTP bằng httptest

`httptest` là gói test HTTP có sẵn, tạo server giả để gọi hàm xử lý mà không cần chạy server thật.

```go
func TestChao(t *testing.T) {
    r := gin.Default()
    r.GET("/chao", func(c *gin.Context) {
        c.JSON(200, gin.H{"loiChao": "Xin chao"})
    })

    req := httptest.NewRequest("GET", "/chao", nil)
    w := httptest.NewRecorder()
    r.ServeHTTP(w, req)

    if w.Code != 200 {
        t.Errorf("muon 200, duoc %d", w.Code)
    }
}
```

- `httptest.NewRequest`: tạo yêu cầu giả.
- `httptest.NewRecorder`: tạo nơi ghi câu trả lời giả, để test đọc mã trạng thái và nội dung trả về mà không cần trình duyệt thật.

### Mock: giả lập phần chưa có hoặc khó test

Mock là tạo một bản giả của một phần, ví dụ giả lập cơ sở dữ liệu, để test hàm xử lý mà không cần cơ sở dữ liệu thật chạy. Cách làm trong Go thường là dùng interface: hàm xử lý nhận interface, test truyền bản mock thỏa interface đó vào, code thật truyền bản thật vào. Đây chính là lý do Bài 20 về interface lại quan trọng trong project thật.

### Benchmark: đo tốc độ

Benchmark là test đo tốc độ, tên hàm bắt đầu bằng `Benchmark`, chạy nhiều lần để tính thời gian trung bình.

```go
func BenchmarkTong(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Tong(3, 4)
    }
}
```

Chạy bằng `go test -bench=. ./...`, `b.N` là số lần máy tự chọn để đo cho đủ chính xác, cậu không cần tự ghi số lần.

### Race: kiểm tra đua nhau

Chạy test với cờ `-race` như đã nhắc ở Bài 27: `go test -race ./...`. Máy sẽ báo chỗ nào hai goroutine cùng sửa một hộp mà không có khóa, để cậu sửa trước khi đưa lên thật.


---

## Bài 39. Profiling, log và cấu hình

Ba việc này làm cho project thật chạy ổn định và gỡ lỗi được khi đã đưa lên server, không còn in ra màn hình để xem như lúc học nữa.

### Profiling với pprof

Profiling là đo xem chương trình tốn thời gian và bộ nhớ ở chỗ nào, để tối ưu đúng chỗ, không đoán mò.

- Bật pprof bằng cách import `_ "net/http/pprof"` trong server, rồi mở trình duyệt vào `/debug/pprof/` để xem.
- Dùng `go test -bench` kết hợp với profile để xem hàm nào tốn nhiều thời gian nhất.
- Quy tắc: đừng tối ưu sớm. Viết cho đúng trước, đo bằng profiling sau, chỉ tối ưu chỗ profiling chỉ ra là tốn nhiều nhất. Tối ưu lung tung thường làm code khó đọc mà không nhanh hơn bao nhiêu.

### Log: ghi nhật ký thay vì in ra màn hình

Lúc học thì dùng `fmt.Println` là đủ, project thật thì dùng log có cấp độ và có thời gian.

```go
import "log"

log.Println("Server bat dau o :8080")
log.Printf("Nguoi dung %d dang nhap", userID)
```

- `log` có sẵn của Go đã ghi thời gian kèm theo, tốt hơn `fmt.Println` khi gỡ lỗi trên server.
- Khi project lớn hơn thì dùng thư viện log có cấu trúc, ví dụ `slog` có sẵn từ Go 1.21, ghi log dưới dạng key-value để máy tìm kiếm cho dễ, ví dụ `slog.Info("dang nhap", "user_id", userID)`.
- Đừng ghi mật khẩu, token, khóa bí mật vào log, dù là log lỗi. Log lỗi chỉ ghi mã lỗi và ngữ cảnh chung, không ghi dữ liệu nhạy cảm.

### Cấu hình bằng biến môi trường

Cấu hình là những thứ thay đổi theo môi trường: cổng chạy, chuỗi kết nối cơ sở dữ liệu, khóa ký JWT. Đừng ghi trực tiếp vào code, hãy đọc từ biến môi trường.

```go
import "os"

port := os.Getenv("PORT")
if port == "" {
    port = "8080"
}
```

- `os.Getenv("PORT")` đọc biến môi trường tên `PORT`. Nếu không có thì dùng giá trị mặc định 8080.
- Lúc phát triển trên máy mình thì tạo file `.env` để ghi các biến này cho tiện, rồi dùng thư viện đọc `.env` để nạp vào, nhưng file `.env` tuyệt đối không push lên GitHub, phải thêm vào `.gitignore`.
- Trên server thật thì cấu hình bằng biến môi trường của server, không dùng file `.env` của máy mình.

---

## Bài 40. Docker cho Go: đóng gói và chạy ở đâu cũng giống nhau

Docker là cách đóng gói chương trình cùng môi trường chạy của nó thành một image, để chạy ở máy nào cũng giống nhau, không bị lỗi "máy mình chạy được mà máy server không chạy được".

Dockerfile nhiều giai đoạn, tức là multi-stage, cho Go:

```dockerfile
# Giai doan 1: build
FROM golang:1.22 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . ./
RUN CGO_ENABLED=0 GOOS=linux go build -o server .

# Giai doan 2: chay
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/server ./
EXPOSE 8080
CMD ["./server"]
```

Bóc:

- Giai đoạn 1 dùng image có sẵn Go để biên dịch code thành file binary tên `server`. `CGO_ENABLED=0` nghĩa là build tĩnh, không phụ thuộc thư viện C của máy, để file binary chạy được trong image nhỏ ở giai đoạn 2.
- Giai đoạn 2 dùng image `alpine` rất nhỏ, chỉ copy file binary từ giai đoạn 1 sang, không copy cả code nguồn và công cụ build, nên image cuối nhẹ và an toàn hơn.
- `EXPOSE 8080` báo cổng server dùng, `CMD ["./server"]` là lệnh chạy khi container khởi động.

File `docker-compose.yml` để chạy server cùng cơ sở dữ liệu Postgres trên máy mình cho tiện:

```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: todo
    ports:
      - "5432:5432"
  app:
    build: .
    environment:
      DATABASE_URL: postgres://user:password@db:5432/todo?sslmode=disable
      PORT: "8080"
    ports:
      - "8080:8080"
    depends_on:
      - db
```

- `db` là service cơ sở dữ liệu, `app` là service Go của cậu.
- Trong `DATABASE_URL`, tên host là `db`, tức là tên service trong compose, không phải `localhost`, vì hai container nói chuyện với nhau qua mạng nội bộ của compose.
- `depends_on` nghĩa là chạy `db` trước rồi mới chạy `app`, nhưng chỉ đảm bảo thứ tự khởi động, không đảm bảo cơ sở dữ liệu đã sẵn sàng nhận kết nối, nên code Go nên thử kết nối lại vài lần khi khởi động thay vì chết ngay lần đầu.

Chạy bằng `docker compose up --build`, dừng bằng `docker compose down`. Muốn xóa luôn dữ liệu thì thêm `-v`, không thì dữ liệu trong volume vẫn giữ lại lần sau.

---

## Bài 41. Project mẫu hoàn chỉnh: Todo API có đăng nhập

Bài này gom tất cả các bài trước lại thành một project thật, ở mức đủ để cậu làm theo từng bước, không phải một đống code dài không biết bắt đầu từ đâu.

### Yêu cầu của project

- Người dùng đăng ký bằng tên và mật khẩu, mật khẩu được băm bằng bcrypt rồi mới lưu.
- Người dùng đăng nhập đúng thì nhận JWT, các route sau phải gửi JWT mới được dùng.
- Mỗi người dùng chỉ xem, sửa, xóa việc của chính mình.
- Việc có các trường: ID, tên, đã xong hay chưa, thời gian tạo.
- Dữ liệu lưu trong Postgres khi chạy thật, lưu trong bộ nhớ khi chạy thử nhanh, hai cách này đổi cho nhau được vì logic CRUD giống nhau.

### Cấu trúc thư mục gợi ý

```
todo-api/
  go.mod
  main.go
  internal/
    handler/
      auth.go
      viec.go
    service/
      auth.go
      viec.go
    repo/
      viec.go
    model/
      viec.go
      nguoi.go
```

Bóc cấu trúc, đây là cách xếp lớp quen thuộc, mỗi lớp chỉ làm một việc:

- `model`: định nghĩa struct, không có logic, chỉ có dữ liệu.
- `repo`: nói chuyện với cơ sở dữ liệu, viết SQL hoặc gọi ORM, trả về struct.
- `service`: logic nghiệp vụ, ví dụ kiểm tra quyền, băm mật khẩu, tạo JWT, gọi repo.
- `handler`: nhận yêu cầu HTTP, đọc JSON, gọi service, trả JSON về, không viết SQL trực tiếp ở đây.
- `main.go`: nối các lớp lại với nhau, đăng ký route, chạy server.

Lợi ích của cách xếp này: muốn đổi cơ sở dữ liệu thì chỉ sửa lớp `repo`, muốn đổi framework web thì chỉ sửa lớp `handler`, logic ở `service` giữ nguyên.

### Luồng chạy của một yêu cầu tạo việc mới

1. Trình duyệt gửi POST tới `/api/viec`, kèm header JWT và JSON `{"ten": "Hoc Go"}`.
2. Middleware xác thực kiểm tra JWT, lấy `userID` ra, cất vào context.
3. Handler đọc JSON, kiểm tra trường `ten` bắt buộc, nếu thiếu thì trả 400.
4. Handler gọi service, service kiểm tra quyền, tạo việc mới với `userID` đó.
5. Service gọi repo, repo chèn một dòng mới vào bảng, lấy ID trả về.
6. Handler trả về JSON việc mới tạo với mã 201.

### Chạy thử bằng curl, không cần giao diện

Đăng ký, đăng nhập, tạo việc, lấy danh sách, sửa việc, xóa việc, mỗi bước đều dùng `curl` để cậu thấy API chạy thật mà không cần viết giao diện web:

```bash
curl -X POST http://localhost:8080/cong-khai/dang-ky \
  -H "Content-Type: application/json" \
  -d '{"ten": "Duongg", "matKhau": "mat-khau-cua-toi"}'

curl -X POST http://localhost:8080/cong-khai/dang-nhap \
  -H "Content-Type: application/json" \
  -d '{"ten": "Duongg", "matKhau": "mat-khau-cua-toi"}'

curl http://localhost:8080/api/viec \
  -H "Authorization: Bearer <TOKEN_VUA_NHAN_DUOC>"
```

Thay `<TOKEN_VUA_NHAN_DUOC>` bằng token thật mà bước đăng nhập trả về. Nếu quên gửi token thì server trả 401, nếu gửi token của người khác mà sửa việc của mình thì server trả 403, đúng như Bài 35.

### Checklist trước khi đưa project lên thật

- [ ] Mật khẩu đã băm, không lưu mật khẩu thật.
- [ ] Khóa JWT và chuỗi kết nối cơ sở dữ liệu để trong biến môi trường, không push lên GitHub.
- [ ] File `.env` đã thêm vào `.gitignore`.
- [ ] Mọi route cần đăng nhập đều đã gắn middleware xác thực, không sót route nào.
- [ ] Mọi hàm có trả về lỗi đều đã kiểm tra `err != nil` ngay sau khi gọi.
- [ ] Đã chạy `go test ./...` và `go test -race ./...` mà không có test nào hỏng.
- [ ] Đã chạy thử bằng Docker trên máy mình, server và cơ sở dữ liệu cùng chạy được.
- [ ] Log không ghi mật khẩu, token, khóa bí mật.
- [ ] Đã có backup cho cơ sở dữ liệu, hoặc ít nhất đã biết cách backup.

---

## Bài 42. Lộ trình học dần sau 30 bài

Bài cuối này không dạy code mới, mà là cách học dần phần nâng cao mà không bị ngợp, đúng như cậu dặn là để học dần.

Thứ tự gợi ý, mỗi bước làm xong một project nhỏ rồi mới sang bước sau:

1. Làm xong Todo API bằng `net/http` thuần, lưu trong bộ nhớ, chạy được bằng `curl`.
2. Đổi Todo API sang lưu trong Postgres, thêm transaction cho những chỗ cần làm cùng nhau.
3. Đổi Todo API sang Gin, thêm middleware xác thực bằng JWT và băm mật khẩu.
4. Viết test cho Todo API bằng `httptest`, chạy `go test -race` cho sạch.
5. Đóng gói Todo API bằng Docker, chạy cùng Postgres bằng `docker-compose`.
6. Thêm một tính năng thật vào Todo API, ví dụ nhắc việc quá hạn bằng goroutine và channel, hoặc xuất danh sách ra file.

Mỗi bước chỉ thêm một thứ mới vào project đã chạy được, đừng viết lại project từ đầu mỗi lần học thứ mới. Project chạy được rồi thì giữ nó chạy được, thêm tính năng mới mà làm hỏng tính năng cũ thì chạy test lại ngay để biết hỏng ở đâu.

Khi gặp lỗi không hiểu, làm theo thứ tự này: đọc thông báo lỗi từ trên xuống dưới, tìm dòng đầu tiên nhắc tới file của mình, không phải file của thư viện, rồi mở đúng file đó, đúng dòng đó ra xem. Đa số lỗi đều nằm ở dòng đó, không phải ở đâu xa.

---

## Phụ lục E. Bảng tra nhanh phần project thật

- `time.Now`, `Format`, `Add`: thời gian hiện tại, định dạng, cộng thời gian.
- `sort.Ints`, `sort.Strings`: sắp xếp số và chữ.
- `regexp.MustCompile`, `MatchString`: biên dịch mẫu và kiểm tra chuỗi khớp mẫu không.
- Worker Pool: nhiều công nhân, một kênh việc, đóng kênh khi hết việc.
- Pipeline: chuyền dữ liệu qua nhiều công đoạn, mỗi công đoạn là một goroutine.
- `sync.Once`: chạy đúng một lần. `atomic`: tăng đếm an toàn không cần mutex.
- Gin: `gin.Default`, `GET`, `POST`, `ShouldBindJSON`, `binding:"required"`, middleware với `c.Next` và `c.Abort`.
- Bcrypt: `GenerateFromPassword` để băm, `CompareHashAndPassword` để so sánh.
- JWT: header, payload, chữ ký, hết hạn, gửi qua header `Authorization: Bearer`.
- 401: chưa xác thực. 403: đã xác thực nhưng không có quyền. 404: không tìm thấy. 400: yêu cầu sai. 500: lỗi server.
- Transaction: `Begin`, `Commit`, `defer Rollback`.
- Migration: file SQL đánh số, chỉ đi lên, không sửa file cũ đã chạy.
- `sqlx`: viết SQL tay nhưng quét vào struct cho tiện. ORM: viết ít code nhất, nhưng phải kiểm tra SQL sinh ra.
- `httptest`: test HTTP không cần server thật. Benchmark: hàm bắt đầu bằng `Benchmark`.
- `pprof`: đo hiệu năng. `slog`: log có cấu trúc. Biến môi trường: `os.Getenv`.
- Docker multi-stage: giai đoạn build dùng image Go, giai đoạn chạy dùng image nhỏ, chỉ copy binary sang.
- `docker-compose`: chạy server cùng cơ sở dữ liệu, tên host trong compose là tên service.

---

*Cập nhật phần project thật: đã viết đầy đủ các bài nâng cao để làm project thật — thời gian, sắp xếp, regex, mẫu song song, Gin, bcrypt và JWT, middleware xác thực và phân quyền, transaction và migration, ORM và sqlx, kiểm thử nâng cao, profiling, log và cấu hình, Docker, project mẫu Todo API có đăng nhập, và lộ trình học dần — theo cùng nhịp một bài một khái niệm, bóc từng dòng, dễ hiểu nhưng vẫn đầy đủ.*

---

## Phần bổ sung cuối: viết thêm cho đầy đủ giáo trình Go

Phần này viết thêm theo yêu cầu của Hoàng Dươngg: còn bài nào thì viết thêm hết cho đầy đủ giáo trình. Vẫn viết vào cùng 1 file `Go/bai-hoc-go-day-du.md`, vẫn giữ nhịp một bài một khái niệm chính, bóc từng dòng, dễ hiểu nhưng đầy đủ. Học phần này thì học dần, mỗi bài làm thử một ví dụ nhỏ rồi mới sang bài sau.

---

## Bài 43. Hàm nâng cao: variadic, closure, đệ quy, receiver

Bốn cách dùng hàm nâng cao hay gặp trong code thật, mỗi cách một ví dụ ngắn.

### Variadic: hàm nhận số lượng đối số không cố định

Từ tiếng Anh: `variadic` = số lượng thay đổi được.

```go
func tongNhieu(so ...int) int {
    tong := 0
    for _, v := range so {
        tong += v
    }
    return tong
}

fmt.Println(tongNhieu(1, 2, 3)) // 6
fmt.Println(tongNhieu(10, 20))   // 30
```

- `so ...int` nghĩa là tham số `so` nhận 0 hoặc nhiều số nguyên, trong hàm thì `so` là một danh sách số nguyên.
- `tong += v` là viết tắt của `tong = tong + v`.
- Gọi hàm với bao nhiêu số cũng được, không cần viết lại hàm cho từng số lượng.

### Closure: hàm nhớ hộp bên ngoài

Closure là hàm được tạo ra bên trong một hàm khác, và nhớ được biến của hàm ngoài dù hàm ngoài đã kết thúc.

```go
func taoDem() func() int {
    dem := 0
    return func() int {
        dem++
        return dem
    }
}

dem1 := taoDem()
fmt.Println(dem1()) // 1
fmt.Println(dem1()) // 2
```

- `taoDem` trả về một hàm, không phải một số.
- Hàm trả về đó nhớ hộp `dem` của riêng nó, mỗi lần gọi thì tăng `dem` lên 1. Tạo một `dem2 := taoDem()` khác thì `dem2` có hộp `dem` riêng, không chung với `dem1`.

### Đệ quy: hàm tự gọi chính nó

Đệ quy, tiếng Anh là `recursion`, là hàm gọi lại chính nó để giải bài toán nhỏ hơn, cho tới khi gặp trường hợp dừng.

```go
func giaiThua(n int) int {
    if n <= 1 {
        return 1
    }
    return n * giaiThua(n-1)
}

fmt.Println(giaiThua(5)) // 120
```

- `giaiThua(5)` bằng `5 * giaiThua(4)`, rồi `4 * giaiThua(3)`, cứ thế cho tới `giaiThua(1)` bằng 1 thì dừng và nhân ngược lại.
- Đệ quy luôn phải có trường hợp dừng, ở đây là `n <= 1`. Quên trường hợp dừng thì hàm gọi mãi không dừng, chương trình sẽ tràn bộ nhớ và chết.

### Receiver: hàm gắn với struct, kiểu giá trị và kiểu con trỏ

Ở Bài 12 cậu đã gặp method với receiver giá trị. Ở đây phân biệt hai loại receiver:

```go
type Dem struct {
    GiaTri int
}

func (d Dem) Xem() int {
    return d.GiaTri
}

func (d *Dem) Tang() {
    d.GiaTri++
}
```

- `(d Dem)` là receiver giá trị: hàm nhận một bản copy của struct, đổi trong hàm thì không đổi struct gốc ở ngoài.
- `(d *Dem)` là receiver con trỏ: hàm nhận địa chỉ của struct, đổi trong hàm thì đổi luôn struct gốc ở ngoài. Vì vậy `Tang` dùng receiver con trỏ, còn `Xem` chỉ đọc thì dùng receiver giá trị là đủ.
- Quy tắc đơn giản: hàm nào đổi struct thì dùng con trỏ, hàm nào chỉ đọc thì dùng giá trị.

---

## Bài 44. Struct nâng cao: embedding, tag, so sánh

### Embedding: nhúng struct vào struct khác

Embedding là nhúng một struct vào trong một struct khác, để struct ngoài dùng luôn các trường và hàm của struct trong mà không cần viết lại.

```go
type DiaChi struct {
    ThanhPho string
}

type Nguoi struct {
    Ten string
    DiaChi
}

d := Nguoi{Ten: "Duongg", DiaChi: DiaChi{ThanhPho: "Phu Tho"}}
fmt.Println(d.Ten)
fmt.Println(d.ThanhPho)
```

- Trong định nghĩa `Nguoi`, chỉ viết `DiaChi` mà không viết tên trường, đó là embedding.
- Khi dùng thì viết thẳng `d.ThanhPho`, không cần viết `d.DiaChi.ThanhPho`, vì trường của struct trong đã được nâng lên struct ngoài.

### Tag: ghi chú cho trường

Tag là chuỗi ghi chú trong dấu backtick sau kiểu của trường, thư viện khác sẽ đọc tag này để biết cách xử lý trường đó. Cậu đã gặp tag `json` ở Bài 23, ở đây nhắc lại nguyên tắc chung:

```go
type Nguoi struct {
    Ten  string `json:"ten" db:"ten"`
    Tuoi int    `json:"tuoi" db:"tuoi"`
}
```

- Một trường có thể có nhiều tag cho nhiều thư viện, mỗi tag viết theo dạng `ten-thu-vien:"gia-tri"`, cách nhau bằng dấu cách.
- Tag không ảnh hưởng tới code Go thuần, chỉ thư viện đọc tag mới dùng nó, ví dụ gói `json` đọc tag `json`, thư viện cơ sở dữ liệu đọc tag `db`.

### So sánh struct

Struct so sánh được bằng `==` khi tất cả các trường của nó đều so sánh được. Ví dụ struct chỉ có `string` và `int` thì so sánh được, struct có trường là slice hoặc map thì không so sánh được bằng `==`, vì slice và map không so sánh được như vậy, phải so sánh từng phần tử hoặc dùng thư viện so sánh sâu.

---

## Bài 45. Slice và Map nâng cao

Bài này là phần nâng cao của Bài 11, dùng khi danh sách và từ điển lớn lên trong project thật.

### Make, copy, clear

```go
s := make([]int, 0, 10)
fmt.Println(len(s), cap(s)) // 0 10

s = append(s, 1, 2, 3)
t := make([]int, len(s))
copy(t, s)
fmt.Println(t) // [1 2 3]

clear(s)
fmt.Println(s) // [0 0 0]
```

Bóc:

- `make([]int, 0, 10)`: tạo danh sách rỗng, độ dài `len` là 0, sức chứa `cap` là 10, tức là đã chuẩn bị sẵn chỗ cho 10 món, thêm vào trong giới hạn đó thì không phải cấp phát lại nhiều lần.
- `cap` khác `len`: `len` là số món đang có, `cap` là số món tối đa có thể chứa mà không cần cấp phát lại.
- `copy(t, s)`: copy từng món từ `s` sang `t`, trả về số món đã copy được.
- `clear(s)`: xóa giá trị của tất cả các món về giá trị 0 của kiểu đó, nhưng độ dài vẫn giữ nguyên, không phải xóa danh sách đi.

### Gói slices và maps của Go mới

Từ Go 1.21 trở đi, Go có sẵn gói `slices` và `maps` để làm những việc hay lặp lại:

```go
import "slices"

fmt.Println(slices.Contains([]string{"a", "b"}, "b")) // true
fmt.Println(slices.Index([]int{10, 20, 30}, 20))       // 1
slices.Sort([]int{3, 1, 2})
```

- `slices.Contains` kiểm tra danh sách có chứa giá trị không, khỏi cần viết vòng lặp.
- `slices.Index` tìm vị trí đầu tiên của giá trị, không tìm thấy thì trả về -1.
- `slices.Sort` sắp xếp, thay cho `sort.Ints` và `sort.Strings` ở Bài 31 khi dùng kiểu chung.

Với map thì dùng gói `maps` để copy, so sánh, xóa nhiều khóa một lúc, thay vì viết vòng lặp tay.

### Sắp xếp theo tiêu chí riêng

Khi danh sách là struct, không phải số hay chữ đơn thuần, thì dùng `sort.Slice` với một hàm so sánh tự viết:

```go
sort.Slice(danhSach, func(i, j int) bool {
    return danhSach[i].Tuoi < danhSach[j].Tuoi
})
```

- Hàm so sánh nhận hai vị trí `i` và `j`, trả về `true` nếu món ở vị trí `i` nên đứng trước món ở vị trí `j`.
- Ở ví dụ này là sắp xếp theo tuổi tăng dần, đổi dấu `<` thành `>` thì thành giảm dần.

---

## Bài 46. Chuỗi nâng cao: strings, rune và unicode

Bài này là phần nâng cao của Bài 9, giải thích vì sao đếm chuỗi tiếng Việt có dấu lại khác với đếm chuỗi không dấu.

### Gói strings

```go
import "strings"

fmt.Println(strings.Join([]string{"a", "b", "c"}, ", ")) // a, b, c
fmt.Println(strings.Split("a,b,c", ","))                 // [a b c]
fmt.Println(strings.TrimSpace("  chao  "))               // chao
fmt.Println(strings.ToUpper("chao"))                     // CHAO
fmt.Println(strings.HasPrefix("chao ban", "chao"))       // true
```

- `Join`: nối danh sách chuỗi lại bằng một chuỗi nối ở giữa.
- `Split`: cắt chuỗi thành danh sách theo chuỗi cắt.
- `TrimSpace`: cắt khoảng trắng ở đầu và cuối chuỗi.
- `ToUpper`: đổi thành chữ hoa, `ToLower` đổi thành chữ thường.
- `HasPrefix`: kiểm tra chuỗi có bắt đầu bằng chuỗi con không, `HasSuffix` kiểm tra có kết thúc bằng chuỗi con không.

### strings.Builder: nối nhiều chuỗi cho nhanh

Nối chuỗi bằng `+` trong vòng lặp nhiều lần sẽ chậm, vì mỗi lần nối là tạo ra một chuỗi mới. Khi nối nhiều lần thì dùng `strings.Builder`:

```go
var b strings.Builder
b.WriteString("Xin chao ")
b.WriteString("Capybara")
fmt.Println(b.String())
```

- `WriteString` ghi thêm chuỗi vào bộ đệm, không tạo chuỗi mới mỗi lần.
- `b.String()` lấy chuỗi cuối cùng ra khi đã ghi xong hết.

### Rune và unicode: vì sao len đếm sai với tiếng Việt có dấu

Ở Bài 9 tớ đã nhắc: `len` đếm theo byte, không phải theo ký tự nhìn thấy. Lý do là Go lưu chuỗi theo UTF-8, mỗi ký tự có thể chiếm 1 byte hoặc nhiều byte. Chữ không dấu chiếm 1 byte, chữ có dấu hoặc emoji chiếm nhiều byte, nên `len` sẽ lớn hơn số ký tự nhìn thấy.

```go
s := "Chào"
fmt.Println(len(s)) // 5, không phải 4
fmt.Println(len([]rune(s))) // 4
```

- `rune` là kiểu ký tự unicode của Go, một `rune` là một ký tự thật, không phải một byte.
- `[]rune(s)` đổi chuỗi thành danh sách các `rune`, rồi đếm danh sách đó thì ra số ký tự thật.
- Khi duyệt chuỗi bằng `range`, Go cũng duyệt theo `rune`, không phải theo byte, nên `for i, r := range s` sẽ cho `r` là từng ký tự thật, còn `i` là vị trí byte bắt đầu của ký tự đó.
- Quy tắc thực hành: đếm độ dài để hiển thị cho người dùng thì dùng `[]rune`, còn `len` thì dùng khi cần biết kích thước byte để gửi qua mạng hoặc lưu file.


---

## Bài 47. HTTP Client: gọi API của người khác

Bài 16 và Bài 33 dạy cậu làm server, bài này dạy cậu làm client, tức là chương trình của cậu đi gọi API của người khác và đọc câu trả lời về.

Code gọi API lấy JSON về:

```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
    "time"
)

type Nguoi struct {
    Ten  string `json:"ten"`
    Tuoi int    `json:"tuoi"`
}

func main() {
    client := &http.Client{Timeout: 10 * time.Second}

    resp, err := client.Get("https://vidu.com/api/nguoi/1")
    if err != nil {
        fmt.Println("Loi goi API:", err)
        return
    }
    defer resp.Body.Close()

    if resp.StatusCode != 200 {
        fmt.Println("Ma trang thai khong phai 200:", resp.StatusCode)
        return
    }

    var d Nguoi
    err = json.NewDecoder(resp.Body).Decode(&d)
    if err != nil {
        fmt.Println("Loi doc JSON:", err)
        return
    }
    fmt.Println(d.Ten, d.Tuoi)
}
```

Bóc:

- `http.Client{Timeout: 10 * time.Second}`: tạo client có thời hạn 10 giây, quá 10 giây mà server chưa trả lời thì hủy, không chờ mãi. Luôn đặt timeout cho client, đừng dùng client mặc định không có timeout trong project thật.
- `client.Get(...)`: gửi yêu cầu GET tới đường dẫn đó.
- `defer resp.Body.Close()`: đóng phần thân câu trả lời khi hàm kết thúc, quên đóng thì sẽ rò rỉ kết nối.
- Kiểm tra `resp.StatusCode` trước khi đọc JSON, vì server có thể trả về mã lỗi kèm nội dung không phải JSON cậu mong đợi.
- `json.NewDecoder(resp.Body).Decode(&d)`: đọc trực tiếp từ thân câu trả lời và giải mã JSON vào struct, không cần đọc hết về chuỗi trung gian.

Gửi JSON đi bằng POST:

```go
duLieu, _ := json.Marshal(Nguoi{Ten: "Duongg", Tuoi: 18})
resp, err := client.Post("https://vidu.com/api/nguoi", "application/json", bytes.NewReader(duLieu))
```

- `json.Marshal` đóng gói struct thành JSON trước.
- `bytes.NewReader` đổi JSON thành luồng đọc để gửi đi.
- Header `Content-Type` là `application/json` báo cho server biết cậu đang gửi JSON, không phải gửi form.

Lỗi hay gặp: quên kiểm tra lỗi của `Get` hoặc `Post` trước khi dùng `resp`, nếu lỗi mà `resp` là `nil` thì dùng tiếp sẽ bị panic.

---

## Bài 48. TCP, UDP, WebSocket và gRPC: các cách nói chuyện khác

HTTP không phải là cách duy nhất để hai chương trình nói chuyện với nhau. Bài này giới thiệu các cách khác ở mức khái niệm, để cậu nhận ra khi gặp trong project thật.

### TCP: nói chuyện có đảm bảo

TCP là giao thức đảm bảo dữ liệu tới đúng thứ tự, không mất, không trùng. HTTP mà cậu dùng hàng ngày cũng chạy trên TCP.

```go
ln, err := net.Listen("tcp", ":9000")
if err != nil {
    fmt.Println("Loi:", err)
    return
}
defer ln.Close()

for {
    conn, err := ln.Accept()
    if err != nil {
        continue
    }
    go xuLyKetNoi(conn)
}
```

- `net.Listen("tcp", ":9000")`: mở cổng 9000 để nghe kết nối TCP tới.
- `ln.Accept()`: chờ có kết nối tới, mỗi kết nối mới thì chạy một goroutine riêng để xử lý, để server không bị chặn ở một kết nối.
- TCP phù hợp khi cần đảm bảo dữ liệu, ví dụ chat, game theo lượt, truyền file.

### UDP: nói chuyện nhanh, không đảm bảo

UDP gửi gói tin đi mà không đảm bảo tới nơi, không đảm bảo thứ tự, nhưng nhanh và nhẹ hơn TCP. Dùng cho video trực tiếp, game thời gian thực, tra cứu DNS, những chỗ mất một gói tin cũng không sao vì gói tiếp theo sẽ tới ngay.

### WebSocket: nói chuyện hai chiều liên tục

HTTP bình thường là: trình duyệt hỏi, server trả lời, xong là hết. WebSocket là: sau khi bắt tay bằng HTTP một lần, kết nối giữ nguyên và cả hai bên đều có thể gửi tin cho nhau bất cứ lúc nào, không cần hỏi trước. Dùng cho chat trực tiếp, thông báo trực tiếp, bảng giá trực tiếp.

### gRPC: gọi hàm qua mạng như gọi hàm trong máy

gRPC là cách gọi hàm ở server khác như gọi hàm trong cùng một chương trình, nhưng dữ liệu truyền đi dưới dạng nhị phân Protobuf, nhỏ và nhanh hơn JSON. Dùng nhiều trong hệ thống microservices, khi nhiều service Go nói chuyện với nhau và cần tốc độ cao. Nhược điểm là khó đọc bằng mắt thường như JSON, và cần định nghĩa schema trước bằng file `.proto`.

Quy tắc chọn: API cho trình duyệt và bên thứ ba thì dùng HTTP với JSON cho dễ đọc và dễ dùng. Service nội bộ cần tốc độ cao thì cân nhắc gRPC. Cần nói chuyện hai chiều liên tục thì cân nhắc WebSocket. Cần đảm bảo dữ liệu thì dùng TCP, cần nhanh và chấp nhận mất gói thì dùng UDP.

---

## Bài 49. Viết CLI: chương trình chạy bằng dòng lệnh

CLI là chương trình chạy trong terminal, nhận tham số từ dòng lệnh, không có giao diện web. Rất nhiều công cụ Go mà cậu dùng hàng ngày, ví dụ chính `go` hay `docker`, đều là CLI.

### Đọc tham số bằng os.Args và flag

```go
package main

import (
    "flag"
    "fmt"
    "os"
)

func main() {
    fmt.Println(os.Args)

    ten := flag.String("ten", "Khach", "Ten nguoi dung")
    tuoi := flag.Int("tuoi", 0, "Tuoi nguoi dung")
    flag.Parse()

    fmt.Println("Ten:", *ten)
    fmt.Println("Tuoi:", *tuoi)
}
```

Bóc:

- `os.Args` là danh sách tham số dòng lệnh, phần tử đầu tiên luôn là tên chương trình, các phần tử sau là tham số người dùng gõ.
- `flag.String("ten", "Khach", "Ten nguoi dung")`: khai báo một cờ tên `ten`, giá trị mặc định là `"Khach"`, mô tả là `"Ten nguoi dung"`. Trả về địa chỉ của giá trị, nên khi dùng phải viết `*ten` để lấy giá trị thật, giống dấu `*` trong con trỏ ở Bài 13.
- `flag.Parse()`: đọc tham số dòng lệnh thật và bỏ vào các cờ đã khai báo. Quên gọi `Parse` thì các cờ vẫn giữ giá trị mặc định.
- Chạy chương trình với `./app -ten Duongg -tuoi 18` thì in ra tên Duongg và tuổi 18, chạy không có tham số thì in ra tên mặc định là Khach.

### Cobra: framework CLI phổ biến

Khi CLI có nhiều lệnh con, ví dụ `git add`, `git commit`, `git push`, thì dùng framework Cobra cho gọn, thay vì tự viết `if` cho từng lệnh con. Cobra lo việc phân tích lệnh con, cờ, và in trợ giúp tự động khi người dùng gõ `--help`. Ở giai đoạn này chỉ cần biết tên gọi, khi nào viết CLI có nhiều lệnh con thì mới học sâu Cobra.

---

## Bài 50. Bảo mật web: CORS, rate limit, validation, injection

Bốn việc bảo mật cơ bản nhất cho API, làm thiếu một việc là API rất dễ bị tấn công.

### CORS: ai được gọi API từ trình duyệt khác

CORS là chính sách của trình duyệt: trang web ở domain A không được tự ý gọi API ở domain B, trừ khi API ở domain B cho phép. Server Go phải trả về header `Access-Control-Allow-Origin` với domain được phép, hoặc `*` nếu cho phép mọi domain, nhưng cho phép mọi domain thì chỉ dùng cho API công khai, không dùng cho API cần đăng nhập.

### Rate limit: giới hạn số yêu cầu

Rate limit là giới hạn mỗi người dùng hoặc mỗi IP chỉ được gọi API một số lần nhất định trong một khoảng thời gian, ví dụ 100 lần một phút. Vượt quá thì server trả về mã 429. Làm rate limit để tránh bị gọi dồn dập làm sập server, và tránh bị dò mật khẩu bằng cách thử hàng nghìn mật khẩu một lúc.

Trong Go, có thể dùng middleware rate limit có sẵn, hoặc tự làm bằng token bucket: mỗi người dùng có một xô token, mỗi yêu cầu lấy một token, token hết thì phải chờ token nạp lại.

### Validation: kiểm tra dữ liệu gửi tới

Đừng bao giờ tin dữ liệu người dùng gửi tới. Kiểm tra ở server, không chỉ kiểm tra ở giao diện, vì người dùng có thể gửi trực tiếp bằng `curl` mà bỏ qua giao diện.

- Trường bắt buộc thì kiểm tra có trống không, như `binding:"required"` ở Bài 33.
- Số thì kiểm tra có trong khoảng hợp lý không, ví dụ tuổi không âm, không lớn hơn 150.
- Chuỗi thì kiểm tra độ dài tối đa, để tránh gửi chuỗi dài hàng triệu ký tự làm đầy bộ nhớ.

### Injection: SQL injection và XSS

- SQL injection: kẻ tấn công gõ câu SQL vào ô nhập liệu, nếu server nối chuỗi trực tiếp vào câu SQL thì câu đó sẽ chạy thật. Cách tránh đã nhắc ở Bài 30: luôn dùng chỗ trống `$1`, `?` và truyền giá trị riêng, không nối chuỗi trực tiếp.
- XSS, tức là Cross-Site Scripting: kẻ tấn công gõ mã JavaScript vào ô nhập liệu, nếu server trả mã đó về cho trình duyệt khác mà không thoát ký tự đặc biệt thì mã đó sẽ chạy trong trình duyệt của người khác. Cách tránh: khi trả dữ liệu người dùng về dưới dạng HTML thì thoát ký tự đặc biệt, còn khi trả dưới dạng JSON cho API thì trình duyệt không chạy mã đó như HTML, nên API JSON an toàn hơn về XSS, nhưng vẫn phải cẩn thận khi hiển thị dữ liệu đó lên giao diện.


---

## Bài 51. Cơ sở dữ liệu nâng cao: pool, index, và hiệu năng truy vấn

Bài này là phần nâng cao của Bài 30 và Bài 36, dành cho khi cơ sở dữ liệu bắt đầu chậm hoặc có nhiều người dùng cùng lúc.

### Connection pool: hồ kết nối

Mở một kết nối mới tới cơ sở dữ liệu mỗi lần cần là rất tốn thời gian. Connection pool là một hồ kết nối đã mở sẵn, mỗi lần cần thì lấy một kết nối từ hồ ra dùng, dùng xong thì trả lại hồ, không đóng hẳn.

Với `database/sql`, cậu cấu hình hồ bằng các hàm sau, ngay sau khi mở `db`:

```go
db.SetMaxOpenConns(25)
db.SetMaxIdleConns(5)
db.SetConnMaxLifetime(5 * time.Minute)
```

- `SetMaxOpenConns`: số kết nối tối đa được mở cùng lúc, ví dụ 25. Vượt quá thì yêu cầu mới phải chờ.
- `SetMaxIdleConns`: số kết nối giữ lại khi rảnh, để lần sau dùng ngay mà không cần mở lại.
- `SetConnMaxLifetime`: thời gian sống tối đa của một kết nối, quá thời gian đó thì đóng và mở lại, để tránh kết nối cũ bị cơ sở dữ liệu cắt mà code không biết.

### Index: mục lục của bảng

Index giống như mục lục của sách: không có mục lục thì tìm một chương phải lật từng trang, có mục lục thì tra mục lục rồi nhảy thẳng tới trang đó. Trong cơ sở dữ liệu, index giúp tìm nhanh theo cột hay được tìm kiếm, ví dụ cột `ten` hay cột `user_id`, nhưng làm chậm việc chèn và cập nhật, vì mỗi lần chèn là phải cập nhật thêm mục lục.

```sql
CREATE INDEX idx_viec_user_id ON viec (user_id);
```

- Tạo index cho cột `user_id` thì câu truy vấn tìm việc theo `user_id` sẽ nhanh hơn nhiều khi bảng lớn.
- Đừng tạo index cho mọi cột, chỉ tạo cho cột hay được dùng trong `WHERE` hoặc `JOIN`, và kiểm tra bằng `EXPLAIN` xem câu truy vấn có dùng index không trước khi kết luận.

### EXPLAIN: xem cơ sở dữ liệu chạy câu truy vấn như thế nào

Đặt `EXPLAIN` trước câu `SELECT` thì cơ sở dữ liệu sẽ in ra kế hoạch chạy câu đó, ví dụ có dùng index không, phải quét bao nhiêu dòng. Khi câu truy vấn chậm, bước đầu tiên là chạy `EXPLAIN`, không phải đoán mò rồi thêm index lung tung.

---

## Bài 52. Fuzzing và coverage: kiểm thử sâu hơn

Bài này là phần nâng cao của Bài 25 và Bài 38, dành cho khi cậu muốn test chắc hơn nữa.

### Fuzzing: máy tự nghĩ ra dữ liệu test

Fuzzing là để máy tự sinh ra hàng nghìn dữ liệu đầu vào ngẫu nhiên, kể cả dữ liệu kỳ lạ mà cậu không nghĩ tới, rồi chạy hàm của cậu với từng dữ liệu đó để xem có bị panic hay sai không.

Hàm fuzz trong Go bắt đầu bằng `Fuzz`, ví dụ test hàm đảo ngược chuỗi:

```go
func FuzzDaoChuoi(f *testing.F) {
    f.Add("chao")
    f.Add("Capybara")
    f.Fuzz(func(t *testing.T, s string) {
        dao := daoChuoi(s)
        dao2 := daoChuoi(dao)
        if dao2 != s {
            t.Errorf("dao hai lan phai tra ve ban dau, duoc %q", dao2)
        }
    })
}
```

- `f.Add(...)`: cho máy vài dữ liệu mẫu ban đầu, gọi là seed, để máy bắt đầu từ đó rồi biến đổi ra dữ liệu mới.
- `f.Fuzz(...)`: hàm test thật, nhận dữ liệu ngẫu nhiên `s` do máy sinh ra, kiểm tra tính chất đúng của hàm, ở đây là đảo hai lần phải về ban đầu.
- Chạy bằng `go test -fuzz=FuzzDaoChuoi`, máy sẽ chạy cho tới khi tìm ra dữ liệu làm hàm sai hoặc cậu dừng tay.

### Coverage: test đã bao phủ bao nhiêu code

Coverage là tỷ lệ code đã được test chạy qua. Chạy test với cờ `-cover` thì máy sẽ in ra tỷ lệ, ví dụ 80% nghĩa là 80% code đã được test chạy qua, 20% còn lại chưa có test nào chạy tới, có thể là nhánh lỗi chưa được test.

```bash
go test -cover ./...
```

Coverage cao thì tốt, nhưng coverage 100% không có nghĩa là code đúng hoàn toàn, vì test có thể chạy qua code mà không kiểm tra kết quả đúng hay sai. Vì vậy coverage chỉ là chỉ số tham khảo, không phải mục tiêu duy nhất.

---

## Bài 53. Tối ưu, build và cross-compile

Bài này gom các việc hay làm khi chuẩn bị đưa chương trình lên thật, sau khi code đã đúng.

### Cross-compile: build cho hệ điều hành khác

Go build được cho hệ điều hành và kiến trúc khác với máy đang build, chỉ cần đặt hai biến môi trường:

```bash
GOOS=linux GOARCH=amd64 go build -o server-linux .
GOOS=windows GOARCH=amd64 go build -o server.exe .
GOOS=darwin GOARCH=arm64 go build -o server-mac .
```

- `GOOS` là hệ điều hành đích: `linux`, `windows`, `darwin` là macOS.
- `GOARCH` là kiến trúc chip đích: `amd64` là chip Intel và AMD 64-bit, `arm64` là chip ARM 64-bit như Apple Silicon.
- Nhờ cross-compile mà cậu ngồi trên máy Windows vẫn build được file chạy trên server Linux, không cần máy Linux riêng.

### Ldflags: nhét thông tin vào binary và làm binary nhỏ hơn

`ldflags` là cờ truyền cho linker, tức là bước cuối cùng khi build, để nhét thông tin vào binary hoặc làm binary nhỏ hơn:

```bash
go build -ldflags "-s -w -X main.version=v1.2.3" -o server .
```

- `-s -w`: bỏ bảng ký hiệu và thông tin debug ra khỏi binary, binary nhỏ hơn, nhưng sau này gỡ lỗi bằng binary đó sẽ khó hơn, nên chỉ dùng cho bản release, không dùng cho bản đang phát triển.
- `-X main.version=v1.2.3`: gán giá trị `v1.2.3` vào biến `version` trong gói `main` lúc build, để chương trình in ra phiên bản của chính nó mà không cần ghi cứng phiên bản vào code.

### Benchmark bộ nhớ

Ngoài benchmark thời gian ở Bài 38, còn có benchmark bộ nhớ bằng cách chạy benchmark với cờ `-benchmem`, máy sẽ in thêm số lần cấp phát bộ nhớ và tổng số byte cấp phát. Hàm nào cấp phát nhiều lần trong vòng lặp thì sẽ chậm và tốn bộ nhớ, sửa bằng cách tái sử dụng bộ đệm hoặc dùng `strings.Builder` như Bài 46 thay vì nối chuỗi bằng `+` trong vòng lặp.

---

## Bài 54. Go workspaces và modules nâng cao

Bài này là phần nâng cao của Bài 24, dành cho khi cậu làm nhiều module cùng lúc trong một project lớn.

### Replace: dùng code local thay cho thư viện đã publish

Trong `go.mod`, directive `replace` báo cho Go là: thay vì tải thư viện từ mạng về, hãy dùng code ở đường dẫn local này.

```
replace example.com/thu-vien => ../thu-vien-local
```

Dùng khi cậu đang sửa một thư viện và muốn test ngay với project chính mà không cần publish thư viện lên mạng trước. Nhớ xóa `replace` trước khi đưa lên thật, hoặc giữ lại chỉ khi có lý do rõ ràng, vì máy khác build sẽ không có đường dẫn local đó.

### Workspaces: làm nhiều module cùng lúc

Go workspace, từ Go 1.18 trở đi, cho phép cậu mở nhiều module trong cùng một workspace mà không cần `replace` từng module một.

File `go.work` ở thư mục cha:

```
go 1.22

use (
    ./todo-api
    ./thu-vien-chung
)
```

- `use` liệt kê các module trong workspace.
- Khi chạy `go build` trong workspace thì Go sẽ dùng code local của các module đó, thay vì tải từ mạng.
- Workspace chỉ dùng lúc phát triển trên máy mình, không push file `go.work` lên cùng với code release, trừ khi cả nhóm đều dùng workspace đó.

### Phiên bản và private modules

- Thư viện Go đánh phiên bản bằng tag Git, ví dụ `v1.2.3`, và `go.mod` ghi phiên bản đó. Khi thư viện lên phiên bản lớn mới, ví dụ từ v1 lên v2, thì đường dẫn import cũng đổi, để code cũ và code mới không lẫn nhau.
- Module private, tức là code trong công ty hoặc code riêng không publish công khai, thì cần cấu hình `GOPRIVATE` và xác thực Git để Go tải được, ở đây chỉ cần biết tên gọi, khi nào làm project công ty thì mới cấu hình sâu.

---

## Bài 55. Kiến trúc hệ thống: monolith, microservices và message queue

Bài cuối cùng của giáo trình, không dạy code mới nhiều, mà dạy cách nghĩ về hệ thống lớn, để cậu chọn kiến trúc đúng cho project của mình.

### Monolith và microservices

- Monolith: toàn bộ hệ thống nằm trong một chương trình duy nhất, dễ phát triển lúc đầu, dễ test, dễ deploy, vì chỉ có một thứ để chạy. Nhược điểm là khi hệ thống lớn lên, một thay đổi nhỏ cũng phải deploy cả hệ thống, và một lỗi ở một phần có thể làm sập cả hệ thống.
- Microservices: tách hệ thống thành nhiều service nhỏ, mỗi service làm một việc, nói chuyện với nhau qua HTTP hoặc gRPC hoặc message queue. Ưu điểm là mỗi service deploy riêng, lỗi ở một service không nhất thiết làm sập service khác. Nhược điểm là phức tạp hơn nhiều: phải lo xác thực giữa các service, theo dõi lỗi qua nhiều service, và deploy nhiều thứ cùng lúc.
- Lời khuyên thực tế, được nhiều người làm Go đồng ý: bắt đầu bằng monolith có cấu trúc tốt, như Todo API ở Bài 41 với các lớp model, repo, service, handler đã tách rõ. Khi nào một phần thật sự cần tách ra vì tải lớn hoặc nhóm khác nhau phụ trách thì mới tách phần đó thành service riêng, đừng tách microservices ngay từ đầu khi chưa cần.

### Message queue: hàng đợi tin nhắn

Message queue là hàng đợi ở giữa hai service: service gửi tin nhắn vào hàng đợi rồi đi làm việc khác ngay, không cần chờ service nhận xử lý xong. Service nhận sẽ lấy tin nhắn từ hàng đợi ra xử lý khi rảnh.

Các hệ thống message queue phổ biến: Kafka cho lượng tin nhắn rất lớn, RabbitMQ cho hàng đợi truyền thống, NATS cho nhẹ và nhanh trong hệ thống Go.

Dùng message queue khi: việc đó không cần trả lời ngay, ví dụ gửi email sau khi đăng ký, xử lý ảnh sau khi tải lên, hoặc khi lượng yêu cầu tăng đột ngột mà service xử lý không kịp, hàng đợi sẽ giữ yêu cầu lại thay vì làm sập service.

### Kết thúc giáo trình

Tới đây là hết giáo trình Go trong file này: từ in một dòng chữ, biến, hàm, danh sách, struct, lỗi, goroutine, HTTP, cơ sở dữ liệu, cho tới Gin, JWT, Docker, Todo API, và kiến trúc hệ thống. Học tiếp sau giáo trình này không phải là học thêm bài mới trong file nữa, mà là làm project thật, gặp vấn đề thật, rồi quay lại tra cứu đúng bài trong file này hoặc đọc tài liệu chính chủ của thư viện đó.

---

## Phụ lục F. Mục lục đầy đủ giáo trình

- Bài 1: In ra màn hình.
- Bài 2: Biến.
- Bài 3: Gán lại giá trị.
- Bài 4: Phép tính với số.
- Bài 5: If và Else.
- Bài 6: For.
- Bài 7: Hàm có tham số.
- Bài 8: Hàm trả về.
- Bài 9: Chuỗi.
- Bài 10: Nhập dữ liệu.
- Bài 11: Danh sách slice và map.
- Bài 12: Struct.
- Bài 13: Con trỏ.
- Bài 14: Xử lý lỗi.
- Bài 15: Goroutine, Channel, WaitGroup.
- Bài 16: HTTP Server.
- Bài 17: Tổ chức project và học tiếp.
- Bài 18: Switch.
- Bài 19: Hằng số, Bool, Số thực và ép kiểu.
- Bài 20: Interface.
- Bài 21: Generics.
- Bài 22: Đọc và ghi file.
- Bài 23: JSON.
- Bài 24: Go mod và nhiều file, nhiều gói.
- Bài 25: Kiểm thử.
- Bài 26: Context.
- Bài 27: Mutex.
- Bài 28: Defer, Panic, Recover.
- Bài 29: Gói lỗi.
- Bài 30: Cơ sở dữ liệu và CRUD.
- Bài 31: Thời gian, sắp xếp và biểu thức chính quy.
- Bài 32: Mẫu chạy song song nâng cao.
- Bài 33: Web với Gin.
- Bài 34: Băm mật khẩu và JWT.
- Bài 35: Middleware xác thực và phân quyền.
- Bài 36: Transaction và Migration.
- Bài 37: ORM và sqlx.
- Bài 38: Kiểm thử nâng cao.
- Bài 39: Profiling, log và cấu hình.
- Bài 40: Docker cho Go.
- Bài 41: Project mẫu Todo API có đăng nhập.
- Bài 42: Lộ trình học dần sau 30 bài.
- Bài 43: Hàm nâng cao.
- Bài 44: Struct nâng cao.
- Bài 45: Slice và Map nâng cao.
- Bài 46: Chuỗi nâng cao, rune và unicode.
- Bài 47: HTTP Client.
- Bài 48: TCP, UDP, WebSocket và gRPC.
- Bài 49: Viết CLI.
- Bài 50: Bảo mật web.
- Bài 51: Cơ sở dữ liệu nâng cao.
- Bài 52: Fuzzing và coverage.
- Bài 53: Tối ưu, build và cross-compile.
- Bài 54: Go workspaces và modules nâng cao.
- Bài 55: Kiến trúc hệ thống.

---

*Giáo trình Go đầy đủ: 55 bài và 6 phụ lục, viết trong cùng 1 file, học dần từng bài một, làm project thật rồi quay lại tra cứu khi cần.*

---

## Phần bổ sung mở rộng cuối cùng: viết hết các phần chuyên sâu còn lại

Phần này viết thêm theo yêu cầu của Hoàng Dươngg: viết hết cho đầy đủ. Đây là các phần chuyên sâu còn lại sau 55 bài, vẫn viết vào cùng 1 file, vẫn giữ nhịp một bài một khái niệm chính, bóc từng dòng, dễ hiểu nhưng đầy đủ. Các bài này là mảng chuyên, không phải ai cũng cần học hết, cậu đọc bài nào khi làm đúng project đó thì mới cần, còn lại chỉ cần biết tên gọi là đủ.

---

## Bài 56. Nhập dữ liệu nâng cao: đọc cả dòng, đổi chuỗi thành số

Bài này là phần còn thiếu của Bài 10, viết đầy đủ ở đây để cậu không phải quay lại tìm nữa.

### Đọc cả dòng có dấu cách bằng bufio

`Scanln` dừng ở dấu cách, nên gõ "Xin chao Capybara" thì nó chỉ lấy được "Xin". Muốn lấy cả dòng có dấu cách thì dùng `bufio`:

```go
package main

import (
    "bufio"
    "fmt"
    "os"
    "strings"
)

func main() {
    fmt.Println("Nhap ho ten cua ban:")
    doc := bufio.NewReader(os.Stdin)
    dong, _ := doc.ReadString('\n')
    dong = strings.TrimSpace(dong)
    fmt.Println("Xin chao " + dong)
}
```

Bóc:

- `bufio.NewReader(os.Stdin)`: tạo một bộ đọc có đệm từ bàn phím, `os.Stdin` là đầu vào chuẩn, tức là bàn phím.
- `doc.ReadString('\n')`: đọc cho tới khi gặp ký tự xuống dòng, tức là tới khi người dùng nhấn Enter, nên lấy được cả dòng có dấu cách.
- `strings.TrimSpace(dong)`: cắt khoảng trắng và dấu xuống dòng ở đầu và cuối dòng vừa đọc, vì `ReadString` giữ lại cả dấu xuống dòng ở cuối.
- Bài này chạy trên máy thật hoặc Termux, không chạy gõ tương tác được trên Go Playground, giống như Bài 10.

### Nhập nhiều giá trị một lần bằng Scanln

`Scanln` nhận được nhiều hộp cùng lúc, mỗi giá trị gõ cách nhau bằng dấu cách:

```go
var ten string
var tuoi int
fmt.Println("Nhap ten va tuoi, cach nhau bang dau cach:")
fmt.Scanln(&ten, &tuoi)
fmt.Println(ten, tuoi)
```

Gõ `Duongg 18` rồi nhấn Enter thì `ten` là `"Duongg"`, `tuoi` là `18`. Gõ sai kiểu, ví dụ gõ chữ vào chỗ tuổi, thì `Scanln` sẽ báo lỗi và tuổi sẽ không đúng, nên sau này khi làm project thật thì nên kiểm tra lỗi của `Scanln` ngay sau khi gọi.

### Đổi chuỗi thành số bằng strconv

Khi đọc bằng `bufio` thì mọi thứ đọc được đều là chuỗi, muốn tính toán thì phải đổi chuỗi thành số bằng gói `strconv`, viết tắt của string conversion, tức là chuyển đổi chuỗi.

```go
import "strconv"

soChuoi := "42"
so, err := strconv.Atoi(soChuoi)
if err != nil {
    fmt.Println("Khong doi duoc thanh so:", err)
    return
}
fmt.Println(so + 1) // 43

soThanhChuoi := strconv.Itoa(43)
fmt.Println(soThanhChuoi + " la chuoi")
```

- `strconv.Atoi`: đổi chuỗi thành số nguyên, `Atoi` là viết tắt của ASCII to integer. Lỗi thì trả về lỗi, ví dụ chuỗi `"abc"` không đổi được thành số.
- `strconv.Itoa`: đổi số nguyên thành chuỗi, `Itoa` là integer to ASCII.
- Đổi số thực thì dùng `strconv.ParseFloat(chuoi, 64)`, số 64 là độ chính xác float64, giống như ở Bài 19.

---

## Bài 57. Reflect: đọc cấu trúc của chương trình lúc đang chạy

Khái niệm mới duy nhất: `reflect`, tức là phản chiếu, soi chương trình của chính mình lúc đang chạy để xem một giá trị thuộc kiểu gì, có những trường nào, giá trị của từng trường là bao nhiêu.

Từ tiếng Anh: `reflect` = phản chiếu, soi lại.

Code:

```go
package main

import (
    "fmt"
    "reflect"
)

type Nguoi struct {
    Ten  string `json:"ten"`
    Tuoi int    `json:"tuoi"`
}

func main() {
    d := Nguoi{Ten: "Duongg", Tuoi: 18}
    kieu := reflect.TypeOf(d)
    giaTri := reflect.ValueOf(d)

    fmt.Println(kieu.Name())
    for i := 0; i < kieu.NumField(); i++ {
        truong := kieu.Field(i)
        fmt.Println(truong.Name, truong.Tag.Get("json"), giaTri.Field(i))
    }
}
```

Bóc:

- `reflect.TypeOf(d)`: lấy kiểu của giá trị `d`, tức là `Nguoi`, và thông tin về các trường của nó.
- `reflect.ValueOf(d)`: lấy giá trị thật của `d` để đọc từng trường.
- `kieu.NumField()`: số trường trong struct.
- `kieu.Field(i)`: thông tin trường thứ `i`, gồm tên trường và tag của nó.
- `truong.Tag.Get("json")`: lấy giá trị của tag `json` của trường đó, ví dụ trường `Ten` thì tag `json` là `"ten"`.
- `giaTri.Field(i)`: giá trị của trường thứ `i`.

Kết quả:

```
Nguoi
Ten ten Duongg
Tuoi tuoi 18
```

Dùng reflect khi nào: khi viết thư viện chung chung, ví dụ thư viện JSON phải đọc bất cứ struct nào mà không biết trước struct đó có những trường nào, thì nó dùng reflect để soi struct rồi mới biết cách đổi sang JSON. Code ứng dụng bình thường thì không nên dùng reflect nhiều, vì code dùng reflect khó đọc, chậm hơn code viết trực tiếp, và mất kiểm tra kiểu lúc biên dịch, sai kiểu thì tới lúc chạy mới phát hiện ra.

---

## Bài 58. Cgo và unsafe: gọi code C và phá vỡ an toàn kiểu

Bài này viết để cậu biết tên gọi và cảnh báo, không phải để khuyến khích dùng hàng ngày.

### Cgo: gọi code C từ Go

Cgo là cách Go gọi code viết bằng ngôn ngữ C. Muốn dùng cgo thì máy phải có trình biên dịch C, và file Go phải có dòng chú thích đặc biệt ngay trên dòng `import "C"`.

```go
/*
#include <stdio.h>
*/
import "C"

import "fmt"

func main() {
    C.puts(C.CString("Xin chao tu C"))
    fmt.Println("Xin chao tu Go")
}
```

Bóc, ở mức khái niệm:

- Phần trong `/* ... */` ngay trên `import "C"` là code C sẽ được chèn vào, ở đây là khai báo hàm `puts` của C.
- `C.CString(...)`: đổi chuỗi Go thành chuỗi C, vì hai ngôn ngữ lưu chuỗi khác nhau.
- `C.puts(...)`: gọi hàm `puts` của C để in ra màn hình.

Cgo làm chương trình chậm hơn khi gọi qua lại nhiều lần, khó build cross-compile như Bài 53, và phụ thuộc vào trình biên dịch C trên máy build. Vì vậy chỉ dùng cgo khi thật sự cần dùng một thư viện C có sẵn mà không có bản Go tương đương, ví dụ thư viện xử lý ảnh hoặc âm thanh chuyên dụng.

### Unsafe: phá vỡ kiểm tra kiểu của Go

Gói `unsafe` cho phép cậu làm những việc Go bình thường cấm, ví dụ đổi một con trỏ kiểu này thành con trỏ kiểu khác, đọc bộ nhớ trực tiếp, hoặc tính kích thước của kiểu dữ liệu theo byte.

```go
import "unsafe"

x := 10
fmt.Println(unsafe.Sizeof(x)) // 8 trên máy 64-bit, vì int chiếm 8 byte
```

- `unsafe.Sizeof` trả về số byte mà một giá trị chiếm trong bộ nhớ.
- Dùng `unsafe` sai thì chương trình có thể đọc nhầm bộ nhớ, ghi đè lên dữ liệu khác, và chết một cách khó gỡ, mà trình biên dịch cũng không báo trước cho cậu.
- Quy tắc thực hành: đừng dùng `unsafe` trong code ứng dụng bình thường. Chỉ thư viện hệ thống, thư viện hiệu năng rất cao, hoặc code giao tiếp với hệ điều hành mới cần tới nó, và khi đó phải có test rất kỹ.


---

## Bài 59. Game, GUI và mobile bằng Go: khi nào chọn, khi nào không

Bài này giới thiệu các mảng mà Go không phải là lựa chọn chính, để cậu biết tên gọi và không mất thời gian đi sai hướng khi làm đúng mảng đó.

### Game bằng Go với Ebitengine

Ebitengine, trước đây gọi là Ebiten, là thư viện làm game 2D bằng Go phổ biến nhất. Cách làm game 2D bằng Ebitengine là: viết một struct game có hàm `Update` để cập nhật trạng thái mỗi khung hình, hàm `Draw` để vẽ, và hàm `Layout` để báo kích thước màn hình, rồi gọi `ebiten.RunGame` để chạy vòng lặp game.

- Ưu điểm: viết bằng Go thuần, chạy được trên nhiều hệ điều hành, làm game 2D nhỏ và vừa rất tiện.
- Hạn chế: làm game 3D lớn, đồ họa nặng, thì các engine chuyên game như Unity hoặc Unreal vẫn mạnh hơn nhiều, vì chúng có sẵn công cụ làm màn chơi, vật lý, ánh sáng.

### Giao diện máy tính bằng Fyne và Wails

- Fyne: thư viện làm giao diện máy tính bằng Go thuần, viết giao diện bằng code Go, chạy được trên Windows, macOS và Linux mà không cần trình duyệt. Phù hợp làm công cụ nhỏ có giao diện đơn giản.
- Wails: cách làm giao diện bằng web, tức là giao diện viết bằng HTML, CSS, JavaScript như làm web, còn logic phía sau viết bằng Go, hai bên nói chuyện với nhau qua cầu nối của Wails. Phù hợp khi cậu đã quen làm web và muốn giao diện đẹp như web mà vẫn có sức mạnh của Go ở phía sau.

### Mobile bằng gomobile

Gomobile là công cụ của Go để viết app điện thoại, hoặc viết thư viện Go rồi gọi từ app Android và iOS viết bằng ngôn ngữ khác. Làm app điện thoại hoàn toàn bằng Go thì ít người làm, vì hệ sinh thái giao diện điện thoại của Go không mạnh bằng Flutter, React Native, hay viết native bằng Kotlin và Swift. Cách dùng Go cho mobile phổ biến hơn là: viết logic lõi bằng Go, đóng gói thành thư viện, rồi app điện thoại gọi thư viện đó.

Kết luận cho bài này: Go mạnh nhất ở backend, công cụ dòng lệnh, và hệ thống phân tán. Game 2D nhỏ thì dùng Ebitengine được, công cụ có giao diện đơn giản thì dùng Fyne hoặc Wails được, còn game lớn, app điện thoại phức tạp thì nên chọn công cụ chuyên cho mảng đó thay vì cố dùng Go cho mọi thứ.

---

## Bài 60. Hệ thống phân tán nâng cao: Raft, tracing, errgroup, sync.Pool

Bài này là phần nâng cao của Bài 32 và Bài 55, dành cho khi hệ thống có nhiều server cùng chạy.

### Raft: đồng thuận giữa nhiều server

Raft là thuật toán để nhiều server cùng đồng ý với nhau về một dữ liệu chung, dù một vài server bị chết hoặc mất kết nối. Ví dụ đời thường: một nhóm người cùng viết một biên bản, phải bầu ra một người trưởng nhóm ghi biên bản, các thành viên khác chép theo trưởng nhóm, nếu trưởng nhóm mất liên lạc thì nhóm bầu lại trưởng nhóm mới từ biên bản đã chép tới đâu.

Các vai trò trong Raft:

- Leader: trưởng nhóm, nhận yêu cầu ghi và gửi cho các thành viên khác.
- Follower: thành viên chép theo leader.
- Candidate: thành viên ứng cử làm leader khi không nghe thấy leader cũ nữa.

Trong Go, cậu không cần tự viết Raft từ đầu cho project bình thường, vì đã có thư viện Raft sẵn, ví dụ `hashicorp/raft`, hoặc dùng cơ sở dữ liệu phân tán đã có Raft bên trong như etcd. Bài này chỉ để cậu hiểu tên gọi và ý tưởng khi gặp trong tài liệu hệ thống phân tán.

### Tracing với OpenTelemetry: theo dõi một yêu cầu qua nhiều service

Khi một yêu cầu đi qua nhiều service, log ở từng service riêng lẻ sẽ rất khó ghép lại với nhau để biết yêu cầu đó đi đâu và chậm ở đâu. Tracing là cách gắn một mã theo dõi chung cho cả hành trình của yêu cầu, mỗi service ghi lại thời gian bắt đầu và kết thúc của phần việc của mình với cùng mã đó, rồi công cụ tracing vẽ lại toàn bộ hành trình thành một cây.

OpenTelemetry là chuẩn chung để làm tracing, log và đo lường, Go có thư viện OpenTelemetry chính thức. Trong code, cậu tạo một span, tức là một đoạn công việc, ở đầu hàm, và đóng span ở cuối hàm bằng `defer`, công cụ sẽ tự ghép các span lại với nhau theo mã theo dõi.

### errgroup và sync.Pool, sync.Map

- `errgroup`: gói trong `golang.org/x/sync`, giúp chạy nhiều goroutine cùng lúc và lấy lỗi đầu tiên gặp phải về, đồng thời hủy các goroutine còn lại khi đã có lỗi. Dùng khi cậu có nhiều việc song song mà chỉ cần một việc lỗi là cả nhóm dừng lại.
- `sync.Pool`: hồ tái sử dụng đồ vật, ví dụ tái sử dụng bộ đệm byte, để tránh cấp phát bộ nhớ mới liên tục. Lấy đồ từ hồ ra dùng, dùng xong thì trả lại hồ, lần sau lấy lại đồ cũ mà dùng tiếp.
- `sync.Map`: map an toàn cho nhiều goroutine cùng dùng mà không cần khóa tay bằng mutex. Dùng khi map bị nhiều goroutine đọc và ghi cùng lúc, còn map bình thường chỉ dùng trong một goroutine hoặc đã có mutex bảo vệ thì dùng map bình thường cho nhanh và dễ đọc hơn.

---

## Bài 61. Hàng đợi và cache chi tiết: Redis, Kafka, NATS

Bài này là phần chi tiết của message queue đã nhắc ở Bài 55, cộng thêm cache, vì hai thứ này hay đi chung trong project thật.

### Redis: cache và hàng đợi nhẹ

Redis là cơ sở dữ liệu lưu trong bộ nhớ, rất nhanh, dùng làm cache, tức là nơi lưu tạm kết quả hay dùng để khỏi phải tính lại hoặc hỏi cơ sở dữ liệu chính nhiều lần, và dùng làm hàng đợi nhẹ.

Các kiểu dữ liệu hay dùng trong Redis:

- String: lưu một giá trị đơn, ví dụ lưu session của người dùng với thời gian hết hạn.
- List: hàng đợi, gửi việc vào cuối danh sách, lấy việc từ đầu danh sách ra làm.
- Set: tập hợp không trùng lặp, ví dụ lưu danh sách người dùng đang online.
- Hash: từ điển, ví dụ lưu thông tin của một người dùng dưới dạng nhiều trường.

Code khái niệm với thư viện Redis của Go, ở mức đủ hiểu luồng, chưa cần chạy server Redis thật:

```go
client.Set(ctx, "ten", "Duongg", 0)
ten, err := client.Get(ctx, "ten").Result()
```

- `Set` lưu giá trị với khóa là `"ten"`, thời gian hết hạn là 0 nghĩa là không hết hạn.
- `Get` lấy giá trị theo khóa, lỗi nếu khóa không tồn tại hoặc đã hết hạn.
- Khi dùng Redis làm cache thì luôn có chiến lược khi cache miss, tức là không tìm thấy trong cache thì hỏi cơ sở dữ liệu chính rồi lưu kết quả vào cache cho lần sau, và khi dữ liệu chính đổi thì xóa hoặc cập nhật cache cũ đi, không thì cache sẽ trả về dữ liệu cũ sai.

### Kafka: hàng đợi cho lượng tin nhắn rất lớn

Kafka là hệ thống hàng đợi phân tán cho lượng tin nhắn rất lớn, ví dụ hàng triệu sự kiện một ngày. Dữ liệu trong Kafka được tổ chức thành topic, tức là chủ đề, và partition, tức là phần chia nhỏ của topic để nhiều máy cùng xử lý song song.

- Producer: bên gửi tin nhắn vào topic.
- Consumer: bên đọc tin nhắn từ topic ra xử lý, nhiều consumer cùng một nhóm sẽ chia nhau các partition để xử lý song song.
- Tin nhắn trong Kafka không bị xóa ngay khi đọc xong, mà giữ lại một thời gian để consumer khác đọc lại hoặc consumer lỗi có thể đọc lại từ đầu.

Dùng Kafka khi lượng tin nhắn lớn, cần giữ lại lịch sử tin nhắn, và cần nhiều service cùng đọc một luồng sự kiện. Dùng hàng đợi nhẹ hơn như Redis hoặc NATS khi lượng tin nhắn vừa và cần đơn giản.

### NATS: hàng đợi nhẹ và nhanh cho hệ thống Go

NATS là hệ thống nhắn tin nhẹ, nhanh, viết bằng Go, dùng cho service Go nói chuyện với nhau. NATS có hai kiểu chính:

- Pub/Sub: bên gửi gửi tin nhắn lên một chủ đề, mọi bên đăng ký chủ đề đó đều nhận được, không cần biết bên gửi là ai.
- Request/Reply: bên gửi gửi yêu cầu và chờ câu trả lời, giống như gọi hàm qua mạng nhưng qua NATS.

Chọn NATS khi hệ thống Go cần nhắn tin nhanh, nhẹ, và không cần giữ lịch sử tin nhắn lâu như Kafka.

---

## Bài 62. gRPC và WebSocket chi tiết

Bài này là phần chi tiết của Bài 48, viết code khung đủ để cậu nhận ra khi gặp trong project thật, chưa cần chạy server gRPC thật ngay.

### gRPC chi tiết: định nghĩa bằng file .proto

Bước đầu tiên của gRPC không phải viết code Go, mà là viết file `.proto` để định nghĩa service và dữ liệu:

```proto
syntax = "proto3";

package todo;

service TodoService {
  rpc LayDanhSach (YeuCauRong) returns (DanhSachViec);
}

message YeuCauRong {}

message Viec {
  int32 id = 1;
  string ten = 2;
  bool xong = 3;
}

message DanhSachViec {
  repeated Viec viec = 1;
}
```

Bóc:

- `service TodoService`: định nghĩa một service có một hàm `LayDanhSach`.
- `rpc LayDanhSach (YeuCauRong) returns (DanhSachViec)`: hàm này nhận một yêu cầu rỗng và trả về danh sách việc.
- `message`: định nghĩa dữ liệu, mỗi trường có một số thứ tự, ví dụ `id = 1`, `ten = 2`, số này dùng để mã hóa nhị phân, không được đổi lung tung sau khi đã dùng.
- `repeated`: nghĩa là danh sách, tương đương slice trong Go.

Sau khi viết file `.proto` thì chạy công cụ `protoc` với plugin Go để sinh code Go từ file đó, rồi cậu chỉ việc viết hàm xử lý thật cho service đó, còn việc mã hóa, gửi nhận qua mạng thì code sinh ra lo hết.

### WebSocket chi tiết: echo server

WebSocket echo server là server nhận tin nhắn nào thì gửi lại đúng tin nhắn đó cho người gửi, dùng để test và làm mẫu cho chat trực tiếp.

Luồng làm việc, ở mức khái niệm với thư viện WebSocket phổ biến của Go:

1. Trình duyệt gửi yêu cầu HTTP bình thường tới server, kèm header xin nâng cấp lên WebSocket.
2. Server đồng ý nâng cấp, từ đây kết nối không còn là HTTP hỏi đáp nữa, mà thành kết nối hai chiều liên tục.
3. Server chạy một vòng lặp: đọc tin nhắn từ kết nối, nếu đọc được thì gửi lại đúng tin nhắn đó, nếu kết nối đóng thì thoát vòng lặp và đóng kết nối.
4. Phía trình duyệt cũng chạy vòng lặp tương tự: gửi tin nhắn đi và nhận tin nhắn về bất cứ lúc nào, không cần hỏi trước.

Khác với HTTP ở chỗ: HTTP server chỉ trả lời khi có yêu cầu, còn WebSocket server có thể tự gửi tin nhắn cho trình duyệt bất cứ lúc nào, ví dụ có người khác nhắn tin tới thì server đẩy tin đó về ngay mà không cần trình duyệt hỏi lại.

---

## Phụ lục G. Bảng tra nhanh phần mở rộng

- `bufio.NewReader`, `ReadString`: đọc cả dòng có dấu cách.
- `strconv.Atoi`, `Itoa`, `ParseFloat`: đổi chuỗi thành số và ngược lại.
- `reflect.TypeOf`, `ValueOf`: soi kiểu và giá trị lúc đang chạy.
- Cgo: gọi code C từ Go, cần trình biên dịch C, dùng khi thật sự cần thư viện C.
- `unsafe.Sizeof`: số byte của một giá trị, dùng `unsafe` sai thì rất nguy hiểm.
- Ebitengine: làm game 2D bằng Go. Fyne: giao diện máy tính bằng Go thuần. Wails: giao diện web với logic Go. Gomobile: app điện thoại hoặc thư viện Go cho app điện thoại.
- Raft: đồng thuận giữa nhiều server, có leader, follower, candidate.
- OpenTelemetry: tracing, gắn mã theo dõi chung cho một yêu cầu qua nhiều service.
- `errgroup`: chạy nhiều goroutine, lấy lỗi đầu tiên và hủy các việc còn lại.
- `sync.Pool`: hồ tái sử dụng đồ vật. `sync.Map`: map an toàn cho nhiều goroutine.
- Redis: cache và hàng đợi nhẹ, lưu trong bộ nhớ, có String, List, Set, Hash.
- Kafka: hàng đợi cho lượng tin nhắn rất lớn, có topic, partition, producer, consumer.
- NATS: hàng đợi nhẹ và nhanh cho hệ thống Go, có Pub/Sub và Request/Reply.
- gRPC: định nghĩa bằng file `.proto`, service, rpc, message, repeated.
- WebSocket: nâng cấp từ HTTP thành kết nối hai chiều liên tục, server tự đẩy tin về được.

---

*Giáo trình Go đầy đủ mở rộng: từ 55 bài lên 62 bài và 7 phụ lục, vẫn trong cùng 1 file, học dần từng bài, làm project thật rồi quay lại tra cứu khi cần. Sau phần này, học tiếp không còn là học thêm bài trong giáo trình nữa, mà là đọc tài liệu chính chủ của từng thư viện cụ thể khi làm đúng project đó.*
