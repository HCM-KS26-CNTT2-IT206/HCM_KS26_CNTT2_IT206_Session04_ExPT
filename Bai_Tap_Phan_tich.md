# BÀI TẬP PHÂN TÍCH

## So sánh Trade-off các giải pháp ra lệnh cho Kỹ thuật Prompting Nâng cao (Logic & Chuỗi)

### Thông tin sinh viên

-   **Họ và tên:** Võ Xuân Thịnh
-   **Mã sinh viên:** N26DTCN032
-   **Lớp:** HCMKS26CNTT2
-   **Ngành:** Công nghệ thông tin
-   **Chủ đề thực tế:** Tự động hóa quy trình kiểm tra đơn hàng tại cửa
    hàng bán lẻ

------------------------------------------------------------------------

## 1. Giới thiệu bài toán

Trong bài tập này, em chọn tình huống kiểm tra đơn hàng tại một cửa hàng
bán lẻ. Khi khách đặt hàng, nhân viên cần kiểm tra thông tin khách hàng,
sản phẩm, số lượng, đơn giá và số lượng hàng còn trong kho.

Nếu làm thủ công, nhân viên có thể nhập thiếu thông tin, tính nhầm tiền
hoặc xác nhận đơn hàng khi kho không còn đủ sản phẩm. Vì vậy, em muốn
thử sử dụng AI để hỗ trợ kiểm tra đơn hàng theo từng bước.

Mục tiêu của bài là viết hai cách ra lệnh khác nhau:

-   **Giải pháp 1:** Prompt ngắn gọn, dễ viết và dễ sử dụng.
-   **Giải pháp 2:** Prompt chi tiết hơn, có các bước và quy định xử lý
    khi gặp lỗi.

Sau đó, em sẽ so sánh ưu điểm, hạn chế của hai cách và chọn phương án
phù hợp.

## 2. Bước 1: Đề xuất hai prompt mẫu

### 2.1. Giải pháp 1 -- Prompt đơn giản

**Ý tưởng:** Đưa yêu cầu chung cho AI, để AI kiểm tra thông tin và trả
kết quả. Cách này dễ viết, không cần quá nhiều hướng dẫn.

**Prompt mẫu 1:**

``` text
Bạn là trợ lý hỗ trợ xử lý đơn hàng cho một cửa hàng bán lẻ.

Hãy kiểm tra thông tin đơn hàng bên dưới:
- Mã đơn hàng: [Mã đơn]
- Tên khách hàng: [Tên khách]
- Số điện thoại: [Số điện thoại]
- Địa chỉ: [Địa chỉ]
- Sản phẩm: [Tên sản phẩm]
- Số lượng đặt: [Số lượng]
- Đơn giá: [Đơn giá]
- Số lượng tồn kho: [Tồn kho]

Hãy kiểm tra thông tin có đầy đủ và hợp lệ không, tính tổng tiền,
so sánh số lượng đặt với tồn kho và cho biết đơn hàng có thể xử lý
hay không.

Nếu có lỗi hoặc thiếu thông tin, hãy nêu rõ để nhân viên kiểm tra.
Trả lời ngắn gọn, dễ hiểu và có kết luận cuối cùng.
Không tự tạo ra dữ liệu còn thiếu.
```

**Nhận xét ban đầu:**

Prompt này khá ngắn và dễ hiểu. Người mới học cũng có thể tự viết được.
Tuy nhiên, prompt chưa quy định cụ thể thứ tự các bước, cách phân loại
từng lỗi hoặc trường hợp nào được phép tính tiền. Vì vậy, kết quả có thể
chưa đồng nhất giữa các lần chạy.

### 2.2. Giải pháp 2 -- Prompt chi tiết, có nhiều ràng buộc

**Ý tưởng:** Chia công việc thành từng bước rõ ràng. AI phải kiểm tra dữ
liệu trước, sau đó mới tính toán và đưa ra kết luận. Nếu gặp lỗi thì
phải dừng ở bước phù hợp và thông báo cho người dùng.

**Prompt mẫu 2:**

``` text
Bạn là trợ lý AI hỗ trợ kiểm tra đơn hàng cho một cửa hàng bán lẻ.
Nhiệm vụ của bạn là kiểm tra dữ liệu, tính tiền và xác định trạng thái
đơn hàng một cách cẩn thận.

DỮ LIỆU ĐẦU VÀO
- Mã đơn hàng: [Mã đơn]
- Tên khách hàng: [Tên khách]
- Số điện thoại: [Số điện thoại]
- Địa chỉ: [Địa chỉ]
- Tên sản phẩm: [Tên sản phẩm]
- Số lượng đặt: [Số lượng]
- Đơn giá: [Đơn giá]
- Số lượng tồn kho: [Tồn kho]

HÃY THỰC HIỆN THEO 4 BƯỚC:

Bước 1 – Kiểm tra thông tin
- Kiểm tra mã đơn, tên khách hàng, số điện thoại, địa chỉ và sản phẩm.
- Đánh dấu trường nào bị bỏ trống.
- Số lượng đặt phải là số nguyên lớn hơn 0.
- Đơn giá phải là số lớn hơn 0.
- Tồn kho phải là số nguyên không âm.
- Nếu dữ liệu sai hoặc thiếu, ghi rõ lỗi. Không tự đoán dữ liệu.

Bước 2 – Kiểm tra điều kiện xử lý
- Chỉ kiểm tra số lượng hàng khi dữ liệu tồn kho được cung cấp hợp lệ.
- Nếu số lượng đặt lớn hơn tồn kho, kết luận không đủ hàng.
- Nếu tồn kho bị thiếu, ghi là chưa thể xác nhận tình trạng hàng.
- Không tự cho rằng cửa hàng còn hàng khi chưa có dữ liệu.

Bước 3 – Tính toán
- Chỉ tính tổng tiền khi số lượng và đơn giá hợp lệ.
- Công thức: Tổng tiền = Số lượng đặt x Đơn giá.
- Kiểm tra lại phép tính trước khi trả kết quả.
- Nếu vẫn tính được tiền nhưng đơn hàng còn thiếu thông tin khác,
  hãy ghi rõ đây chỉ là số tiền dự kiến, chưa phải xác nhận đơn hàng.

Bước 4 – Kết luận và xử lý lỗi
Chọn một trạng thái phù hợp:
- ĐỦ ĐIỀU KIỆN XỬ LÝ: Thông tin cần thiết hợp lệ và đủ hàng.
- CHỜ BỔ SUNG: Có thông tin bắt buộc bị thiếu.
- KHÔNG HỢP LỆ: Có dữ liệu sai định dạng hoặc giá trị không hợp lý.
- KHÔNG ĐỦ HÀNG: Số lượng đặt lớn hơn tồn kho.
- CHƯA THỂ XÁC NHẬN: Thiếu dữ liệu cần thiết để kiểm tra điều kiện hàng hóa.

Nếu có nhiều lỗi, liệt kê tất cả lỗi.
Nếu một bước không thực hiện được, không được tự bỏ qua lỗi để kết luận
đơn hàng hợp lệ.
Không xác nhận đã thanh toán hoặc giao hàng nếu không có dữ liệu chứng minh.

YÊU CẦU ĐẦU RA

1. Thông tin đơn hàng
2. Kết quả kiểm tra dữ liệu
3. Kết quả kiểm tra tồn kho
4. Tổng tiền (nếu tính được, ghi rõ là dự kiến hay đã xác định)
5. Trạng thái đơn hàng
6. Lỗi phát hiện và hướng xử lý tiếp theo

Hãy giải thích ngắn gọn căn cứ của kết luận. Không cần trình bày suy nghĩ
nội bộ chi tiết; chỉ nêu các bước kiểm tra và kết quả có thể xác minh.
```

**Nhận xét ban đầu:**

Prompt này dài hơn nhưng quy định rõ từng bước. Đặc biệt, nó có hướng xử
lý khi thiếu dữ liệu hoặc khi một bước không thể thực hiện. Nhờ vậy, AI
ít có khả năng tự suy đoán và kết luận đơn hàng hợp lệ khi chưa đủ thông
tin.

## 3. Bước 2: Lập bảng so sánh Trade-off

Trade-off có thể hiểu đơn giản là sự đánh đổi giữa các ưu điểm và hạn
chế. Ví dụ, prompt càng chi tiết thì có thể kiểm soát lỗi tốt hơn, nhưng
lại tốn nhiều nội dung hơn và mất công viết.

  -----------------------------------------------------------------------
  Tiêu chí                Giải pháp 1 -- Đơn giản Giải pháp 2 -- Chi tiết
  ----------------------- ----------------------- -----------------------
  Độ dễ hiểu              Cao, câu lệnh ngắn và   Khá cao, nhưng có nhiều
                          dễ đọc                  quy định nên cần đọc kỹ

  Độ dài prompt           Ngắn                    Dài hơn

  Tốn token               Ít                      Nhiều hơn

  Thời gian viết prompt   Nhanh                   Mất thời gian hơn

  Prompt Chaining         Có yêu cầu các việc     Chia thành 4 bước cụ
                          nhưng chưa chia bước rõ thể

  Kiểm tra dữ liệu        Có kiểm tra nhưng chưa  Kiểm tra từng trường và
                          nêu rõ từng điều kiện   điều kiện

  Xử lý dữ liệu thiếu     Có yêu cầu báo lỗi      Nêu rõ trường hợp thiếu
                          nhưng chưa quy định     dữ liệu và cách phản
                          cách xử lý chi tiết     hồi

  Chặn lỗi biên           Trung bình              Tốt hơn vì có nhiều
                                                  ràng buộc

  Tính nhất quán của kết  Có thể thay đổi tùy     Có khả năng ổn định hơn
  quả                     cách AI hiểu yêu cầu    nhờ cấu trúc đầu ra

  Phù hợp với người mới   Rất phù hợp để bắt đầu  Phù hợp khi đã hiểu quy
                                                  trình cơ bản

  Khả năng mở rộng        Cần bổ sung thêm yêu    Dễ bổ sung quy tắc vào
                          cầu khi bài toán phức   từng bước
                          tạp                     
  -----------------------------------------------------------------------

### 3.1. Phân tích ưu và nhược điểm

**Giải pháp 1 -- Đơn giản**

-   Ưu điểm:
    -   Dễ viết, dễ nhớ và dễ chỉnh sửa.
    -   Không cần biết quá nhiều kỹ thuật vẫn có thể sử dụng.
    -   Tiết kiệm token và phù hợp với yêu cầu đơn giản.
-   Nhược điểm:
    -   Chưa hướng dẫn AI theo một quy trình thật cụ thể.
    -   Có thể bỏ sót một số trường hợp lỗi.
    -   Kết quả giữa các lần chạy có thể chưa thống nhất.

**Giải pháp 2 -- Chi tiết**

-   Ưu điểm:
    -   Có quy trình rõ ràng từ kiểm tra đến kết luận.
    -   Xử lý được nhiều trường hợp thiếu hoặc sai dữ liệu.
    -   Giúp hạn chế việc AI tự suy đoán thông tin.
    -   Kết quả đầu ra có cấu trúc, dễ kiểm tra.
-   Nhược điểm:
    -   Prompt dài, tốn token hơn.
    -   Mất thời gian để viết và chỉnh sửa.
    -   Nếu có quá nhiều quy định không cần thiết thì prompt có thể trở
        nên phức tạp.

### 3.2. Nhận xét chung

Theo em, không phải lúc nào prompt dài hơn cũng tốt hơn. Nếu chỉ cần
kiểm tra một đơn hàng đơn giản thì Giải pháp 1 đã có thể đáp ứng. Nhưng
nếu sử dụng trong cửa hàng có nhiều đơn hàng và cần hạn chế sai sót thì
Giải pháp 2 phù hợp hơn.

## 4. Bước 3: Lựa chọn và chạy thử

### 4.1. Phương án được lựa chọn

Em chọn **Giải pháp 2 -- Prompt chi tiết**.

**Lý do lựa chọn:**

-   Bài toán xử lý đơn hàng có liên quan đến tiền và số lượng hàng trong
    kho nên cần kiểm tra cẩn thận.
-   Prompt chia thành từng bước, giúp AI thực hiện công việc có trình
    tự.
-   Có quy định rõ khi thiếu thông tin hoặc dữ liệu không hợp lệ.
-   Kết quả có cấu trúc nên nhân viên dễ đọc và kiểm tra lại.

Mặc dù prompt này dài hơn và tốn token hơn, nhưng theo em việc hạn chế
nhầm lẫn trong xử lý đơn hàng quan trọng hơn việc tiết kiệm một lượng
nhỏ nội dung prompt.

### 4.2. Dữ liệu chạy thử

Em sử dụng trường hợp đơn hàng bị thiếu số điện thoại để kiểm tra khả
năng phát hiện lỗi của Giải pháp 2.

**Input:**

``` text
Mã đơn hàng: DH002
Tên khách hàng: Trần Minh Anh
Số điện thoại:
Địa chỉ: TP. Hồ Chí Minh
Tên sản phẩm: Chuột không dây
Số lượng đặt: 2
Đơn giá: 150000
Số lượng tồn kho: 8
```

### 4.3. Kết quả chạy thử dự kiến

**KẾT QUẢ KIỂM TRA ĐƠN HÀNG DH002**

**1. Thông tin đơn hàng**

-   Mã đơn: DH002
-   Khách hàng: Trần Minh Anh
-   Sản phẩm: Chuột không dây
-   Số lượng đặt: 2
-   Đơn giá: 150.000đ

**2. Kết quả kiểm tra dữ liệu**

-   Tên khách hàng: Đã có.
-   Số điện thoại: Chưa được cung cấp.
-   Địa chỉ: Đã có.
-   Tên sản phẩm: Đã có.
-   Số lượng: Hợp lệ.
-   Đơn giá: Hợp lệ.

**3. Kết quả kiểm tra tồn kho**

-   Số lượng đặt: 2 sản phẩm.
-   Số lượng tồn kho: 8 sản phẩm.
-   Kết quả: Đủ hàng theo dữ liệu được cung cấp.

**4. Tổng tiền dự kiến**

2 × 150.000 = **300.000đ**.

Đây là số tiền dự kiến vì đơn hàng vẫn còn thiếu thông tin.

**5. Trạng thái đơn hàng**

**CHỜ BỔ SUNG**

**6. Lỗi phát hiện và hướng xử lý**

-   Lỗi: Chưa có số điện thoại khách hàng.
-   Hướng xử lý: Yêu cầu bổ sung số điện thoại trước khi tiếp tục xác
    nhận đơn hàng.

### 4.4. Đánh giá kết quả chạy thử

Qua kết quả trên, em thấy Giải pháp 2 đã phát hiện được thông tin còn
thiếu và không kết luận đơn hàng đã đủ điều kiện xử lý.

AI vẫn có thể tính được số tiền dự kiến vì số lượng và đơn giá đã hợp
lệ. Tuy nhiên, AI ghi rõ đơn hàng đang chờ bổ sung thông tin. Điều này
giúp tránh nhầm lẫn giữa việc tính được tiền và việc xác nhận đơn hàng
hoàn tất.

*Lưu ý: Phần kết quả trên là kết quả chạy thử minh họa theo các quy tắc
của prompt. Khi hoàn thiện bài nộp, em sẽ đưa prompt vào ChatGPT hoặc
Gemini để chạy thực tế và có thể chụp ảnh màn hình kết quả.*

## 5. Kết luận

Sau khi so sánh hai giải pháp, em nhận thấy mỗi cách viết prompt đều có
ưu điểm riêng.

Giải pháp 1 phù hợp với những công việc đơn giản, cần câu lệnh ngắn và
không có nhiều trường hợp đặc biệt. Giải pháp 2 phù hợp hơn với bài toán
xử lý đơn hàng vì có các bước rõ ràng, kiểm tra dữ liệu kỹ hơn và có
cách xử lý khi gặp lỗi.

Em lựa chọn Giải pháp 2 vì mục tiêu của bài toán không chỉ là tính tổng
tiền mà còn phải kiểm tra thông tin và xác định đơn hàng có đủ điều kiện
xử lý hay không.

Qua bài tập này, em hiểu thêm rằng khi viết prompt, mình cần cân nhắc
giữa độ ngắn gọn và mức độ chi tiết. Không nên viết quá dài nếu không
cần thiết, nhưng với những công việc có nhiều điều kiện thì cần hướng
dẫn AI cụ thể để kết quả đáng tin cậy hơn.s