معماری سیستم ViraAI (ViraAI System Architecture)

۱. نمای کلی (Overview)

سیستم ViraAI بر پایه یک معماری سرویس‌گرای ماژولار (Modular Service-Oriented Architecture) طراحی شده است. هسته اصلی شامل یک مایکروفریم‌ورک سبک برای مسیریابی و ارتباطات است که به سرویس‌های تخصصی هوش مصنوعی متصل می‌شود.

۲. لایه‌های معماری (Architectural Layers)

A. لایه ارائه (Presentation Layer - Frontend)

فناوری: React/Next.js (به دلیل پشتیبانی از Server-Side Rendering و عملکرد بالا).

مسئولیت: مدیریت رابط کاربری، احراز هویت کاربر (UI/Session Management)، نمایش داده‌ها، و فراخوانی APIها.

ارتباط: از طریق API Gateway با Backend ارتباط برقرار می‌کند.

B. لایه API Gateway

فناوری: Ruby on Rails (مخزن viraai-core).

مسئولیت: دروازه ورودی برای تمام درخواست‌های Frontend. مدیریت احراز هویت (AuthN/AuthZ)، محدودسازی نرخ درخواست (Rate Limiting)، و مسیریابی (Routing) درخواست‌ها به سرویس‌های صحیح.

ملاحظات: هرگز هیچ منطق تجاری (Business Logic) یا محاسبات سنگین هوش مصنوعی در این لایه اجرا نمی‌شود.

C. لایه سرویس‌های Core (Core Services Layer)

این لایه شامل ماژول‌های محاسباتی اصلی است که در پس‌زمینه (Background) اجرا می‌شوند.

پردازشگر متن (NLP Processor):

فناوری: Python (Flask/Django) + Hugging Face Transformers.

مسئولیت: وظایف سنگین پردازش زبان طبیعی (NLP) مانند تولید متن، خلاصه‌سازی و تحلیل احساسات.

ارتباط: به صورت ناهمگام (Asynchronous) از طریق صف پیام (Queue) با API Gateway تبادل پیام می‌کند.

مدیریت مدل‌ها (Model Manager):

فناوری: Go/Rust (برای عملکرد و Concurrency بالا).

مسئولیت: مدیریت و بارگذاری نسخه‌های مختلف مدل‌های ML/AI، توزیع بار (Load Balancing) بین مدل‌ها و نظارت بر سلامت (Health Check) مدل‌های فعال.

D. لایه داده (Data Layer)

پایگاه داده اصلی: PostgreSQL (برای داده‌های ساختاریافته، احراز هویت و metadata).

پایگاه داده کش: Redis (برای کش کردن نتایج مکرر AI و صف‌های پیام).

ذخیره‌سازی فایل: S3-Compatible Storage (برای ذخیره فایل‌های ورودی/خروجی کاربر و مدل‌های ML حجیم).

۳. جریان داده (Data Flow Example: Text Generation)

درخواست (Request): کاربر در Frontend، متنی را وارد کرده و دکمه "تولید" را می‌زند.

API Gateway: درخواست به API Gateway (viraai-core) می‌رسد، احراز هویت می‌شود، و یک شناسه کار (Job ID) منحصر به فرد ایجاد می‌شود.

صف‌بندی (Queuing): API Gateway، درخواست را به همراه Job ID، به صف پیام (Redis Queue) ارسال می‌کند.

پردازش: سرویس پردازشگر متن (NLP Processor)، درخواست را از صف برداشته، مدل ML مناسب را از Model Manager فراخوانی می‌کند و فرآیند تولید را شروع می‌کند.

نتیجه (Result): پس از اتمام تولید، نتیجه به همراه Job ID در یک جدول موقت در PostgreSQL ذخیره می‌شود.

پاسخ‌دهی: Frontend به صورت متناوب (Polling) وضعیت Job ID را از API Gateway بررسی می‌کند. هنگامی که نتیجه آماده شد، API Gateway آن را بازمی‌گرداند.
