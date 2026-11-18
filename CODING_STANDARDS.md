# 📘 Coding Standards & Development Checklist

> Bộ quy chuẩn + checklist cho toàn bộ vòng đời code, từ lúc có ý tưởng đến khi release và bảo trì.
> Dùng được cho **mọi quy mô**, từ script cá nhân đến project nhóm hay production.

---

## 📑 Mục lục

0. [Cách dùng tài liệu này](#0-cách-dùng-tài-liệu-này)
1. [Nguyên tắc cốt lõi](#1-nguyên-tắc-cốt-lõi)
2. [Lên kế hoạch trước khi code](#2-lên-kế-hoạch-trước-khi-code)
3. [Khởi tạo project](#3-khởi-tạo-project)
4. [Git workflow](#4-git-workflow)
5. [Viết code](#5-viết-code)
6. [Security](#6-security)
7. [Testing](#7-testing)
8. [Self-review & Code review](#8-self-review--code-review)
9. [CI/CD](#9-cicd)
10. [Documentation](#10-documentation)
11. [Release & Versioning](#11-release--versioning)
12. [Deploy & Monitoring](#12-deploy--monitoring)
13. [Maintenance](#13-maintenance)
14. [Phụ lục: Templates](#14-phụ-lục-templates)

---

## 0. Cách dùng tài liệu này

Mỗi mục được gắn nhãn quy mô. Chỉ cần làm những mục **ngang hoặc thấp hơn** quy mô project của bạn.

| Nhãn | Quy mô | Ví dụ |
|------|--------|-------|
| 🟢 **S** | Small: script, bài tập, tool cá nhân, CTF solver | `scan_ports.py`, bài lab |
| 🟡 **M** | Medium: project cá nhân nghiêm túc, đồ án, project nhóm 2–5 người | Web app, API, portfolio project |
| 🔴 **L** | Large: production, nhiều người, có user thật, có dữ liệu nhạy cảm | SaaS, hệ thống công ty |

> 💡 **Quy tắc:** project 🟡 M làm hết 🟢 + 🟡. Project 🔴 L làm tất cả.
> Project lớn dần thì nâng cấp dần, không cần làm hết từ ngày đầu.

**Ký hiệu:**

- `- [ ]` việc cần làm / cần check
- ⚠️ lỗi hay gặp
- 🔒 liên quan bảo mật
- 📚 **Nguồn:** link tới tài liệu gốc (OWASP, NIST, GitHub Docs, Python Docs, Google...) để bạn tự kiểm chứng
- 💭 **Kinh nghiệm thực tế:** quy tắc nhiều team dùng nhưng **không có chuẩn chính thức**. Dùng làm điểm khởi đầu rồi điều chỉnh theo team

> ℹ️ Mọi con số và khuyến nghị bảo mật trong tài liệu này đã được đối chiếu với nguồn gốc vào **11/2026**. Chuẩn bảo mật thay đổi theo thời gian (ví dụ OWASP Top 10 có bản 2025, thay bản 2021), nên hãy kiểm tra lại link gốc khi dùng.

---

## 1. Nguyên tắc cốt lõi

Áp dụng cho **mọi** project, không phân biệt quy mô.

| Nguyên tắc | Ý nghĩa |
|------------|---------|
| **KISS** (Keep It Simple, Stupid) | Giải pháp đơn giản nhất mà chạy đúng là giải pháp tốt nhất |
| **DRY** (Don't Repeat Yourself) | Một logic chỉ nên tồn tại ở một chỗ |
| **YAGNI** (You Aren't Gonna Need It) | Không code tính năng "để sau này có thể cần" |
| **Single Responsibility** | Một function/class chỉ làm một việc |
| **Fail fast** | Phát hiện lỗi càng sớm càng tốt: validate input ngay đầu, crash rõ ràng thay vì im lặng |
| **Secure by default** 🔒 | Mặc định là an toàn; muốn mở quyền thì phải chủ động bật |
| **Boy Scout Rule** | Rời khỏi đoạn code sạch hơn lúc bạn đến |

> ⚠️ DRY không có nghĩa là gộp mọi thứ trông giống nhau. Hai đoạn code giống nhau nhưng **thay đổi vì lý do khác nhau** thì nên để riêng (tránh "wrong abstraction").

📚 **Nguồn:** DRY do Andy Hunt & Dave Thomas đặt ra trong *The Pragmatic Programmer*. YAGNI xuất phát từ Extreme Programming. Single Responsibility thuộc bộ nguyên tắc SOLID của Robert C. Martin. Đây là **nguyên tắc thiết kế**, không phải chuẩn bắt buộc. "Wrong abstraction": xem bài [The Wrong Abstraction](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction) của Sandi Metz.

---

## 2. Lên kế hoạch trước khi code

> "Weeks of coding can save you hours of planning." Đừng mở editor ngay.

### 🟢 S

- [ ] Viết ra **1–3 câu**: chương trình này giải quyết vấn đề gì, input là gì, output là gì
- [ ] Liệt kê các **edge case** nghĩ ra được (input rỗng, sai định dạng, quá lớn, ký tự đặc biệt...)

### 🟡 M

- [ ] Viết **requirements** ngắn: functional (làm gì) + non-functional (nhanh cỡ nào, chịu bao nhiêu user)
- [ ] Chia việc thành **issues/tasks** nhỏ (mỗi task ≤ 1 ngày làm)
- [ ] Vẽ sơ đồ kiến trúc đơn giản (các thành phần + luồng dữ liệu)
- [ ] Thiết kế **database schema** (bảng, quan hệ, index) trước khi viết query
- [ ] Chọn tech stack và ghi lại **lý do** chọn

### 🔴 L

- [ ] Viết **design doc / RFC** cho tính năng lớn, cho team review trước khi code
- [ ] **Threat modeling** 🔒: xác định asset, attacker, attack surface. Gợi ý dùng mô hình **STRIDE** của Microsoft: **S**poofing, **T**ampering, **R**epudiation, **I**nformation disclosure, **D**enial of service, **E**levation of privilege
  - 📚 [Microsoft Learn: Threat modeling framework](https://learn.microsoft.com/en-us/training/modules/tm-use-a-framework-to-identify-threats-and-find-ways-to-reduce-or-eliminate-risk/1b-threat-modeling-framework) · [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html)
- [ ] Ghi **ADR** (Architecture Decision Record) cho mỗi quyết định kiến trúc quan trọng
  - 📚 [adr.github.io](https://adr.github.io/)
- [ ] Ước lượng capacity, chi phí, SLA

---

## 3. Khởi tạo project

### 🟢 S

- [ ] `git init` ngay từ đầu, kể cả project nhỏ
- [ ] Tạo `.gitignore` phù hợp ngôn ngữ (lấy từ [github/gitignore](https://github.com/github/gitignore))
- [ ] `README.md` tối thiểu: project làm gì + cách chạy
- [ ] Khai báo dependencies (`requirements.txt`, `package.json`, `go.mod`...)

### 🟡 M

- [ ] Cấu trúc thư mục rõ ràng (xem [Phụ lục 14.1](#141-cấu-trúc-thư-mục-mẫu))
- [ ] `LICENSE` (gợi ý: [choosealicense.com](https://choosealicense.com); MIT cho project cá nhân)
- [ ] `.env.example` liệt kê các biến môi trường cần thiết (**không có giá trị thật**) 🔒
- [ ] `.editorconfig` để thống nhất indent, line ending giữa các editor
- [ ] Cài **formatter + linter** ngay từ đầu (xem bảng dưới)
- [ ] Cài **pre-commit hooks** (xem [Phụ lục 14.4](#144-pre-commit-config-mẫu))
- [ ] **Lock file** cho dependencies (`package-lock.json`, `poetry.lock`, `uv.lock`...) để build có thể tái lập

| Ngôn ngữ | Formatter | Linter |
|----------|-----------|--------|
| Python | `black` hoặc `ruff format` | `ruff`, `mypy` (type check) |
| JavaScript/TS | `prettier` | `eslint` |
| Go | `gofmt` | `go vet`, `golangci-lint` |
| Java | `google-java-format` | `checkstyle`, `spotbugs` |
| C/C++ | `clang-format` | `clang-tidy`, `cppcheck` |
| SQL | `sqlfluff` | `sqlfluff` |

### 🔴 L

- [ ] `CONTRIBUTING.md`: cách đóng góp, chạy test, quy trình PR
- [ ] `SECURITY.md` 🔒: cách báo cáo lỗ hổng (responsible disclosure)
- [ ] `CODE_OF_CONDUCT.md` (nếu open source)
- [ ] `CODEOWNERS`: ai chịu trách nhiệm review phần nào
- [ ] Issue templates + PR template trong `.github/`
- [ ] Dev environment tái lập được: `Dockerfile` / `docker-compose.yml` / Dev Container

---

## 4. Git workflow

### 4.1 Branch

### 🟢 S

- [ ] Commit thường xuyên, mỗi commit là **một thay đổi có ý nghĩa**

### 🟡 M

- [ ] **Không commit thẳng vào `main`**. Mọi thay đổi đi qua branch + Pull Request
- [ ] Đặt tên branch theo quy ước:

```text
feature/<mô-tả-ngắn>     → feature/user-login
fix/<mô-tả-ngắn>         → fix/null-pointer-cart
hotfix/<mô-tả-ngắn>      → hotfix/sqli-search
docs/<mô-tả-ngắn>        → docs/api-readme
refactor/<mô-tả-ngắn>    → refactor/payment-service
chore/<mô-tả-ngắn>       → chore/update-deps
```

### 🔴 L

- [ ] Bật **branch protection** cho `main`: bắt buộc PR, bắt buộc CI pass, bắt buộc ≥1 approval
- [ ] Chọn một mô hình branching và tuân thủ: **GitHub Flow** (đơn giản, khuyên dùng) hoặc **Git Flow** (nhiều release song song)
- [ ] Bật **signed commits** (GPG/SSH signing) 🔒

### 4.2 Commit message: Conventional Commits

Theo chuẩn [Conventional Commits](https://www.conventionalcommits.org/):

```text
<type>(<scope>): <mô tả ngắn, thể mệnh lệnh, không quá 72 ký tự>

[body: giải thích TẠI SAO thay đổi, không phải thay đổi CÁI GÌ]

[footer: Closes #123, BREAKING CHANGE: ...]
```

| Type | Khi nào dùng |
|------|-------------|
| `feat` | Thêm tính năng mới |
| `fix` | Sửa bug |
| `docs` | Chỉ thay đổi tài liệu |
| `style` | Format, khoảng trắng (không đổi logic) |
| `refactor` | Sửa cấu trúc code, không đổi hành vi |
| `perf` | Cải thiện hiệu năng |
| `test` | Thêm/sửa test |
| `chore` | Việc vặt: cập nhật deps, config |
| `ci` | Thay đổi CI/CD |
| `security` | Vá lỗ hổng bảo mật (một số team dùng, hoặc dùng `fix`) |

**Ví dụ tốt / xấu:**

```bash
# ✅ Tốt
feat(auth): add rate limiting to login endpoint
fix(search): use parameterized query to prevent SQL injection
docs(readme): add setup instructions for Windows

# ❌ Xấu
update
fix bug
asdfgh
final version 2 real final
```

### 4.3 Pull Request

- [ ] PR **nhỏ**: lý tưởng dưới ~200–400 dòng thay đổi. PR lớn sẽ được review hời hợt
  - 📚 Nghiên cứu của SmartBear trên team Cisco (khoảng 2.500 review, 3,2 triệu dòng code): review quá 400 dòng một lúc thì khả năng tìm ra lỗi giảm rõ rệt. [SmartBear: Best Practices for Code Review](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/)
  - 📚 Google cũng yêu cầu thay đổi nhỏ, nhưng nhấn mạnh "nhỏ" là **tập trung vào một ý**, không phải con số dòng cứng nhắc. [Google eng-practices: Small CLs](https://google.github.io/eng-practices/review/developer/small-cls.html)
- [ ] Một PR = một mục đích. Không gộp feature + refactor + fix vào chung
- [ ] Mô tả rõ: làm gì, tại sao, test thế nào, có screenshot nếu là UI (xem [Phụ lục 14.2](#142-pull-request-template))
- [ ] Link tới issue liên quan (`Closes #123`)
- [ ] Tự review diff của mình **trước** khi nhờ người khác

---

## 5. Viết code

### 5.1 Naming

- [ ] Tên **nói lên ý nghĩa**, không cần comment mới hiểu
- [ ] Theo convention của ngôn ngữ (`snake_case` cho Python, `camelCase` cho JS/Java, `PascalCase` cho class...)
- [ ] Biến boolean bắt đầu bằng `is_`, `has_`, `can_`, `should_`
- [ ] Function là **động từ** (`get_user`, `validate_email`), class là **danh từ** (`User`, `PaymentService`)
- [ ] Tránh viết tắt khó hiểu, tránh tên một chữ (trừ `i`, `j` trong vòng lặp ngắn)

```python
# ❌ Xấu
def proc(d, f):
    x = d * 0.1 if f else 0
    return x

# ✅ Tốt
DISCOUNT_RATE = 0.1

def calculate_discount(price: float, is_member: bool) -> float:
    return price * DISCOUNT_RATE if is_member else 0.0
```

### 5.2 Function & cấu trúc

- [ ] Function **ngắn**, làm **một việc**. Khi function phải cuộn màn hình mới đọc hết, hoặc phải đặt tên có chữ "and", thì cân nhắc tách
  - 💭 Không có chuẩn chính thức về số dòng. Nhiều linter cho cấu hình ngưỡng (ví dụ rule `max-lines-per-function` của ESLint, hoặc rule `PLR0915` của Ruff giới hạn số statement). Hãy chọn ngưỡng cùng team
- [ ] Tối đa ~3–4 tham số; nhiều hơn thì gom vào object/dataclass
- [ ] Tránh lồng sâu (> 3 cấp `if`/`for`). Dùng **guard clause** / early return
- [ ] Không dùng **magic number/string**, đặt thành hằng số có tên
- [ ] Tránh biến global có thể thay đổi (mutable global state)

```python
# ❌ Lồng sâu
def process(user):
    if user:
        if user.is_active:
            if user.has_permission("write"):
                do_work(user)

# ✅ Guard clause
def process(user):
    if not user:
        return
    if not user.is_active:
        return
    if not user.has_permission("write"):
        return
    do_work(user)
```

### 5.3 Error handling

- [ ] **Không bao giờ nuốt lỗi im lặng** (`except: pass`, `catch (e) {}`)
- [ ] Bắt exception **cụ thể**, không bắt chung chung `Exception` trừ ở tầng ngoài cùng
- [ ] Thông báo lỗi cho **user** chung chung; chi tiết lỗi ghi vào **log** 🔒
- [ ] Giải phóng tài nguyên đúng cách (`with` trong Python, `try-with-resources` trong Java, `defer` trong Go)

```python
# ❌ Nuốt lỗi + lộ thông tin
try:
    data = load_config(path)
except:
    pass

# ✅ Cụ thể + log + thông báo hợp lý
import logging
logger = logging.getLogger(__name__)

try:
    data = load_config(path)
except FileNotFoundError:
    logger.error("Config file not found: %s", path)
    raise SystemExit("Configuration error. Check logs for details.")
```

### 5.4 Comment

- [ ] Comment giải thích **TẠI SAO**, không giải thích **CÁI GÌ** (code đã nói "cái gì")
- [ ] Function/class public có **docstring** (mô tả, tham số, giá trị trả về, exception)
- [ ] Xóa code bị comment out. Git đã lưu lịch sử rồi
- [ ] `TODO`/`FIXME` phải kèm issue hoặc tên người: `# TODO(#42): handle pagination`

### 5.5 Logging

- [ ] Dùng **logging library**, không dùng `print()` trong code chính
- [ ] Dùng đúng **level**: `DEBUG` < `INFO` < `WARNING` < `ERROR` < `CRITICAL`
- [ ] 🔒 **Không bao giờ log**: password, session ID, access token, API key, connection string, encryption key, số thẻ, PII không cần thiết. Nếu cần theo dõi theo session thì log **giá trị hash** của session ID
  - 📚 [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html) (mục "Data to exclude")
- [ ] 🔴 L: log dạng **structured** (JSON), có `request_id`/`trace_id` để trace

### 5.6 Configuration

- [ ] Config tách khỏi code, đọc từ **biến môi trường** hoặc file config ([12-Factor App](https://12factor.net/config))
- [ ] Có giá trị mặc định an toàn; thiếu config bắt buộc thì **crash ngay khi khởi động** (fail fast)

---

## 6. Security

> 🔒 Tham chiếu: [OWASP Top 10:2025](https://owasp.org/Top10/2025/) (bản mới nhất, thay bản 2021), [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/), [CWE Top 25](https://cwe.mitre.org/top25/), [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/).

### 🟢 S (bắt buộc với MỌI project)

- [ ] **Không hardcode secret** (password, API key, token) trong code
- [ ] `.env` nằm trong `.gitignore`
- [ ] **Validate mọi input** từ bên ngoài (user, file, network, API, env)
- [ ] Không dùng `eval()`, `exec()`, `pickle.loads()`, `os.system()` với dữ liệu không tin cậy
  - 📚 Python Docs ghi rõ module `pickle` **không an toàn**: dữ liệu pickle độc hại có thể **thực thi code tùy ý** lúc unpickle. Dùng `json` cho dữ liệu từ bên ngoài. [Python Docs: pickle](https://docs.python.org/3/library/pickle.html)

### 🟡 M

- [ ] **SQL**: luôn dùng **parameterized query / prepared statement** hoặc ORM. Không nối chuỗi (CWE-89, thuộc **A05:2025 Injection**)
  - 📚 OWASP xếp prepared statement là **lớp phòng thủ chính số 1**, còn "escape toàn bộ input" thì bị **khuyến cáo mạnh là không nên** dùng làm biện pháp chính. [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

```python
# ❌ SQL Injection: input "' OR '1'='1" sẽ bypass
cursor.execute(f"SELECT * FROM users WHERE name = '{username}'")

# ✅ Parameterized: DB driver tách riêng code và data
cursor.execute("SELECT * FROM users WHERE name = %s", (username,))
```

- [ ] **Output encoding** chống XSS (CWE-79): dùng template engine có auto-escape
- [ ] **Password**: hash bằng **Argon2id** (ưu tiên), hoặc `bcrypt` cho hệ thống cũ. **Không** dùng MD5/SHA1/SHA256 thuần, vì chúng quá nhanh nên brute-force bằng GPU rất rẻ
  - 📚 OWASP khuyến nghị Argon2id với cấu hình tối thiểu `m=19 MiB, t=2, p=1`. Lưu ý `bcrypt` chỉ xử lý tối đa 72 byte input. [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) · Argon2 được chuẩn hóa trong [RFC 9106](https://www.rfc-editor.org/rfc/rfc9106)
- [ ] **Command execution**: dùng list argument, không dùng `shell=True` với input (CWE-78)
  - 📚 Python Docs: khi truyền list argument, `subprocess` **không gọi shell**, nên ký tự đặc biệt như `;` hay `|` được truyền nguyên văn. Khi bật `shell=True`, ứng dụng phải **tự chịu trách nhiệm** escape mọi ký tự đặc biệt để tránh **shell injection**. [Python Docs: subprocess, Security Considerations](https://docs.python.org/3/library/subprocess.html#security-considerations)

```python
# ❌ Command injection: host = "8.8.8.8; rm -rf /"
subprocess.run(f"ping -c 1 {host}", shell=True)

# ✅ Truyền list, không qua shell
subprocess.run(["ping", "-c", "1", host], check=True)
```

- [ ] **Path traversal** (CWE-22): chuẩn hóa đường dẫn và kiểm tra nằm trong thư mục cho phép
- [ ] **Secret scanning** trong pre-commit: `gitleaks` hoặc `trufflehog`
- [ ] **Dependency scanning**: bật Dependabot / `pip-audit` / `npm audit` / `trivy`
- [ ] **SAST** (Static Application Security Testing): `semgrep`, `bandit` (Python), CodeQL
- [ ] Authentication & Authorization: kiểm tra quyền ở **server side**, mọi endpoint
- [ ] HTTPS everywhere; cookie có `HttpOnly`, `Secure`, `SameSite`

### 🔴 L

- [ ] Nguyên tắc **Least Privilege**: DB user, IAM role, service account chỉ có quyền tối thiểu
- [ ] Quản lý secret bằng **secret manager** (AWS Secrets Manager, HashiCorp Vault...), xoay vòng (rotate) định kỳ
- [ ] **Rate limiting** + chống brute force cho login/API
- [ ] **Security headers**: `Content-Security-Policy`, `Strict-Transport-Security`, `X-Content-Type-Options`...
- [ ] **DAST** (Dynamic testing): OWASP ZAP chạy trên môi trường staging
- [ ] **Container scanning**: `trivy image`, không chạy container bằng root
- [ ] **SBOM** (Software Bill of Materials) cho mỗi release
- [ ] Audit log cho hành động nhạy cảm (đăng nhập, đổi quyền, xóa dữ liệu)
- [ ] Pentest định kỳ / bug bounty
- [ ] Đối chiếu với **OWASP ASVS** level phù hợp

> ⚠️ **Lỡ commit secret lên Git?** Xóa commit **không đủ**, vì secret vẫn còn trong lịch sử và có thể đã bị bot quét.
> → **Revoke/rotate secret ngay lập tức**, sau đó mới dọn lịch sử (`git filter-repo`).
> ⚠️ `git revert` **không** xóa được secret: commit gốc vẫn nằm trong lịch sử.
>
> 📚 GitHub Docs: nếu dữ liệu nhạy cảm là password/token/credential thì **bước đầu tiên** là revoke hoặc rotate nó. [Removing sensitive data from a repository](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)

---

## 7. Testing

### Testing Pyramid

```text
        /\
       /E2E\        ← Ít: chậm, đắt, dễ vỡ
      /------\
     /Integration\  ← Vừa: test các thành phần kết hợp
    /--------------\
   /   Unit Tests   \ ← Nhiều: nhanh, rẻ, cô lập
  /------------------\
```

📚 Mô hình do Mike Cohn đưa ra trong sách *Succeeding with Agile*, được Martin Fowler phổ biến. Ý chính: viết test ở nhiều mức độ chi tiết khác nhau, và **càng lên cao thì càng ít test**. [Martin Fowler: The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)

💭 Mô hình này vẫn đang được tranh luận: một số người đề xuất "Testing Trophy", đặt nặng integration test hơn. Hãy coi kim tự tháp là định hướng, không phải tỉ lệ bắt buộc.

### 🟢 S

- [ ] Chạy thử với input bình thường **+ ít nhất 2–3 edge case**
- [ ] Viết vài unit test cho logic quan trọng nhất

### 🟡 M

- [ ] Dùng test framework (`pytest`, `jest`, `go test`, `JUnit`)
- [ ] Unit test cho business logic; mỗi bug fix kèm **regression test**
- [ ] Test theo cấu trúc **AAA**: Arrange → Act → Assert
- [ ] Test các trường hợp: happy path, edge case, error case, **input độc hại** 🔒
- [ ] Đo **code coverage**, nhưng nhớ rằng **coverage cao ≠ test tốt**
  - 📚 Google **không có con số bắt buộc**. Hướng dẫn chung của họ: 60% là "chấp nhận được", 75% là "đáng khen", 90% là "mẫu mực", và đừng cố đẩy từ 90% lên 95%. [Google Testing Blog: Code Coverage Best Practices](https://testing.googleblog.com/2020/08/code-coverage-best-practices.html)
- [ ] Test chạy **độc lập**, không phụ thuộc thứ tự, không phụ thuộc mạng/thời gian thật (dùng mock)

```python
# pytest: ví dụ AAA + edge case + security case
import pytest
from app.validators import validate_username

def test_valid_username():
    # Arrange
    name = "alice_01"
    # Act
    result = validate_username(name)
    # Assert
    assert result is True

@pytest.mark.parametrize("bad_input", [
    "",                       # rỗng
    "a" * 300,                # quá dài
    "admin' OR '1'='1",       # SQLi payload
    "<script>alert(1)</script>",  # XSS payload
    "../../etc/passwd",       # path traversal
])
def test_rejects_invalid_username(bad_input):
    assert validate_username(bad_input) is False
```

### 🔴 L

- [ ] **Integration test** với DB/service thật (dùng Docker / Testcontainers)
- [ ] **E2E test** cho luồng quan trọng (Playwright, Cypress)
- [ ] **Performance / load test** (k6, Locust, JMeter)
- [ ] **Fuzz testing** cho parser, input handler (AFL++, atheris, go-fuzz) 🔒
- [ ] Contract test giữa các service
- [ ] Không merge nếu test fail (bắt buộc qua CI)

---

## 8. Self-review & Code review

### 8.1 Self-review checklist (trước khi commit / mở PR)

- [ ] Code **chạy được** và đã test thủ công
- [ ] Đã chạy **formatter + linter**, không còn warning
- [ ] Đã chạy **toàn bộ test**, tất cả pass
- [ ] Không còn `print()` debug, code comment out, `console.log` thừa
- [ ] Không có secret, file `.env`, file rác (`*.pyc`, `node_modules`, `.DS_Store`)
- [ ] Đã đọc lại **toàn bộ diff** (`git diff --staged`)
- [ ] Tên biến/hàm rõ ràng, không còn tên tạm như `temp`, `test2`, `xxx`
- [ ] Docs/README đã cập nhật nếu thay đổi cách dùng

### 8.2 Checklist cho người review (🟡 M trở lên)

**Tính đúng đắn**

- [ ] Code làm đúng như mô tả PR / yêu cầu issue?
- [ ] Edge case đã được xử lý? (null, rỗng, số âm, overflow, concurrent access)
- [ ] Error handling hợp lý?

**Thiết kế**

- [ ] Có đơn giản hơn được không?
- [ ] Có trùng lặp với code đã có?
- [ ] Đặt đúng chỗ (đúng layer, đúng module)?

**Bảo mật** 🔒

- [ ] Input được validate? Output được encode?
- [ ] Có query nào nối chuỗi không?
- [ ] Kiểm tra quyền (authorization) đầy đủ chưa?
- [ ] Có log/trả về thông tin nhạy cảm không?
- [ ] Dependency mới có đáng tin không? (số người dùng, maintainer, lỗ hổng đã biết)

**Hiệu năng**

- [ ] Có query N+1 không? Có query trong vòng lặp không?
- [ ] Cột hay dùng trong `WHERE`/`JOIN` đã có **index** chưa?
- [ ] Có load toàn bộ dữ liệu lớn vào memory không? (nên dùng pagination/streaming)

**Test & Docs**

- [ ] Có test cho thay đổi này chưa? Test có thật sự kiểm tra đúng thứ cần kiểm tra?
- [ ] Docs cập nhật chưa?

> 💬 **Văn hóa review:** góp ý về **code**, không phải về **người**. Dùng prefix để rõ mức độ:
> `nitpick:` (nhỏ, không chặn merge) · `suggestion:` (gợi ý) · `question:` (hỏi) · `issue:` (vấn đề cần sửa, nên kèm gợi ý) · `praise:` (khen điểm tốt)
>
> 📚 [Google eng-practices: How to write code review comments](https://google.github.io/eng-practices/review/reviewer/comments.html) (Google dùng prefix `Nit:`, `Optional:`, `FYI:`) · [Conventional Comments](https://conventionalcomments.org/)

---

## 9. CI/CD

### 🟡 M: CI tối thiểu (chạy mỗi khi push / mở PR)

```text
Push/PR → Lint → Format check → Unit test → Security scan → ✅ Ready to merge
```

- [ ] Thiết lập CI (GitHub Actions, GitLab CI...); xem [Phụ lục 14.5](#145-github-actions-mẫu)
- [ ] Lint + format check
- [ ] Chạy test + báo cáo coverage
- [ ] Secret scan + dependency scan + SAST 🔒
- [ ] Badge trạng thái CI trong README

### 🔴 L: CI/CD đầy đủ

```text
PR → Lint → Test → SAST/SCA → Build → Container scan → Deploy Staging
   → Integration/E2E → DAST → Manual approval → Deploy Production → Smoke test
```

- [ ] Build artifact **một lần**, deploy cùng artifact đó qua các môi trường
- [ ] Môi trường tách biệt: `dev` → `staging` → `production`
- [ ] Deploy tự động lên staging; production cần approval
- [ ] Chiến lược deploy an toàn: blue-green, canary, hoặc rolling
- [ ] Có kế hoạch **rollback** đã được thử nghiệm
- [ ] 🔒 Bảo mật pipeline: pin action theo **full-length commit SHA**, giới hạn `permissions` của token, dùng OIDC thay vì long-lived credentials
  - 📚 GitHub: pin theo full-length commit SHA hiện là **cách duy nhất** để dùng một action như bản phát hành bất biến, vì tag có thể bị dời sang commit độc hại. [GitHub Docs: Secure use reference](https://docs.github.com/en/actions/reference/security/secure-use#using-third-party-actions)
  - Ví dụ thực tế: vụ action `tj-actions/changed-files` bị chiếm quyền vào tháng 3/2025, làm lộ secret của nhiều repo dùng nó qua tag

---

## 10. Documentation

### 🟢 S

- [ ] `README.md`: mô tả + cách cài + cách chạy + ví dụ

### 🟡 M: README đầy đủ nên có

```markdown
# Tên Project
Mô tả 1–2 câu. Badge CI/coverage/license.

## Features
## Demo / Screenshots
## Tech Stack
## Getting Started
### Prerequisites
### Installation
### Configuration (.env)
## Usage
## Running Tests
## Project Structure
## Roadmap
## Contributing
## License
## Contact
```

- [ ] Docstring cho public API
- [ ] Tài liệu API (OpenAPI/Swagger nếu là REST API)
- [ ] Sơ đồ kiến trúc (Mermaid render trực tiếp được trên GitHub)

### 🔴 L

- [ ] `CHANGELOG.md` (theo [Keep a Changelog](https://keepachangelog.com/))
- [ ] ADR cho các quyết định kiến trúc
- [ ] **Runbook**: xử lý sự cố, cách deploy, cách rollback
- [ ] Onboarding guide cho thành viên mới

---

## 11. Release & Versioning

### 🟡 M

- [ ] Đánh version theo **[Semantic Versioning](https://semver.org/)**: `MAJOR.MINOR.PATCH`
  - `MAJOR`: thay đổi phá vỡ tương thích (breaking change)
  - `MINOR`: thêm tính năng, vẫn tương thích ngược
  - `PATCH`: sửa bug, tương thích ngược
- [ ] Tạo **Git tag** cho mỗi release: `git tag -a v1.2.0 -m "Release v1.2.0"`
- [ ] Viết **release notes** trên GitHub Releases

### 🔴 L

- [ ] Tự động hóa changelog + versioning từ Conventional Commits (`release-please`, `semantic-release`)
- [ ] Ký release / artifact (Sigstore `cosign`) 🔒
- [ ] Đính kèm SBOM

---

## 12. Deploy & Monitoring

### 🟡 M

- [ ] Có hướng dẫn deploy rõ ràng (hoặc script deploy)
- [ ] Health check endpoint (`/health`)
- [ ] Theo dõi lỗi cơ bản (log tập trung, hoặc Sentry)

### 🔴 L

- [ ] **Observability**, 3 trụ cột: **Logs**, **Metrics**, **Traces** (OpenTelemetry)
- [ ] Dashboard + **alerting** cho các chỉ số quan trọng (error rate, latency, CPU/memory)
- [ ] Định nghĩa **SLO/SLI**
- [ ] **Backup** định kỳ + **đã thử restore** (backup chưa từng restore = chưa có backup)
- [ ] Security monitoring 🔒: phát hiện đăng nhập bất thường, alert khi có hành vi đáng ngờ
- [ ] **Incident response plan** + viết **postmortem** không đổ lỗi (blameless) sau sự cố

---

## 13. Maintenance

### 🟢 S

- [ ] Ghi chú lại những gì đã học / vấn đề gặp phải

### 🟡 M

- [ ] Cập nhật dependencies định kỳ (Dependabot / Renovate)
- [ ] Xử lý cảnh báo bảo mật trong thời hạn hợp lý 🔒
- [ ] Dọn dẹp branch đã merge

### 🔴 L

- [ ] Theo dõi và trả dần **technical debt**: ghi thành issue có label riêng, dành thời gian cố định mỗi sprint để xử lý
  - 💭 Tỉ lệ thời gian dành cho tech debt tùy team, không có chuẩn chính thức
- [ ] Review quyền truy cập định kỳ (xóa quyền người đã rời team) 🔒
- [ ] Đánh giá lại threat model khi kiến trúc thay đổi
- [ ] Kế hoạch deprecate tính năng/API cũ có thông báo trước

---

## 14. Phụ lục: Templates

### 14.1 Cấu trúc thư mục mẫu

```text
my-project/
├── .github/
│   ├── workflows/
│   │   └── ci.yml
│   ├── ISSUE_TEMPLATE/
│   └── pull_request_template.md
├── src/                    # Source code chính
│   └── my_project/
│       ├── __init__.py
│       ├── api/            # Tầng giao tiếp (routes, controllers)
│       ├── services/       # Business logic
│       ├── models/         # Data models
│       ├── repositories/   # Truy cập database
│       └── utils/
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
│   └── adr/
├── scripts/                # Script tiện ích (setup, migrate...)
├── .editorconfig
├── .env.example
├── .gitignore
├── .pre-commit-config.yaml
├── CHANGELOG.md
├── CONTRIBUTING.md
├── Dockerfile
├── LICENSE
├── README.md
├── SECURITY.md
└── pyproject.toml          # hoặc package.json, go.mod...
```

### 14.2 Pull Request Template

Lưu tại `.github/pull_request_template.md`:

```markdown
## 📝 Mô tả
<!-- Thay đổi gì và TẠI SAO? -->

Closes #

## 🔧 Loại thay đổi
- [ ] feat: Tính năng mới
- [ ] fix: Sửa bug
- [ ] refactor: Tái cấu trúc
- [ ] docs: Tài liệu
- [ ] security: Vá bảo mật
- [ ] ⚠️ Breaking change

## 🧪 Đã test thế nào?
<!-- Các bước để reviewer tự kiểm tra -->

## 📸 Screenshots (nếu có UI)

## ✅ Checklist
- [ ] Code đã qua lint/format
- [ ] Đã thêm/cập nhật test, tất cả pass
- [ ] Đã tự review diff
- [ ] Không có secret / thông tin nhạy cảm
- [ ] Đã cập nhật docs nếu cần
```

### 14.3 Bug Report Template

Lưu tại `.github/ISSUE_TEMPLATE/bug_report.md`:

```markdown
---
name: Bug report
about: Báo cáo lỗi
labels: bug
---

**Mô tả lỗi**

**Các bước tái hiện**
1.
2.
3.

**Kết quả mong đợi**

**Kết quả thực tế**

**Môi trường**
- OS:
- Version:

**Log / Screenshot**
```

### 14.4 Pre-commit config mẫu

Lưu tại `.pre-commit-config.yaml` (ví dụ cho Python). Cài bằng `pip install pre-commit && pre-commit install`.

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.6.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      - id: detect-private-key        # 🔒 chặn commit private key

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.6.9
    hooks:
      - id: ruff                      # linter
        args: [--fix]
      - id: ruff-format               # formatter

  - repo: https://github.com/gitleaks/gitleaks
    rev: v8.18.4
    hooks:
      - id: gitleaks                  # 🔒 quét secret
```

> ⚠️ Các số `rev` ở trên có thể đã cũ. Chạy `pre-commit autoupdate` để cập nhật lên bản mới nhất.

### 14.5 GitHub Actions mẫu

Lưu tại `.github/workflows/ci.yml` (ví dụ Python):

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read            # 🔒 least privilege cho GITHUB_TOKEN

jobs:
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install ruff pytest pytest-cov

      - name: Lint
        run: ruff check .

      - name: Format check
        run: ruff format --check .

      - name: Test
        run: pytest --cov=src --cov-report=term-missing

  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0     # cần full history để quét secret

      - name: Secret scan
        uses: gitleaks/gitleaks-action@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Dependency audit
        run: |
          pip install pip-audit
          pip-audit -r requirements.txt

      - name: SAST (Bandit)
        run: |
          pip install bandit
          bandit -r src/
```

> ⚠️ Version của action (`@v4`, `@v5`...) có thể thay đổi theo thời gian. Kiểm tra trên GitHub Marketplace.
> Với project 🔴 L, nên pin theo **commit SHA** thay vì tag để chống supply-chain attack.

### 14.6 `.env.example` mẫu

```bash
# Database
DB_HOST=localhost
DB_PORT=5432
DB_NAME=myapp
DB_USER=
DB_PASSWORD=

# Security
SECRET_KEY=          # generate: python -c "import secrets; print(secrets.token_hex(32))"
JWT_EXPIRES_MINUTES=15

# External APIs
API_KEY=
```

---

## ⚡ Quick Checklist: in ra dán cạnh màn hình

**Trước khi code**

- [ ] Hiểu rõ vấn đề, input/output, edge case
- [ ] Đang ở đúng branch (không phải `main`)

**Trong khi code**

- [ ] Tên rõ nghĩa · Function ngắn · Không magic number
- [ ] Validate input · Không nối chuỗi SQL/command · Không hardcode secret
- [ ] Xử lý lỗi cụ thể, không nuốt lỗi

**Trước khi commit**

- [ ] Format + lint sạch
- [ ] Test pass
- [ ] `git diff --staged` đã đọc lại
- [ ] Không có secret, file rác, debug print
- [ ] Commit message theo Conventional Commits

**Trước khi merge**

- [ ] CI xanh
- [ ] Đã được review
- [ ] Docs đã cập nhật

---

## 📚 Tài liệu tham khảo

| Chủ đề | Nguồn |
|--------|-------|
| Commit convention | [Conventional Commits](https://www.conventionalcommits.org/) |
| Versioning | [Semantic Versioning](https://semver.org/) |
| Changelog | [Keep a Changelog](https://keepachangelog.com/) |
| App config & deploy | [The Twelve-Factor App](https://12factor.net/) |
| Web security | [OWASP Top 10:2025](https://owasp.org/Top10/2025/) · [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/) · [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/) |
| Password storage | [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html) · [RFC 9106 (Argon2)](https://www.rfc-editor.org/rfc/rfc9106) |
| Injection | [OWASP SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html) · [Python subprocess security](https://docs.python.org/3/library/subprocess.html#security-considerations) |
| Logging | [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html) |
| Threat modeling | [Microsoft STRIDE](https://learn.microsoft.com/en-us/training/modules/tm-use-a-framework-to-identify-threats-and-find-ways-to-reduce-or-eliminate-risk/1b-threat-modeling-framework) · [OWASP Threat Modeling Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Threat_Modeling_Cheat_Sheet.html) |
| Weakness catalog | [CWE Top 25](https://cwe.mitre.org/top25/) |
| Secure development | [NIST SP 800-218 SSDF v1.1](https://csrc.nist.gov/pubs/sp/800/218/final) (chuẩn của chính phủ Mỹ, công bố 02/2022) |
| Supply chain | [SLSA Framework](https://slsa.dev/) · [OpenSSF Scorecard](https://securityscorecards.dev/) · [GitHub Actions secure use](https://docs.github.com/en/actions/reference/security/secure-use) |
| Git & secrets | [GitHub: Removing sensitive data](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) |
| Testing | [Martin Fowler: Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html) · [Google: Code Coverage Best Practices](https://testing.googleblog.com/2020/08/code-coverage-best-practices.html) |
| Code review | [Google Engineering Practices](https://google.github.io/eng-practices/) · [SmartBear: Code Review Best Practices](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/) |
| Clean code (sách) | *Clean Code* (Robert C. Martin) · *The Pragmatic Programmer* (Hunt & Thomas) |

---

<p align="center"><i>Tài liệu này là "living document": cập nhật khi team học được điều mới.</i></p>
