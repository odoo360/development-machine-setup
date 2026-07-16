# راهنمای نصب و اجرای محلی

این پروژه با Docker اجرا می‌شود و برای راه‌اندازی آن به Git و Docker Desktop نیاز دارید.

## پیش‌نیازها

اگر روی ویندوز هستید، این ابزارها را با `winget` نصب کنید:

```powershell
winget install --id Git.Git -e
winget install --id Docker.DockerDesktop -e
```

بعد از نصب Docker Desktop، آن را اجرا کنید و مطمئن شوید Docker روشن است.

## کلون کردن مخزن

```powershell
git clone https://github.com/odoo360/development-machine-setup.git
cd runapp
```

## اجرای پروژه

فایل `compose.yaml` برای اجرای سرویس‌ها استفاده می‌شود. پروژه را با این دستور بالا بیاورید:

```powershell
docker compose up -d
```

اگر برای اولین بار اجرا می‌کنید، Docker ایمیج‌ها را دانلود می‌کند و چند دقیقه زمان می‌برد.

## باز کردن در مرورگر

بعد از بالا آمدن سرویس‌ها، این آدرس را در مرورگر باز کنید:

```text
http://localhost:8069
```

## تنظیم اولیه دیتابیس (Database Selector)

در اجرای اول، به صفحه‌ی زیر هدایت می‌شوید:

```text
http://localhost:8069/web/database/selector
```

اگر پیام هشدار مربوط به Database Manager را دیدید، مقدار `Master Password` را از همان پیام کپی کنید.


مقادیر پیشنهادی برای ساخت دیتابیس:

- Master Password: همان مقداری که بالای فرم نمایش داده می‌شود (مثلا `8v6h-w59j-6zuj`)
- Database Name: یک نام ساده و بدون فاصله، مثلا `odoo360_dev`
- Email: ایمیل ادمین، مثلا `admin@example.com`
- Password: رمز عبور کاربر ادمین Odoo
- Phone Number: اختیاری
- Language: مثلا `Farsi` یا زبان دلخواه
- Country: کشور دلخواه
- Demo Data: برای محیط توسعه روشن باشد، برای محیط واقعی خاموش باشد


## توقف سرویس‌ها

برای متوقف کردن پروژه از این دستور استفاده کنید:

```powershell
docker compose down
```
