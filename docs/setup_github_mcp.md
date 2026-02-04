# Hướng dẫn thiết lập GitHub MCP Server

Để AI có thể đọc trực tiếp tài liệu Best Practices và Source Code mới nhất từ GitHub mà không cần copy-paste thủ công, bạn cần thiết lập **GitHub MCP Server**.

Dưới đây là các bước cấu hình chi tiết.

## 1. Tạo GitHub Personal Access Token (PAT)

AI cần quyền truy cập vào Repository của bạn thông qua Token.

1.  Truy cập: [GitHub Tokens Settings](https://github.com/settings/tokens).
2.  Chọn **Generate new token (classic)** (cho đơn giản) hoặc Fine-grained token.
3.  Đặt tên (Note): Ví dụ `MCP-AI-Access`.
4.  Chọn Scopes:
    *   **`repo`**: (Bắt buộc) Để đọc code và file từ Private Repos.
    *   **`user`**: (Tùy chọn) Để đọc thông tin user.
5.  Nhấn **Generate token** và **LƯU LẠI NGAY** chuỗi token này (bắt đầu bằng `ghp_...`).

## 2. Cấu hình MCP Server

Tùy thuộc vào ứng dụng AI bạn đang dùng (Claude Desktop, Cursor, v.v.), file cấu hình sẽ nằm ở vị trí khác nhau.

### Đối với Claude Desktop App
Mở file cấu hình tại:
*   **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
*   **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

Thêm (hoặc sửa) mục `github` trong `mcpServers`:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_YOUR_TOKEN_HERE" 
      }
    }
  }
}
```

> ⚠️ **Lưu ý**: Thay `ghp_YOUR_TOKEN_HERE` bằng token bạn vừa tạo ở Bước 1.

### Đối với các MCP Client khác (Smithery, etc.)
Nếu bạn dùng tool quản lý MCP, hãy tìm package `github` hoặc `@modelcontextprotocol/server-github` và điền biến môi trường `GITHUB_PERSONAL_ACCESS_TOKEN`.

## 3. Khởi động lại và Kiểm tra

1.  Tắt và mở lại ứng dụng AI (Claude Desktop/Cursor).
2.  Tại cửa sổ chat, hãy thử kiểm tra kết nối bằng cách gõ:
    > "Hãy đọc file `docs/golang_best_practices.md` từ repository `your-username/your-repo` và tóm tắt nội dung."
3.  Nếu AI xuất hiện công cụ (tool) có icon cái búa và thực hiện lệnh `read_file` hoặc `search_repositories` thành công, nghĩa là bạn đã setup xong.

## 4. Cách sử dụng hiệu quả

Khi đã kết nối, bạn không cần paste lại nội dung file. Hãy dùng các câu lệnh sau:

*   **Đọc Best Practice**:
    > "Đọc quy tắc trong `docs/golang_best_practices.md` từ repo `company/backend-core` và áp dụng để review đoạn code sau..."
*   **Tìm kiếm code cũ**:
    > "Tìm trong repo `company/backend-core` xem hàm `ProcessPayment` được implement ở file nào?"
*   **Tạo Pull Request (Nâng cao)**:
    > "Tạo một branch mới tên `refactor/auth` từ branch `develop` và push các thay đổi sau..." (Cần MCP server hỗ trợ write, cẩn thận khi dùng).

---

### Troubleshooting (Gỡ lỗi)
*   **Lỗi `command not found`**: Đảm bảo máy bạn đã cài [Node.js](https://nodejs.org/) để chạy lệnh `npx`.
*   **Lỗi `Resource not found`**: Kiểm tra lại tên Repository (định dạng `owner/repo`) và chắc chắn Token có quyền `repo` (nếu là Private Repo).
