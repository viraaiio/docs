مسیرهای اصلی API و تخصیص کامپوننت (API Paths & Component Allocation)

این سند مروری سریع بر مسیرهای اصلی API و اینکه کدام بخش از معماری (Frontend، Backend یا Core) مسئول رسیدگی به درخواست (Request) و پاسخ (Response) نهایی آن است، ارائه می‌دهد.

۱. عملیات مربوط به احراز هویت (Authentication Operations)

مسیر (Path)

متد (Method)

وظیفه (Function)

کامپوننت مسئول (Responsible Component)

/auth/login

POST

ورود کاربر و صدور توکن (Token Issuance)

ViraAI Core (Backend)

/auth/register

POST

ثبت‌نام کاربر جدید

ViraAI Core (Backend)

/auth/logout

POST

خروج از سیستم و ابطال توکن

ViraAI Core (Backend)

/auth/refresh

POST

دریافت توکن دسترسی جدید

ViraAI Core (Backend)

۲. عملیات مدیریت فایل‌های هسته (Core File Management)

این مسیرها وظیفه آپلود، بازیابی و مدیریت فایل‌های اصلی را بر عهده دارند که توسط سیستم هوش مصنوعی استفاده می‌شوند.

مسیر (Path)

متد (Method)

وظیفه (Function)

کامپوننت مسئول (Responsible Component)

/files/upload

POST

آپلود فایل جدید به فضای ابری

API Gateway -> ViraAI Core

/files/{fileId}

GET

دریافت متادیتای فایل مشخص

ViraAI Core (Backend)

/files/{fileId}/download

GET

دانلود فایل

API Gateway -> ViraAI Core

/files/{fileId}

DELETE

حذف یک فایل

ViraAI Core (Backend)

۳. عملیات هوش مصنوعی و پردازش (AI & Processing Operations)

این مسیرها ارتباط مستقیم با موتورهای پردازشی و هوش مصنوعی ViraAI برقرار می‌کنند.

مسیر (Path)

متد (Method)

وظیفه (Function)

کامپوننت مسئول (Responsible Component)

/ai/process

POST

ارسال یک فایل برای پردازش اولیه توسط AI

ViraAI Core -> AI Processing Engine

/ai/status/{jobId}

GET

بررسی وضعیت یک کار پردازشی

ViraAI Core

/ai/results/{jobId}

GET

دریافت نتایج نهایی پردازش AI

ViraAI Core

۴. رابط کاربری و نمایش (Frontend Endpoints)

این مسیرها توسط Frontend (Next.js/React) برای رندرینگ و مدیریت جلسه (Session Management) استفاده می‌شوند.

مسیر (Path)

متد (Method)

وظیفه (Function)

کامپوننت مسئول (Responsible Component)

/dashboard

GET

صفحه اصلی و داشبورد کاربر

Presentation Layer (Frontend)

/settings

GET

تنظیمات حساب کاربری

Presentation Layer (Frontend)

/docs

GET

دسترسی به اسناد پروژه

Presentation Layer (Frontend)
