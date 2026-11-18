# 📘 Coding Standards & Development Checklist

[![Lint](https://github.com/trinhcongkhanh/coding-standards/actions/workflows/lint.yml/badge.svg)](https://github.com/trinhcongkhanh/coding-standards/actions/workflows/lint.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

> Bộ quy chuẩn + checklist cho toàn bộ vòng đời phát triển phần mềm, **dùng được cho mọi quy mô** từ script cá nhân đến hệ thống production, có chú trọng **bảo mật** (OWASP, CWE, NIST SSDF).

---

## ✨ Có gì trong repo này?

- ✅ **Checklist theo từng giai đoạn**: lên kế hoạch → code → test → review → CI/CD → release → bảo trì
- 📏 **Chia theo quy mô** 🟢 Small / 🟡 Medium / 🔴 Large, project cỡ nào cũng áp dụng được
- 🔒 **Security-first**: ví dụ code sai/đúng cho SQLi, command injection, path traversal, kèm mã CWE
- 📦 **Templates dùng ngay**: pre-commit, GitHub Actions, PR template, `.env.example`
- ⚡ **Quick Checklist** để in ra dán cạnh màn hình

## 🚀 Bắt đầu nhanh

**1. Đọc checklist:** mở [`CODING_STANDARDS.md`](CODING_STANDARDS.md)

**2. Chọn level cho project của bạn:**

| Nhãn | Quy mô | Ví dụ |
|------|--------|-------|
| 🟢 **S** | Script, bài tập, CTF solver | `scan_ports.py` |
| 🟡 **M** | Đồ án, project nhóm, portfolio | Web app, REST API |
| 🔴 **L** | Production, có user thật | SaaS, hệ thống công ty |

**3. Copy file vào project của bạn:**

```bash
# Tải checklist về project
curl -O https://raw.githubusercontent.com/trinhcongkhanh/coding-standards/main/CODING_STANDARDS.md

# Hoặc tải template cụ thể, ví dụ pre-commit config
curl -o .pre-commit-config.yaml \
  https://raw.githubusercontent.com/trinhcongkhanh/coding-standards/main/templates/.pre-commit-config.yaml
```

Hoặc bấm nút **"Use this template"** ở đầu trang để tạo repo mới từ repo này.

## 📁 Cấu trúc

```text
.
├── CODING_STANDARDS.md        # 📘 Tài liệu chính
├── templates/                 # 📦 File mẫu, copy thẳng vào project
│   ├── .pre-commit-config.yaml
│   ├── ci-python.yml
│   ├── pull_request_template.md
│   ├── bug_report.md
│   └── .env.example
├── .github/                   # Cấu hình GitHub của chính repo này
├── CONTRIBUTING.md
├── SECURITY.md
├── CHANGELOG.md
└── LICENSE
```

## 🗺️ Roadmap

- [x] Checklist v1 (S/M/L)
- [ ] Template cho JavaScript/TypeScript, Go
- [ ] Bản tiếng Anh
- [ ] Checklist riêng cho database (schema, migration, indexing)

## 🤝 Đóng góp

Mọi góp ý đều được chào đón! Xem [CONTRIBUTING.md](CONTRIBUTING.md) để biết cách đóng góp.

## 📄 License

Phát hành theo giấy phép [MIT](LICENSE). Bạn được tự do dùng, sửa, chia sẻ.

---

<p align="center">Nếu thấy hữu ích, hãy cho repo một ⭐ nhé!</p>
