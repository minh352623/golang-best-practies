# HƯỚNG DẪN: SỬ DỤNG GOLANG BEST PRACTICES KHI TƯƠNG TÁC VỚI AI

Chào các bạn Backend Engineers,

Để tối ưu hóa việc sử dụng AI trong lập trình và đảm bảo code sinh ra tuân thủ tuyệt đối tiêu chuẩn của dự án, mọi người vui lòng thực hiện theo quy trình hướng dẫn dưới đây.

## 1. Nguyên tắc "Context First" (Bối cảnh là trên hết)

AI rất thông minh nhưng nó không biết các quy định riêng của team chúng ta. Nếu bạn chỉ yêu cầu "Viết cho tôi hàm Update User", AI sẽ viết theo cách phổ thông.

**Quy tắc bắt buộc**: Luôn cung cấp file Best Practice của dự án vào cửa sổ chat trước khi yêu cầu viết code.

---

## 2. Cách thiết lập phiên làm việc với AI (Prompting)

### Bước 1: Thiết lập "Hợp đồng kỹ thuật"
Mỗi khi bắt đầu một Session mới (trên ChatGPT/Claude), hãy dán nội dung file Best Practice kèm câu lệnh sau:

> "Tôi gửi cho bạn tài liệu Best Practice của dự án Golang của tôi. Hãy đọc kỹ các mục từ 1 đến 9 (về Error Handling, Concurrency, Interface, slog...). Từ giờ trở đi, tất cả code bạn viết ra phải tuân thủ tuyệt đối các quy tắc này. Nếu yêu cầu của tôi vi phạm quy tắc, bạn phải nhắc nhở tôi trước khi thực hiện. Xác nhận nếu bạn đã hiểu."

### Bước 2: Yêu cầu viết code cụ thể
Khi yêu cầu AI viết code, hãy nhắc lại các từ khóa quan trọng trong Best Practice để AI tập trung.

*   ❌ **Ví dụ chưa tốt**: "Viết hàm call API lấy thông tin sản phẩm."
*   ✅ **Ví dụ chuẩn**: "Viết hàm lấy thông tin sản phẩm từ Repository. Nhớ wrap error với ngữ cảnh, sử dụng slog để log lỗi và truyền context xuống tầng Database."

---

## 3. Sử dụng AI để Review ngược lại Code của mình

Bạn có thể dùng tài liệu Best Practice để yêu cầu AI kiểm tra code bạn vừa viết:

> "Đây là đoạn code tôi vừa viết. Dựa trên tài liệu Best Practice đã gửi, hãy chỉ ra các điểm chưa đạt chuẩn (ví dụ: thiếu pre-allocation, chưa dùng errgroup, hay đặt tên package sai) và đề xuất bản sửa lỗi."

---

## 4. Mẹo sử dụng theo từng công cụ

### Đối với Cursor hoặc VS Code Copilot
*   **Sử dụng tính năng Reference (@)**: Trong Cursor, hãy gõ `@BestPractice.md` kèm câu lệnh để AI luôn đọc file này làm căn cứ.
*   **Tạo file `.cursorrules` (Nếu dùng Cursor)**: Copy toàn bộ nội dung Best Practice dán vào file này ở thư mục gốc. AI của Cursor sẽ tự động áp dụng cho mọi câu trả lời mà bạn không cần dán lại.

### Đối với ChatGPT / Claude (Web)
*   **Sử dụng tính năng Custom Instructions**: Bạn có thể copy tóm tắt các quy tắc quan trọng (như Error wrapping, Interface design) dán vào phần Custom Instructions của tài khoản. Như vậy, mọi cửa sổ chat mới đều sẽ mặc định hiểu các quy tắc này.

---

## 5. Checklist kiểm tra nhanh Output của AI

Trước khi copy code từ AI vào dự án, member phải tự kiểm tra lại 5 điểm "nóng" sau:

1.  **Error Handling**: Lỗi có được wrap bằng `%w` không? Có dùng `errors.Is` thay vì `==` không?
2.  **Concurrency**: Có sử dụng `errgroup` cho các task song song không? Context có được truyền xuyên suốt không?
3.  **Performance**: Các Slice/Map có được `make` với capacity trước không?
4.  **Interfaces**: Hàm có đang trả về struct cụ thể (concrete type) thay vì interface không?
5.  **Logging**: Có dùng `slog` với đầy đủ key-value không?
