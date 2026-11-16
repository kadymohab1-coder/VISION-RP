# VISION RP — Real Life Roleplay

مشروع لعبة مدينة ثلاثية الأبعاد يعمل عبر المتصفح باستخدام React + Vite + Three.js. تم اعتماد **Firebase** (وليس Tencent CloudBase) للحسابات وقاعدة بيانات ملفات اللاعبين.

## الخدمات الموصولة برمجياً
- Firebase Authentication: إنشاء حساب بالبريد وكلمة المرور، وتسجيل الدخول والخروج.
- Cloud Firestore: حفظ بيانات حساب اللاعب في `users/{uid}` وحفظ ملف الشخصية في `playerProfiles/{uid}`.
- Firebase Auth يستخدم جلسة محلية مستمرة في المتصفح حتى لا يحتاج اللاعب لتسجيل الدخول كل مرة على نفس الجهاز.
- حفظ ملف الشخصية إلى Firestore عند دخول المدينة.

## إعداد Firebase لأول مرة
1. افتح Firebase Console وأنشئ مشروعاً جديداً.
2. من إعدادات المشروع أضف تطبيق Web وانسخ إعدادات Firebase الظاهرة.
3. من **Build → Authentication → Get started → Sign-in method** فعّل **Email/Password**.
4. من **Build → Firestore Database** أنشئ قاعدة بيانات.
5. انسخ `.env.example` إلى `.env` واملأ القيم من إعدادات تطبيق الويب:

```env
VITE_FIREBASE_API_KEY=...
VITE_FIREBASE_AUTH_DOMAIN=your-project-id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your-project-id
VITE_FIREBASE_STORAGE_BUCKET=your-project-id.firebasestorage.app
VITE_FIREBASE_MESSAGING_SENDER_ID=...
VITE_FIREBASE_APP_ID=...
```

6. انشر قواعد الأمان الموجودة في `firestore.rules`. لا تستخدم قواعد الاختبار المفتوحة في مشروع منشور.
7. ثبّت الحزم وابنِ المشروع:

```bash
npm install
npm run build
npm run dev
```

## نشر الموقع على Firebase Hosting (اختياري)
المشروع يتضمن `firebase.json` و`.firebaserc.example`. ثبّت Firebase CLI على جهازك (مثلاً عبر `npm install -g firebase-tools`)، سجّل الدخول، انسخ `.firebaserc.example` إلى `.firebaserc` واستبدل `YOUR_FIREBASE_PROJECT_ID` بمعرّف مشروعك، ثم نفّذ:

```bash
npm run build
firebase deploy --only hosting,firestore:rules
```

## ملاحظات مهمة عن حالة التنفيذ
- كود Firebase Authentication وFirestore مضاف للمشروع، لكن لا يمكن إجراء تسجيل دخول حقيقي أو اختبار قاعدة بياناتك دون إعداد مشروع Firebase الخاص بك وملف `.env`.
- لا توجد مفاتيح سرية مطلوبة في واجهة الويب؛ إعداد Firebase Web config مخصص للعميل، والحماية الفعلية تتم بقواعد Firestore. لا تضع Service Account JSON أو مفاتيح Admin داخل ملفات الواجهة.
- يتم حفظ ملف الشخصية عند دخول المدينة. بعض تقدم اللعب الذي لم تتم إضافته إلى `CharacterProfile` أو لم يُربط بعملية الحفظ يظل محلياً حالياً.
- لم يتم تأكيد البناء في بيئة العمل هذه؛ يلزم تنفيذ `npm install` ثم `npm run build` بعد توفر اتصال الحزم.
