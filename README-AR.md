# نبض البيتكوين — مشروع تطبيق iPhone

هذا ملف مشروع SwiftUI أصلي، وليس IPA جاهزاً للتثبيت. يحتاج iOS 16 أو أحدث.

## المزايا
- واجهة عربية داكنة مع سعر BTC/USD من Coinbase Exchange.
- تحديث كل دقيقة أثناء فتح التطبيق، وسحب للتحديث، ووقت آخر صفقة ظاهر.
- شارت يومي لأسبوع، شهر، أو 90 يوماً.
- متوسطا 20 و50 يوماً، RSI وفق Wilder، وأعلى وأدنى 20 يوماً كمستويات محتملة.
- المؤشرات تستبعد شمعة اليوم غير المكتملة.
- حفظ سعر هدف محلياً؛ المتابعة داخل التطبيق فقط دون إشعارات خلفية.
- لا تسجيل دخول، ولا مفاتيح API، ولا تداول، ولا دفع، ولا توصيات شراء مضمونة.

## استخراج IPA من الآيفون عبر GitHub
1. فك ضغط المشروع وأنشئ مستودع GitHub خاصاً جديداً.
2. ارفع **محتويات** مجلد BitcoinPulse إلى جذر المستودع، بما فيها `.github/workflows/build-ios.yml`. رفع ZIP وحده لا يشغّل البناء. رفع الملفات عبر git من كمبيوتر أسهل، أو استخدم محرر GitHub لإنشاء الملفات بمساراتها الصحيحة.
3. افتح تبويب Actions، واختر `Build unsigned iPhone IPA` ثم `Run workflow`.
4. بعد نجاح العملية افتح التنفيذ ثم نزّل Artifact باسم `BitcoinPulse-unsigned`.
5. فك ضغط الـArtifact لتحصل على `BitcoinPulse-unsigned.ipa`، ثم وقّعه بخدمتك وثبّته على جهازك.

إذا لم يظهر تشغيل Actions، تأكد من وجود ملف workflow في الفرع الافتراضي ومن تفعيل Actions. قد تتطلب دقائق macOS المتاحة في حساب GitHub ميزانية أو دفعاً؛ راجع حدود حسابك قبل التشغيل. لا تحتاج أسرار توقيع للبناء غير الموقع.

## البناء على ماك
ثبّت Xcode وأكمل تنزيل مكوناته، ثم ثبّت XcodeGen:

```bash
brew install xcodegen
bash scripts/build-ipa.sh
```

الناتج: `dist/BitcoinPulse-unsigned.ipa`.
يفتح `xcodegen generate` المشروع كملف `BitcoinPulse.xcodeproj` عند الرغبة بالتعديل عبر Xcode.

## حدود التحقق
تمت مراجعة ملفات المصدر وإعدادات المشروع وفحص صيغة YAML وShell في بيئة Linux. لم يُجرَ بناء Xcode أو اختبار جهاز/محاكي، لذلك قد يحتاج المشروع تعديلاً عند أول بناء. سير عمل GitHub مرفق لإجراء البناء الحقيقي؛ لم يتم تشغيله من هذه المحادثة.
لا توجد بيانات أسعار وهمية أو مخزنة مسبقاً. يلزم اتصال بالمصدر، وقد تمنع الشبكة أو قيود الخدمة الوصول أو تحد المعدل. عند فشل تحديث لاحق تُعرض آخر بيانات ناجحة مع رسالة واضحة. السعر خاص بسوق Coinbase، وقد يختلف عن منصتك.

## طريقة التحليل
الاتجاه الصاعد: آخر إغلاق مكتمل فوق SMA20 وSMA20 فوق SMA50. الهابط هو العكس؛ غير ذلك اتجاه مختلط. أعلى وأدنى 20 جلسة هما نطاق تاريخي وليسا ضمان دعم أو مقاومة. لا يتضمن المشروع نموذج توقع سنوي أو ذكاء اصطناعياً أو أخباراً.

## مصادر التطوير
- https://docs.cdp.coinbase.com/api-reference/exchange-api/rest-api/products/get-product-candles
- https://docs.cdp.coinbase.com/api-reference/exchange-api/rest-api/products/get-product-ticker
- https://docs.cdp.coinbase.com/api-reference/exchange-api/rest-api/products/get-product-stats
- https://github.com/yonaskolb/XcodeGen/blob/master/Docs/ProjectSpec.md

السعر المستهدف يحفظ على الجهاز عبر UserDefaults. ترسل طلبات الأسعار إلى Coinbase؛ لا يجمع التطبيق بيانات حساب أو محفظة.
