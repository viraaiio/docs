۳. مدل وظیفه پردازش (Processing Job Model)

این مدل وظایف ارسال شده به موتورهای هوش مصنوعی را برای پیگیری وضعیت و نتایج مدیریت می‌کند.

فیلد (Field)

نوع داده (Data Type)

توضیحات (Description)

jobId

string (UUID)

شناسه یکتای وظیفه پردازش

fileId

string (UUID)

فایل مرتبط با این وظیفه (Foreign Key)

submitterId

string (UUID)

کاربری که وظیفه را شروع کرده است

submissionTime

timestamp

زمان ارسال وظیفه

processingEngine

string

موتور هوش مصنوعی استفاده شده (مثلاً: 'OCR_Engine', 'NLP_Summarizer')

status

enum ('PENDING', 'IN_PROGRESS', 'COMPLETED', 'FAILED')

وضعیت فعلی وظیفه

completionTime

timestamp

زمان پایان موفقیت‌آمیز وظیفه (در صورت وجود)

outputResultId

string (UUID)

شناسه نتیجه ذخیره‌شده (برای بازیابی نتایج نهایی)

metadata

object

پارامترهای اضافی پردازش (مثلاً: تنظیمات دقت، زبان)
