## BÁO CÁO BÀI TẬP: SO SÁNH TRADE-OFF CÁC GIẢI PHÁP PROMPTING NÂNG CAO

**Bối cảnh bài toán được chọn:** Tự động hóa xử lý email khiếu nại của khách hàng cho một cửa hàng thương mại điện tử. AI cần đọc email, phân tích vấn đề và viết email phản hồi.

### Bước 1: Đề xuất 2 prompt mẫu
**Giải pháp 1: Prompt đơn giản, ngắn gọn (Không dùng kỹ thuật nâng cao)**
> "Hãy đọc email khiếu nại sau của khách hàng và viết một email phản hồi xin lỗi. Đồng thời, cho tôi biết khách hàng đang gặp vấn đề gì và cảm xúc của họ ra sao.
> Email khách hàng: [NỘI_DUNG_EMAIL]"

**Giải pháp 2: Prompt chi tiết, nhiều ràng buộc (Áp dụng Chain-of-Thought & Prompt Chaining)**
> "Bạn là chuyên viên chăm sóc khách hàng cao cấp. Hãy xử lý email khiếu nại dưới đây theo đúng luồng logic sau:
> **INPUT:** 
> - Email khách hàng: [NỘI_DUNG_EMAIL]
> **QUÁ TRÌNH TƯ DUY (CHAIN-OF-THOUGHT & CHAINING):**
> - **Bước 1 (Phân tích):** Khách hàng đang gặp vấn đề gì? Cảm xúc của họ là gì? Khách hàng đã cung cấp 'Mã đơn hàng' chưa?
> - **Bước 2 (Quyết định):** Nếu ở Bước 1 khách CHƯA cung cấp Mã đơn hàng, hướng giải quyết là: Xin lỗi và yêu cầu bổ sung mã đơn. Nếu khách ĐÃ cung cấp, hướng giải quyết là: Xin lỗi và thông báo chuyển thông tin cho bộ phận kỹ thuật.
> - **Bước 3 (Thực thi):** Viết email phản hồi cuối cùng dựa trên hướng giải quyết ở Bước 2.
> **RÀNG BUỘC ĐẦU RA:**
> Trình bày kết quả rõ ràng theo 3 phần: [Phân tích logic], [Hướng giải quyết], và [Email phản hồi]. Tuyệt đối không tự bịa ra Mã đơn hàng nếu khách không cung cấp."

### Bước 2: Lập bảng so sánh Trade-off
| Tiêu chí đánh giá | Giải pháp 1 (Đơn giản) | Giải pháp 2 (Chi tiết, áp dụng CoT & Chuỗi) |
| :--- | :--- | :--- |
| **Độ dễ hiểu / Thời gian viết** | Rất cao (Viết nhanh, ai cũng làm được). | Trung bình (Cần tư duy logic, tốn thời gian thiết kế cấu trúc). |
| **Tốn Token (Chi phí API)** | Ít (Tiết kiệm chi phí đầu vào và ra). | Nhiều hơn (Do chứa nhiều lệnh hướng dẫn và AI phải in ra các bước suy luận). |
| **Chặn lỗi biên (Edge cases)** | Kém (Dễ bị lỗi ảo giác - Hallucination). | Cực tốt (AI phải kiểm tra điều kiện dữ liệu trước khi ra quyết định). |
| **Độ ổn định / Nhất quán** | Thấp (Mỗi lần tạo ra một cấu trúc câu trả lời khác nhau, khó tích hợp tự động). | Cao (Luôn tuân thủ luồng tư duy và cấu trúc đầu ra đã quy định). |

### Bước 3: Lựa chọn và chạy thử
**1. Lựa chọn phương án tối ưu nhất:**
Tôi chọn **Giải pháp 2 (Chi tiết)** làm phương án tối ưu nhất vì trong môi trường doanh nghiệp thực tế, tính chính xác và an toàn được đặt lên hàng đầu.

**2. Chạy thử Giải pháp 2 (Test Case thực tế):**
*   **Dữ liệu truyền vào (Input):**
    *   `[NỘI_DUNG_EMAIL]`: *"Chào shop, tôi mua cái máy xay sinh tố tuần trước mà hôm nay bật không lên điện. Hàng kém chất lượng quá, tôi rất bực mình. Giải quyết ngay cho tôi!"* (Cố tình thiếu mã đơn hàng).
*   **Kết quả đầu ra của AI:**
> **[Phân tích logic]**
> - Vấn đề: Máy xay sinh tố mới mua 1 tuần bật không lên điện.
> - Cảm xúc: Bực tức, không hài lòng về chất lượng.
> - Trạng thái dữ liệu: Khách hàng CHƯA cung cấp Mã đơn hàng.
> 
> **[Hướng giải quyết]**
> Do chưa có Mã đơn hàng, không thể tra cứu hệ thống. Cần viết email xoa dịu cảm xúc, gửi lời xin lỗi và xin bổ sung Mã đơn hàng hoặc Số điện thoại.
> 
> **[Email phản hồi]**
> Chào Quý khách,
> Cửa hàng rất xin lỗi vì trải nghiệm không vui của Quý khách với sản phẩm máy xay sinh tố. 
> Để chúng tôi có thể lập tức tra cứu thông tin bảo hành trên hệ thống và tiến hành đổi/sửa cho Quý khách theo đúng chính sách, Quý khách vui lòng cung cấp giúp chúng tôi **Mã đơn hàng** hoặc **Số điện thoại** đã dùng để đặt hàng nhé.
> Ngay khi nhận được thông tin, bộ phận kỹ thuật sẽ ưu tiên xử lý trường hợp của mình ngay lập tức. Một lần nữa thành thật xin lỗi Quý khách!
> Trân trọng.
