Animal News – نسخه به‌روزشده با IP مرورگر، لیست سیاه و گفت‌وگو

محتویات:
- index.html: صفحه سفارش، دریافت IP عمومی از ipify در مرورگر، ثبت سفارش از Edge Function و چت مشتری
- admin.html: پنل مدیر، تأیید/رد/جعلی، نمایش و رفع مسدودی IP، پاسخ به پیام‌ها
- animal-news-full-migration.sql: SQL کامل تقویم + قیمت‌گذاری + محدودیت IP + لیست سیاه ۵ روزه + گفت‌وگو
- supabase/functions/create-ad-order/index.ts: Edge Function ثبت سفارش
- supabase/config.toml: غیرفعال‌کردن JWT اجباری برای endpoint عمومی ثبت سفارش

نصب:
1. از Supabase دیتابیس خود نسخه پشتیبان بگیرید.
2. فایل animal-news-full-migration.sql را کامل و یک‌بار در Supabase > SQL Editor اجرا کنید.
   این فایل شامل calendar-migration و migration امنیت/چت است؛ لازم نیست آن دو را جداگانه اجرا کنید.
3. پوشه supabase را در ریشه مخزن پروژه قرار دهید.
4. Supabase CLI را به پروژه متصل کنید و از ریشه پروژه اجرا کنید:
   supabase functions deploy create-ad-order
   اگر CLI فایل config.toml را خودکار از ریشه پروژه می‌خواند، از همین فایل استفاده شود.
5. متغیرهای SUPABASE_URL و SUPABASE_SERVICE_ROLE_KEY باید در محیط Edge Function باشند. هرگز Service Role Key را داخل HTML قرار ندهید.
6. index.html و admin.html و animal-news-logo.jpg را در محل انتشار سایت جایگزین کنید.
7. یک سفارش آزمایشی ثبت کنید؛ سپس پنل مدیر را باز کنید و بررسی کنید IP سفارش نمایش داده شود.
8. با سفارش آزمایشی، دکمه «جعلی 🚫»، فهرست IPهای مسدود، رفع مسدودی و گفت‌وگوی مشتری/مدیر را آزمایش کنید.

نکته مهم درباره IP:
- طبق درخواست، index.html آدرس IP عمومی را در مرورگر از api.ipify.org می‌گیرد و همراه سفارش می‌فرستد.
- این IP توسط سرور مستقلاً تأیید نمی‌شود؛ کاربر فنی می‌تواند درخواست را دستکاری کند یا با VPN/Proxy IP دیگری ارائه کند. بنابراین این روش برای محدودیت ساده مناسب است، نه ضدتقلب قطعی.
- اگر api.ipify.org در دسترس نباشد، ثبت سفارش متوقف می‌شود تا سفارش بدون IP ثبت نشود.
- IP سفارش‌های قدیمی که قبل از این تغییر ثبت شده‌اند خالی است؛ برای آن‌ها امکان مسدودسازی IP وجود ندارد و سیستم خطا می‌دهد.

رفتار:
- مدیر فقط با کلیک «جعلی 🚫» IP همان سفارش را دقیقاً ۵ روز از زمان اقدام مسدود می‌کند.
- محدودیت ۲ سفارش در بازه شناور ۷ روز برای هر IP اعمال می‌شود.
- رفع مسدودی فقط از پنل مدیر و از طریق تابع محافظت‌شده انجام می‌شود.
- چت با کد پیگیری سفارش کار می‌کند؛ کد سفارش را محرمانه نگه دارید.

پیش‌فرض‌های schema که migration به آن‌ها تکیه دارد:
- public.ad_orders با ستون‌های id از نوع uuid، tracking_code، customer_name، contact، ad_date، banner_path، receipt_path، notes، amount_coins، status، created_at و reviewed_at
- public.ad_admins با ستون user_id
- Storage bucket با نام ad-uploads
- وضعیت‌های سفارش: pending / approved / rejected

اگر schema واقعی دیتابیس شما با این موارد فرق دارد، پیش از اجرای SQL باید migration متناسب‌سازی شود.
