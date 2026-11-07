# سامانه ثبت سفارش تبلیغات

## فایل‌ها
- `index.html`: فرم ثبت سفارش و پیگیری کد
- `admin.html`: پنل مدیریت
- `supabase-setup.sql`: ساخت دیتابیس و سیاست‌های دسترسی

## اطلاعات اتصال
Project URL و Publishable key که ارائه کرده‌اید در هر دو فایل HTML تنظیم شده‌اند.
کلید Publishable/anon برای مرورگر طراحی شده است. هرگز `service_role` را در فایل‌های سایت قرار ندهید.

## راه‌اندازی
1. فایل `supabase-setup.sql` را در SQL Editor اجرا کنید (اگر قبلاً اجرا کرده‌اید، دوباره لازم نیست).
2. در Authentication > Users حساب مدیر را بسازید.
3. UUID مدیر را با دستور زیر به جدول مدیران اضافه کنید:
   `insert into public.ad_admins(user_id) values ('UUID') on conflict do nothing;`
4. در Authentication > URL Configuration آدرس سایت GitHub Pages را به Site URL و Redirect URLs اضافه کنید، اگر ورود مدیر از آن آدرس استفاده می‌کند.
5. فایل‌های `index.html` و `admin.html` را در ریشه مخزن GitHub قرار دهید و GitHub Pages را فعال کنید.
6. صفحه مشتری `index.html` و صفحه مدیریت `admin.html` است.

## نکات مهم
- فایل‌های آپلودی در bucket خصوصی `ad-uploads` ذخیره می‌شوند.
- هر تصویر حداکثر ۵ مگابایت است.
- تعرفه ثابت ۱۵٬۰۰۰ سکه است.
- پنل مدیریت با حساب Supabase Auth کار می‌کند.
- در صورت نمایش خطای RLS یا permission، SQL setup و شناسه مدیر را بررسی کنید.
