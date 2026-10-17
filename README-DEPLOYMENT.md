# حزمة بناء Windows — SDFBranchSystem (v6)

## محتويات الحزمة
- `.github/workflows/build-windows.yml`: Workflow يبني نسخة Windows ويشغّل `install-smoke`.
- `SDFBranchSystem-source.zip`: المصدر، الاختبارات، أدوات البناء وفحص artifacts.
- `AUDIT-STATUS-v6.md`: نتائج التحقق وحدوده.
- `CHANGELOG-v6.md` و`DIFF-v5-v6.md`: التعديلات.
- `WINDOWS-ACCEPTANCE-CHECKLIST-v6.md`: بوابات قبول الإصدار.

## التشغيل
1. فك ضغط الحزمة.
2. ارفع **محتوياتها** إلى جذر مستودع GitHub مع الحفاظ على المسارات؛ يجب أن يكون `.github/workflows/build-windows.yml` في جذر المستودع، وأن يكون `SDFBranchSystem-source.zip` بجانبه.
3. من تبويب **Actions** اختر `بناء نسخة Windows` ثم `Run workflow` على فرع مناسب.
4. لا تنشئ tag إصدارًا عامًا قبل مراجعة نتيجة التشغيل.
5. لا تعتمد الإصدار إلا بعد نجاح الوظيفتين `build` و`install-smoke` ومراجعة artifacts والسجلات.
6. راجع `SDFBranchSystem-CI-Diagnostics-<run_id>` و`SDFBranchSystem-Install-Smoke-Diagnostics-<run_id>` عند أي فشل.
7. نفّذ قائمة القبول اليدوية v6 على Windows نظيف، خصوصًا شاشة تغيير كلمة المرور وPDF/Excel والنسخ الاحتياطي والاستعادة.

## ما أضيف في v6
- توحيد fixture `QApplication` وإضافة cleanup دفاعي لعناصر Qt بين الاختبارات.
- إضافة فحص محتوى artifacts بحثًا عن أسرار شائعة، بالإضافة إلى حواجز الامتدادات.
- إضافة تدقيق اعتماديات `pip-audit` وتوليد SBOM بصيغة CycloneDX JSON داخل CI.
- إيقاف دفاعي لأي process متبقٍ داخل مجلد تثبيت الاختبار قبل تشغيل uninstaller.

## حدود مهمة
**هذه الحزمة ليست ملف EXE Windows جاهزًا للتوزيع.** لا يمكن إنشاء أو اعتماد Windows executable/installer من بيئة Linux الحالية دون تشغيل Windows CI. كما أن تغييرات Qt lifecycle لم تثبت بعد أنها تعالج exit 139؛ يجب تشغيل الاختبارات الكاملة مع كل الاعتماديات ومراجعة رمز الخروج الفعلي.

لا تنشئ ملف constraints نهائيًا من بيئة Linux عشوائية. أنشئه من مجموعة نسخ اجتازت Windows CI ثم أعد البناء بها. التوقيع الرقمي يحتاج شهادة ومفتاحًا تحت سيطرة مالك الإصدار، ولذلك لم يُنشأ هنا.
