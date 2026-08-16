بله. بهترین روش این است که یک «Production Project Checklist» ثابت داشته باشی و برای هر پروژه از روی آن جلو بروی.

همه پروژه‌ها به تمام موارد زیر نیاز ندارند؛ موارد را به سه سطح تقسیم کرده‌ام:

- **ضروری:** تقریباً برای هر پروژه حرفه‌ای
- **قبل از Production:** پیش از ارائه به کاربر واقعی
- **اختیاری:** وابسته به نوع و اندازه پروژه

# 1. فایل‌های اصلی Repository

## ضروری

- [ ] `README.md`
- [ ] `LICENSE`
- [ ] `.gitignore`
- [ ] `.gitattributes`
- [ ] `.editorconfig`
- [ ] فایل مدیریت وابستگی‌ها:
  - Python: `pyproject.toml`
  - Node.js: `package.json`
- [ ] فایل Lock وابستگی‌ها:
  - `uv.lock`
  - `poetry.lock`
  - `package-lock.json`
  - یا معادل آن
- [ ] `.env.example`
- [ ] `CHANGELOG.md`

## برای پروژه عمومی GitHub

- [ ] `CONTRIBUTING.md`
- [ ] `CODE_OF_CONDUCT.md`
- [ ] `SECURITY.md`
- [ ] `SUPPORT.md`
- [ ] `CITATION.cff` برای پروژه‌های تحقیقاتی
- [ ] `GOVERNANCE.md` برای پروژه‌های تیمی یا بزرگ

GitHub نیز وجود README، License، راهنمای مشارکت، Code of Conduct و Security Policy را جزو استانداردهای یک repository حرفه‌ای می‌داند. [GitHub repository best practices](https://docs.github.com/en/repositories/creating-and-managing-repositories/best-practices-for-repositories)، [Community health files](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)

# 2. یک README حرفه‌ای

`README.md` حداقل باید این بخش‌ها را داشته باشد:

- [ ] نام و توضیح یک‌خطی پروژه
- [ ] تصویر یا GIF واقعی محصول
- [ ] Badge مربوط به CI، License و نسخه
- [ ] مسئله‌ای که پروژه حل می‌کند
- [ ] قابلیت‌های اصلی
- [ ] Quick Start
- [ ] اجرای Docker
- [ ] نصب Native
- [ ] نمونه استفاده
- [ ] معماری خلاصه
- [ ] ساختار پوشه‌ها
- [ ] تنظیم متغیرهای محیطی
- [ ] اجرای تست‌ها
- [ ] محدودیت‌های شناخته‌شده
- [ ] ملاحظات امنیت و حریم خصوصی
- [ ] لینک مستندات کامل
- [ ] روش مشارکت
- [ ] License

قاعده خوب:

> یک توسعه‌دهنده جدید باید بتواند با خواندن README پروژه را بفهمد، نصب کند و اجرا کند.

# 3. فایل‌های مخصوص AI Agent

## `AGENTS.md` در ریشه

فایل اصلی باید کوتاه باشد و موارد زیر را مشخص کند:

- [ ] هدف پروژه
- [ ] ترتیب کار Agent
- [ ] دستورات نصب، تست و اجرا
- [ ] ساختار معماری
- [ ] مرزهای امنیتی
- [ ] استاندارد کدنویسی
- [ ] Definition of Done
- [ ] فایل‌هایی که Agent باید در شرایط خاص بخواند
- [ ] محدودیت‌های مربوط به Git و انتشار

نمونه:

```markdown
# Repository Guide

## Working Loop

1. Read README.md and the relevant module.
2. Add a failing test before changing behavior.
3. Implement the smallest readable change.
4. Run the narrow test.
5. Run the complete quality suite.
6. Update documentation.

## Boundaries

- Never commit secrets.
- Preserve backward compatibility.
- Keep user data private.
- Avoid unnecessary abstractions.

## Context Pointers

- Read docs/architecture.md before changing module boundaries.
- Read docs/security.md before changing authentication.
- Read docs/database.md before changing schemas or migrations.
```

## چندین `AGENTS.md`

فقط وقتی بخش‌ها قوانین متفاوت دارند:

```text
project/
├── AGENTS.md
├── backend/
│   └── AGENTS.md
├── frontend/
│   └── AGENTS.md
├── infrastructure/
│   └── AGENTS.md
└── docs/
    └── AGENTS.md
```

مثلاً:

### `backend/AGENTS.md`

- ساختار API
- قوانین Database
- Validation
- Migration
- تست‌های Backend
- Error handling

### `frontend/AGENTS.md`

- Design system
- Accessibility
- Responsive breakpoints
- Component conventions
- Browser tests

### `infrastructure/AGENTS.md`

- Docker
- Deployment
- Secrets
- Infrastructure as Code
- Rollback rules

### `docs/AGENTS.md`

- لحن مستندات
- زبان
- قالب نمونه‌ها
- جلوگیری از ادعاهای بدون مدرک

نکته مهم:

> برای هر پوشه `AGENTS.md` نساز. فقط زمانی اضافه کن که قوانین آن بخش واقعاً متفاوت باشند.

# 4. ساختار کد

یک ساختار عمومی مناسب:

```text
project/
├── src/
│   └── project_name/
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── contract/
│   └── e2e/
├── docs/
├── scripts/
├── migrations/
├── infrastructure/
├── .github/
├── Dockerfile
├── compose.yaml
├── pyproject.toml
├── README.md
└── AGENTS.md
```

## اصول ساختار

- [ ] کد اصلی داخل `src/`
- [ ] تست‌ها از کد اصلی جدا
- [ ] تنظیمات از منطق برنامه جدا
- [ ] اسکریپت‌های عملیاتی داخل `scripts/`
- [ ] Migrationها نسخه‌بندی‌شده
- [ ] فایل‌های تولیدشده داخل Git نباشند
- [ ] هر ماژول مسئولیت مشخص داشته باشد

# 5. مستندات داخل `docs/`

## ضروری

```text
docs/
├── architecture.md
├── development.md
├── deployment.md
├── configuration.md
├── troubleshooting.md
└── security.md
```

## بسته به پروژه

- [ ] `api.md`
- [ ] `database.md`
- [ ] `privacy.md`
- [ ] `data-retention.md`
- [ ] `authentication.md`
- [ ] `authorization.md`
- [ ] `observability.md`
- [ ] `performance.md`
- [ ] `backup-and-restore.md`
- [ ] `incident-response.md`
- [ ] `runbook.md`
- [ ] `release-process.md`
- [ ] `adr/` برای تصمیم‌های معماری

نمونه ADR:

```text
docs/adr/
├── 0001-use-fastapi.md
├── 0002-use-postgresql.md
└── 0003-local-first-processing.md
```

هر ADR باید بگوید:

- مسئله چه بود؟
- چه گزینه‌هایی داشتیم؟
- چه تصمیمی گرفتیم؟
- چرا؟
- پیامد تصمیم چیست؟

# 6. تست‌ها

## حداقل ضروری

- [ ] Unit tests
- [ ] Integration tests
- [ ] تست Validation
- [ ] تست Error handling
- [ ] تست ورودی‌های مرزی
- [ ] تست Regression برای Bugها
- [ ] Coverage report

## قبل از Production

- [ ] API contract tests
- [ ] End-to-End tests
- [ ] Browser tests
- [ ] Database migration tests
- [ ] Smoke tests
- [ ] تست نصب Package
- [ ] تست Docker image
- [ ] تست startup و shutdown
- [ ] تست permission و authentication
- [ ] تست مسیرهای failure

## وابسته به پروژه

- [ ] Performance tests
- [ ] Load tests
- [ ] Security tests
- [ ] Accessibility tests
- [ ] Visual regression tests
- [ ] Chaos/recovery tests

Coverage بالا به‌تنهایی کافی نیست. مهم‌تر این است که مسیرهای بحرانی و failureها تست شده باشند.

# 7. ابزارهای کنترل کیفیت

- [ ] Formatter
- [ ] Linter
- [ ] Type checker
- [ ] Test runner
- [ ] Coverage
- [ ] Dependency audit
- [ ] Secret scanning
- [ ] Static security analysis
- [ ] Pre-commit hooks

مثال Python:

```text
Ruff          Formatter + Linter
MyPy          Type checking
Pytest        Tests
pytest-cov    Coverage
Bandit        Security linting
pip-audit     Dependency vulnerabilities
```

مثال JavaScript:

```text
Prettier
ESLint
TypeScript
Vitest/Jest
Playwright
npm audit
```

# 8. CI

فایل معمول:

```text
.github/workflows/ci.yml
```

CI باید در هر Pull Request این موارد را بررسی کند:

- [ ] نصب وابستگی‌ها از ابتدا
- [ ] Format check
- [ ] Lint
- [ ] Type check
- [ ] Unit tests
- [ ] Integration tests
- [ ] Coverage threshold
- [ ] Build برنامه
- [ ] Build پکیج
- [ ] Build Docker image
- [ ] Migration validation
- [ ] Dependency audit
- [ ] Secret scan

## امنیت CI

- [ ] `permissions` حداقلی و صریح
- [ ] عدم چاپ Secret در Log
- [ ] عدم استفاده مستقیم از ورودی کاربر در Shell
- [ ] Pin کردن Third-party Actions به commit SHA برای پروژه‌های حساس
- [ ] جداکردن CI از Deployment
- [ ] محدودکردن Deployment به branch و environment مشخص
- [ ] استفاده از OIDC به‌جای Secretهای Cloud طولانی‌مدت

GitHub توصیه می‌کند permissionهای workflow حداقلی باشند و Actionهای خارجی در پروژه‌های حساس به SHA کامل pin شوند. [GitHub Actions security hardening](https://docs.github.com/en/code-security/tutorials/secure-your-organization/protect-against-threats)

# 9. Docker

## فایل‌ها

- [ ] `Dockerfile`
- [ ] `.dockerignore`
- [ ] `compose.yaml`
- [ ] Health check
- [ ] Development override در صورت نیاز

## Dockerfile حرفه‌ای

- [ ] Base image مشخص و محدود
- [ ] Multi-stage build در صورت نیاز
- [ ] Cache-friendly layer ordering
- [ ] اجرای برنامه با کاربر non-root
- [ ] UID/GID مشخص در صورت نیاز
- [ ] عدم قرار دادن Secret داخل image
- [ ] حداقل packageهای سیستم‌عامل
- [ ] `WORKDIR` مطلق
- [ ] Health check
- [ ] Signal handling صحیح
- [ ] Graceful shutdown
- [ ] Image کوچک و قابل بازتولید
- [ ] اسکن vulnerability

Docker نیز اجرای سرویس با `USER` غیر Root و استفاده از `WORKDIR` مطلق را توصیه می‌کند. [Docker build best practices](https://docs.docker.com/build/building/best-practices/)

## `compose.yaml`

- [ ] Portهای مشخص
- [ ] Volumeهای لازم
- [ ] Networkهای لازم
- [ ] Health check
- [ ] Restart policy
- [ ] Resource limits
- [ ] `read_only: true` در صورت امکان
- [ ] `no-new-privileges`
- [ ] Dependency health conditions
- [ ] عدم قرار دادن Secret مستقیم در فایل

# 10. Configuration و Secretها

- [ ] تنظیمات از Environment Variables خوانده شوند
- [ ] `.env` داخل Git نباشد
- [ ] `.env.example` فقط نام متغیرها را داشته باشد
- [ ] برنامه هنگام startup تنظیمات را validate کند
- [ ] Secret پیش‌فرض ناامن نداشته باشد
- [ ] Production secrets داخل Secret Manager باشند
- [ ] تنظیمات Development، Test و Production جدا باشند
- [ ] مقادیر حساس در Log نمایش داده نشوند

نمونه:

```env
APP_ENV=development
APP_PORT=8000
DATABASE_URL=
SECRET_KEY=
LOG_LEVEL=INFO
```

# 11. API حرفه‌ای

اگر پروژه API دارد:

- [ ] OpenAPI/Swagger
- [ ] Schemaهای Request و Response
- [ ] Validation
- [ ] نسخه‌بندی API
- [ ] Error format ثابت
- [ ] Status code صحیح
- [ ] Pagination
- [ ] Timeout
- [ ] Rate limiting
- [ ] Authentication
- [ ] Authorization
- [ ] CORS محدود
- [ ] Request size limit
- [ ] Idempotency برای عملیات حساس
- [ ] Correlation/Request ID
- [ ] API contract tests

نمونه Error Contract:

```json
{
  "error": {
    "code": "invalid_input",
    "message": "Email address is invalid",
    "request_id": "req_123"
  }
}
```

# 12. Database و داده‌ها

اگر پروژه Database دارد:

- [ ] Schema مشخص
- [ ] Migration versioned
- [ ] Indexهای لازم
- [ ] Transaction boundaries
- [ ] Connection pooling
- [ ] Query timeout
- [ ] Seed data فقط برای Development
- [ ] Backup
- [ ] Restore
- [ ] تست واقعی Restore
- [ ] Retention policy
- [ ] حذف امن داده کاربر
- [ ] Migration rollback strategy
- [ ] جلوگیری از N+1 Query
- [ ] Audit log برای عملیات حساس

مهم:

> داشتن Backup کافی نیست؛ باید Restore نیز آزمایش شده باشد.

# 13. Security

## ضروری

- [ ] Threat model
- [ ] Input validation
- [ ] Authentication
- [ ] Authorization
- [ ] Rate limiting
- [ ] Secure headers
- [ ] Dependency scanning
- [ ] Secret scanning
- [ ] Code scanning
- [ ] `SECURITY.md`
- [ ] سیاست گزارش آسیب‌پذیری
- [ ] عدم ذخیره Password خام
- [ ] محدودسازی Uploadها
- [ ] جلوگیری از SQL Injection
- [ ] جلوگیری از Command Injection
- [ ] جلوگیری از Path Traversal
- [ ] CORS/CSRF صحیح
- [ ] Redaction اطلاعات حساس در Log

برای Repository عمومی، GitHub فعال‌کردن Dependabot alerts، Secret scanning، Push protection و Code scanning را توصیه می‌کند. [GitHub security recommendations](https://docs.github.com/en/repositories/creating-and-managing-repositories/best-practices-for-repositories)

# 14. Observability

قبل از Production باید بتوانی بفهمی برنامه چه مشکلی دارد.

- [ ] Structured logs
- [ ] Log level
- [ ] Request ID
- [ ] Metrics
- [ ] Error tracking
- [ ] Distributed tracing در سیستم‌های چندسرویسی
- [ ] Health endpoint
- [ ] Readiness endpoint
- [ ] Liveness endpoint
- [ ] Dashboard
- [ ] Alerts
- [ ] SLO/SLI
- [ ] Audit logs
- [ ] حذف اطلاعات حساس از Log

حداقل endpointها:

```text
/health/live
/health/ready
/metrics
```

# 15. Reliability

- [ ] Timeout برای عملیات شبکه
- [ ] Retry فقط برای خطاهای موقت
- [ ] Exponential backoff
- [ ] Circuit breaker در صورت نیاز
- [ ] Graceful shutdown
- [ ] Idempotent operations
- [ ] محدودسازی Queue
- [ ] جلوگیری از مصرف نامحدود حافظه
- [ ] Request size limits
- [ ] Connection limits
- [ ] Recovery procedure
- [ ] Disaster recovery plan
- [ ] تست failure dependencyها

Retry کورکورانه خطرناک است. عملیات غیر idempotent ممکن است چند بار اجرا شود.

# 16. Deployment و Release

- [ ] محیط‌های Development، Staging و Production
- [ ] Infrastructure as Code
- [ ] CD workflow جدا
- [ ] Approval برای Production
- [ ] Database migration مرحله‌بندی‌شده
- [ ] Rollback strategy
- [ ] Release tags
- [ ] Semantic Versioning
- [ ] GitHub Releases
- [ ] Release notes
- [ ] Artifact signing
- [ ] SBOM
- [ ] Build provenance
- [ ] Feature flags برای تغییرات پرریسک
- [ ] Smoke test بعد از Deployment

# 17. GitHub Repository Settings

- [ ] توضیح کوتاه Repository
- [ ] Topicهای مرتبط
- [ ] Social preview
- [ ] Branch protection
- [ ] Pull Request اجباری
- [ ] CI اجباری قبل از Merge
- [ ] Review اجباری
- [ ] جلوگیری از Force Push روی `main`
- [ ] حذف خودکار branch بعد از Merge
- [ ] Dependabot
- [ ] Secret scanning
- [ ] Push protection
- [ ] Code scanning
- [ ] Private vulnerability reporting
- [ ] Issue templates
- [ ] Pull Request template
- [ ] `CODEOWNERS` برای پروژه تیمی

ساختار:

```text
.github/
├── workflows/
│   ├── ci.yml
│   ├── security.yml
│   └── release.yml
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml
│   ├── feature_request.yml
│   └── config.yml
├── PULL_REQUEST_TEMPLATE.md
├── dependabot.yml
└── CODEOWNERS
```

# 18. چک‌لیست حداقلی برای شروع پروژه

اگر می‌خواهی پروژه را امروز شروع کنی، ابتدا این‌ها را بساز:

```text
project/
├── src/
├── tests/
├── docs/
│   ├── architecture.md
│   └── development.md
├── .github/
│   └── workflows/
│       └── ci.yml
├── .env.example
├── .gitignore
├── .editorconfig
├── AGENTS.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── Dockerfile
├── compose.yaml
├── LICENSE
├── README.md
└── pyproject.toml
```

# 19. چک‌لیست قبل از اولین Push

- [ ] README قابل‌فهم است
- [ ] پروژه با یک دستور اجرا می‌شود
- [ ] Secret وجود ندارد
- [ ] `.gitignore` صحیح است
- [ ] تست‌ها پاس می‌شوند
- [ ] Formatter و Linter پاس می‌شوند
- [ ] Docker build می‌شود
- [ ] CI تعریف شده
- [ ] License مشخص است
- [ ] Commitها خوانا هستند

# 20. چک‌لیست قبل از Production

- [ ] CI کاملاً سبز
- [ ] تست Integration و E2E
- [ ] Security review
- [ ] Threat model
- [ ] Secret manager
- [ ] Health/Readiness checks
- [ ] Logs، Metrics و Alerts
- [ ] Backup و Restore آزمایش‌شده
- [ ] Load test
- [ ] Rate limiting
- [ ] Timeout و Retry
- [ ] Migration plan
- [ ] Rollback plan
- [ ] Incident runbook
- [ ] Privacy و retention policy
- [ ] Staging verification
- [ ] Release tag
- [ ] Production smoke test

## قانون نهایی

یک پروژه فقط زمانی «Production-ready» است که:

```text
Can build
+ Can test
+ Can deploy
+ Can observe
+ Can recover
+ Can secure
+ Can maintain
```

یعنی صرفاً داشتن `README`، Docker و CI کافی نیست؛ باید بتوانی پروژه را اجرا، بررسی، منتشر، مانیتور، بازیابی و نگهداری کنی.
