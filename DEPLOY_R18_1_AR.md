# نشر صدى العراق R18.1 على Railway

1. افتح نفس خدمة صدى العراق ونفس بيئة `production` في Railway؛ لا تنشئ خدمة جديدة.
2. خذ نسخة احتياطية من إعدادات الخدمة وRailway Volume واحتفظ بكل متغيرات البيئة الحالية.
3. ارفع محتويات هذه الحزمة إلى نفس مستودع GitHub المرتبط بخدمة Railway (أو انشر commit R18.1). لا ترفع ZIP داخل مجلد فرعي؛ يجب أن يكون `package.json` و`server.js` و`railway.json` في جذر الخدمة الحالي.
4. تأكد أن Root Directory في Railway ما زال يشير إلى جذر المشروع. إعداد builder في هذه الحزمة هو `RAILPACK`، وBuild Command هو `npm run build`، وStart Command هو `npm start`، وHealthcheck هو `/healthz`.
5. عند ظهور فشل من نوع تعذّر اتصال build daemon قبل أي خطوة تثبيت، أعد نشر نفس commit بعد التأكد من حالة Railway. لا تغيّر Firebase أو مفاتيح API أو ملفات البيانات بسبب هذا الخطأ وحده.
6. بعد نجاح النشر، افحص `/healthz` ثم `/api/release` وتأكد من أن Build ID هو `SADA-3.2-R18.2-RAILPACK-BUILD-FIX-20261111`.
7. اختبر لوحة الإدارة أولاً، ثم تحقق من ظهور المزود وسلامة رصيده، وبعد ذلك نفّذ طلباً صغيراً آمناً على خدمة تجريبية حقيقية مع مراقبة الطلب والرصيد لدى الموقع والمزود.
8. لا تمسح قواعد Firebase أو مسارات `users` و`orders` و`balances` و`recharge logs`، ولا تفصل Volume ولا تعِد تهيئته.

## تنبيه بتاريخ Railway

ملفات `railway.json` / Config as Code للخدمات القديمة سيجري التوقف عن قراءتها بتاريخ 1 ديسمبر 2026. بعد تثبيت هذا الإصدار، خطط لترحيل إعداد خدمة الإنتاج إلى `.railway/railway.ts` عبر Railway CLI (`railway config pull` أو `railway config migrate`)، ثم افحص خطة التغيير بـ`railway config plan` ولا تطبقها قبل التأكد من عدم وجود حذف لخدمات أو متغيرات أو Volumes غير مقصودة.
