# 🤝 Hướng dẫn đóng góp

Cảm ơn bạn đã quan tâm! Mọi đóng góp, dù chỉ là sửa một lỗi chính tả, đều có giá trị.

## Có thể đóng góp gì?

- 🐛 Báo nội dung sai hoặc lỗi thời → mở [Issue](../../issues/new/choose)
- 💡 Đề xuất thêm mục mới → mở Issue loại "Đề xuất"
- ✏️ Sửa trực tiếp → mở Pull Request

## Quy trình gửi Pull Request

1. **Fork** repo này về tài khoản của bạn
2. **Clone** bản fork về máy:

   ```bash
   git clone git@github.com:<your-username>/coding-standards.git
   cd coding-standards
   ```

3. **Tạo branch** mới theo quy ước:

   ```bash
   git checkout -b docs/them-muc-database
   ```

4. **Sửa nội dung**, kiểm tra Markdown (không bắt buộc nhưng nên làm):

   ```bash
   npx markdownlint-cli2 "**/*.md"
   ```

5. **Commit** theo [Conventional Commits](https://www.conventionalcommits.org/):

   ```bash
   git commit -m "docs(security): add CSRF checklist items"
   ```

6. **Push** và mở Pull Request vào branch `main`

## Quy ước nội dung

- Viết tiếng Việt, giữ nguyên **thuật ngữ kỹ thuật, lệnh, code** bằng tiếng Anh
- Mỗi mục checklist phải có **nhãn quy mô** 🟢 S / 🟡 M / 🔴 L
- Mục liên quan bảo mật gắn 🔒 và nên dẫn nguồn (OWASP, CWE, NIST...)
- Ví dụ code phải **chạy được** và ghi rõ ngôn ngữ trong code block
- Không thêm link tới nguồn thiếu uy tín

## Quy ước commit

| Type | Dùng khi |
|------|---------|
| `docs` | Thêm/sửa nội dung tài liệu |
| `feat` | Thêm template mới trong `templates/` |
| `fix` | Sửa nội dung sai, link hỏng |
| `ci` | Sửa workflow GitHub Actions |
| `chore` | Việc vặt khác |
