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
git clone https://github.com/odoo360/runapp.git
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

## توقف سرویس‌ها

برای متوقف کردن پروژه از این دستور استفاده کنید:

```powershell
docker compose down
```
