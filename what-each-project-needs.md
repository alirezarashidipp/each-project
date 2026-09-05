my-project/
│
├── app/                         # کد اصلی برنامه
│   ├── __init__.py
│   ├── main.py                  # Entry point
│   │
│   ├── api/                     # API / Routes
│   │   ├── __init__.py
│   │   └── routes.py
│   │
│   ├── services/                # Business logic
│   │   └── ...
│   │
│   ├── models/                  # Models / DB models
│   │   └── ...
│   │
│   ├── schemas/                 # Request/Response schemas
│   │   └── ...
│   │
│   ├── db/                      # Database
│   │   ├── connection.py
│   │   └── ...
│   │
│   ├── core/                    # تنظیمات مرکزی
│   │   ├── config.py
│   │   ├── security.py
│   │   └── logging.py
│   │
│   ├── templates/               # HTML
│   │   ├── index.html
│   │   └── ...
│   │
│   └── static/                  # Frontend assets
│       ├── css/
│       │   └── style.css
│       ├── js/
│       │   └── main.js
│       └── images/
│           └── ...
│
├── tests/                       # تست‌ها
│   ├── __init__.py
│   ├── test_api.py
│   └── ...
│
├── migrations/                  # Database migrations
│   └── ...
│
├── scripts/                     # اسکریپت‌های مدیریتی
│   └── ...
│
├── docs/                        # مستندات اضافی
│   └── ...
│
├── .env                         # Secretها - داخل Git نرود
├── .env.example                 # نمونه متغیرهای محیطی
├── .gitignore
├── .dockerignore
│
├── Dockerfile
├── docker-compose.yml
│
├── pyproject.toml               # Python project + dependencies
├── uv.lock                      # نسخه دقیق dependencyها
│
├── README.md                    # نحوه نصب/اجرا/استفاده
├── LICENSE                      # در صورت نیاز
└── Makefile                     # commandهای پرتکرار - اختیاری
