API Specification (مشخصات API)

این سند جزئیات endpoints و روش‌های دسترسی به قابلیت‌های اصلی پلتفرم Vira AI را شرح می‌دهد.

Base URL

تمام درخواست‌ها باید به Base URL زیر ارسال شوند:

https://api.viraaiio.com/v1

احراز هویت (Authentication)

تمام درخواست‌های API باید شامل یک هدر Authorization باشند که حاوی توکن Bearer است.

هدر

مقدار نمونه

توضیحات

Authorization

Bearer <YOUR_API_KEY>

کلید API ارائه شده توسط پلتفرم.

Content-Type

application/json

الزامی برای درخواست‌های POST و PUT.

درخواست‌هایی که بدون احراز هویت یا با توکن نامعتبر ارسال شوند، با کد وضعیت 401 Unauthorized مواجه خواهند شد.

1. مدل‌ها و پیش‌بینی‌ها (Models and Predictions)

این بخش به مدیریت و اجرای مدل‌های یادگیری ماشین استقرار یافته اختصاص دارد.

۱.۱. اجرای پیش‌بینی (Run Prediction)

پیش‌بینی را بر روی یک مدل مشخص اجرا می‌کند.

مشخصه

مقدار

Endpoint

POST /models/{model_id}/predict

مورد استفاده

اجرای استنتاج (Inference) بر روی داده‌های ورودی جدید.

پارامترهای مسیر (Path Parameters):

پارامتر

نوع

توضیحات

model_id

string

شناسه‌ی یکتا (UUID) مدل مورد نظر برای اجرا.

بدنه درخواست (Request Body - application/json):

{
  "inputs": [
    {
      "feature_name": "temperature",
      "value": 25.5
    },
    {
      "feature_name": "humidity",
      "value": 0.75
    }
  ]
}


بدنه پاسخ موفق (Success Response - 200 OK):

{
  "model_id": "mdl-1a2b3c4d5e",
  "prediction_time": "2024-11-19T09:30:00Z",
  "results": [
    {
      "output_name": "risk_score",
      "predicted_value": 0.89,
      "confidence": 0.95
    }
  ]
}


2. مدیریت داده (Data Management)

این بخش برای آپلود، مشاهده و مدیریت مجموعه‌های داده (Datasets) استفاده می‌شود.

۲.۱. آپلود مجموعه داده (Upload Dataset)

یک فایل مجموعه داده جدید را آپلود می‌کند. (در این پیاده‌سازی از multipart/form-data استفاده می‌شود)

مشخصه

مقدار

Endpoint

POST /data/upload

مورد استفاده

ارسال فایل داده برای آموزش یا استفاده در پلتفرم.

بدنه درخواست (Request Body - multipart/form-data):

فیلد

نوع

توضیحات

file

file

فایل داده (مثلاً CSV، JSON)

dataset_name

string

نام دلخواه برای مجموعه داده.

بدنه پاسخ موفق (Success Response - 201 Created):

{
  "dataset_id": "ds-f9e8d7c6b5",
  "name": "sales_q4_2024",
  "status": "Processing",
  "uploaded_at": "2024-11-19T09:45:00Z"
}


۲.۲. دریافت مشخصات مجموعه داده (Get Dataset Metadata)

مشخصات و متادیتای یک مجموعه داده خاص را بازیابی می‌کند.

مشخصه

مقدار

Endpoint

GET /data/{dataset_id}

مورد استفاده

بررسی وضعیت و ساختار داده آپلود شده.

پارامترهای مسیر (Path Parameters):

پارامتر

نوع

توضیحات

dataset_id

string

شناسه‌ی یکتای مجموعه داده.

بدنه پاسخ موفق (Success Response - 200 OK):

{
  "dataset_id": "ds-f9e8d7c6b5",
  "name": "sales_q4_2024",
  "row_count": 12500,
  "column_count": 15,
  "status": "Ready",
  "uploaded_by": "user-a1b2c3d4",
  "schema": [
    {"column_name": "date", "data_type": "date"},
    {"column_name": "revenue", "data_type": "float"},
    // ... سایر ستون‌ها
  ]
}
