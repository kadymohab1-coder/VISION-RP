# إعداد Firebase لمشروع VISION RP

تم إدخال إعدادات تطبيق الويب التي أرسلها صاحب المشروع في ملف `.env` وربط المشروع بمعرّف Firebase: `visionrp-6e6ba`.

## يلزم تفعيله مرة واحدة من Firebase Console

1. افتح Authentication > Sign-in method، ثم فعّل Email/Password.
2. افتح Firestore Database وأنشئ قاعدة بيانات (Production mode).
3. من صفحة Firestore > Rules، انشر محتوى الملف `firestore.rules`.
4. من Authentication > Settings > Authorized domains، تأكد من إضافة نطاق الاستضافة بعد نشر الموقع.

## البناء والنشر

من داخل مجلد المشروع:

```bash
npm install
npm run build
```

للنشر عبر Firebase CLI:

```bash
npm install -g firebase-tools
firebase login
firebase deploy --only hosting,firestore:rules
```

## ملاحظات مهمة

- ملف `.env` يحتوي إعدادات تطبيق الويب. Firebase Web API key ليس كلمة مرور إدارية، لكنه يفضّل تقييده من Google Cloud Console بنطاقات موقعك وواجهات Firebase المطلوبة.
- لا تضع Service Account JSON أو مفاتيح Admin داخل تطبيق المتصفح.
- لا يمكن تأكيد تسجيل الدخول أو حفظ البيانات فعلياً قبل تفعيل Email/Password وإنشاء Firestore ونشر القواعد في لوحة Firebase.
