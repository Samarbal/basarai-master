# SRS: Basar AI SaaS Product Evolution

## 1. ملخص المشروع

Basar AI هو منتج SaaS يتيح للمستخدمين إنشاء محتوى بصري احترافي للعلامات التجارية، باستخدام AI، مع دعم قوالب جاهزة، تخصيص هوية العلامة التجارية، كتابة النصوص، وإدارة المزايا والاشتراكات. يركز المنتج على تجربة احترافية للمسوقين، منشئي المحتوى، وكالات العلامات التجارية، مع دعم طريقتين لاستخدام الخدمة:

1. المستخدم يضيف مفتاح API الخاص به (BYOK / Bring Your Own Key)
2. المستخدم اشتراك مدفوع عبر المنصة (Platform Billing)

يستند المشروع الحالي على بنية قوية موجودة في:
- Frontend: Next.js 14
- Backend: FastAPI
- Auth/DB/Storage: Supabase
- AI generation: OpenAI + Gemini

الهدف الحالي هو تحويل المنتج من نسخة داخلية موجهة للحالة التجريبية إلى منتج جاهز للسوق مع تجربة مستخدم احترافية، صفحة هبوط، وعملية توليد محتوى أكثر دقة من خلال قوالب ذكية ومحرك نصوص Hybrid.

---

## 2. رؤية المنتج

### 2.1 الهدف التجاري
إنشاء منصة إنتاج محتوى مرئي مدعومة بالذكاء الاصطناعي تساعد المستخدم على:
- إنشاء صور إعلانية ومنشورات ومقاطع غلاف
- استخدام هوية العلامة التجارية بشكل تلقائي
- توليد نصوص مناسبة للمنصة (Caption, Headline, CTA)
- استخدام قوالب جاهزة حسب نوع المحتوى
- اختيار أسلوب استخدام الخدمة: مفتاح شخصي أو اشتراك المنصة

### 2.2 الفئة المستهدفة
- المسوقون الرقميون
- صانعو المحتوى
- أصحاب العلامات التجارية الصغيرة
- الوكالات الإعلانية
- فرق التسويق داخل الشركات

### 2.3 القيم المضافة
- تقليل وقت الإنتاج من ساعات إلى دقائق
- جودة الصور مرتبطة بهوية العلامة التجارية
- نصوص احترافية مصممة لملف إنستغرام/فيسبوك/لينكدإن وغيرها
- إمكانية التوسع نحو اشتراكات ومزايا نقدية

---

## 3. نطاق المشروع

### 3.1 داخل النطاق
- صفحة Landing Page
- صفحة تسجيل دخول وتسجيل مستخدم
- إدارة الحساب والاشتراك
- إنشاء العلامات التجارية
- Brand Kit
- Template Library
- Generator page احترافي
- Hybrid engine للصور والنصوص
- Groq captions generation
- عرض السجل التاريخي
- نظام admin overview

### 3.2 خارج النطاق في المرحلة الحالية
- إدارة فريق متعدد المستخدمين بالكامل
- تخصيص صلاحيات معقدة للفرق
- دمج دفع متقدم متعدد البائعين
- دعم أكثر من نموذج AI في نفس الوقت بدون اختيار واضح
- سير عمل إنتاج عدّة الصور في نفس الطلب

---

## 4. المتطلبات الوظيفية

### FR-01: صفحة الهبوط Landing Page
- يجب أن تعرض القيمة الأساسية للمنتج في 5 ثواني
- يجب أن يحتوي على CTA واضح: "ابدأ الآن" / "استخدم مفتاحك" / "شاهد العرض"
- يجب أن يوضح كيفية عمل المنصة
- يجب أن يوضح ما إذا كان المستخدم يدفع أم يستخدم مفتاحه الخاص

### FR-02: تجربة تسجيل الدخول والتسجيل
- يجب أن يتوفر تسجيل دخول/تسجيل عبر Supabase Auth
- يجب أن توجد واجهة مميزة للـ SaaS وبعيدة عن الواجهة التجريبية
- يجب أن يتيح لك الاختيار بين:
  - Use my own API key
  - Subscribe to platform

### FR-03: إدارة العلامات التجارية
- يمكن للمستخدم إنشاء علامة تجارية
- يمكن تعديل اسم العلامة
- يمكن رفع/تحديث/حذف الشعار
- يمكن معرفة حالة Brand Kit

### FR-04: Brand Kit
- يجب أن يسمح للمستخدم بتعبئة بيانات الهوية التجارية:
  - Tagline
  - Tone
  - Audience
  - Colors
  - Avoid words
- يجب أن يقيم النظام حالة الإكمال
- يجب أن يحوّل هذه البيانات إلى summary يساعد AI في إنشاء الصور

### FR-05: Template Library
- يجب أن توجد قوالب جاهزة مثل:
  - إعلان منتج
  - خصم
  - اقتباس
  - غلاف
  - منشور إنستغرام
  - فيديو كفر
  - banner LinkedIn
- كل قالب يجب أن يحتوي على:
  - اسم القالب
  - نوع المحتوى
  - اسم المنصة
  - توصيات التصميم
  - نص موجه للـ AI

### FR-06: Generator page
- يجب أن يمنع مصطلح free prompt غير الموجه
- يجب أن يطلب من المستخدم تحديد:
  - القالب
  - الهدف
  - المنصة
  - النبرة
  - أدوات التصميم
  - استخدام الشعار أو عدمه
- يجب أن يكون هناك خيارات أكثر احترافية من مجرد مربع نص عادي

### FR-07: Hybrid Engine
- يجب أن يتيح توليد صورة + نصوص بطريقة متكاملة
- يجب أن يشتمل على:
  - AI image generation
  - AI text generation
  - logo integration
  - brand text overlay
  - caption/CTA generation

### FR-08: Groq caption generation
- يجب أن يدعم إنشاء نصوص/كابشن/CTA باستخدام Groq
- يمكن استخدام هذا في الحالات المرئية أو كملف نصي مكمل للنتيجة

### FR-09: History / Management
- يجب أن يكون هناك سجل كامل للنتائج
- يجب أن يسمح بعرض التفاصيل
- يمكن حذف الصورة/السجل
- يمكن تنزيل الصورة

### FR-10: Billing / Plan selection
- يجب أن يتيح للمستخدم اختيار الخطة المناسبة
- يجب أن يوفر ربطًا للدفع المستقبلي
- يجب أن تحدد الخطة حدود الاستخدام

### FR-11: Admin Dashboard
- يجب أن يتيح لإدارة النظام رؤية الإحصائيات الإجمالية
- يجب أن يوضح عدد العلامات التجارية والإنتاجات والتشغيلات النشطة

---

## 5. المتطلبات غير الوظيفية

### NFR-01: الأداء
- يجب أن يكون وقت الاستجابة الأولي مناسبًا للـ SaaS
- في حالات الاستهلاك العادي، يجب أن يكون وقت توليد الصورة مقبولاً

### NFR-02: الأمان
- يجب حماية مفاتيح الـ API داخل Vault
- يجب عدم تخزين مفتاح API بشكل واضح داخل DB
- يجب أن تكون جميع calls محمية عبر JWT

### NFR-03: القابلية للتوسع
- البنية الحالية ستدعم إضافة قوالب جديدة ومزودين جديدين
- يجب أن تكون الخدمة قابلة لإضافة محركات نصوص جديدة

### NFR-04: قابلية الصيانة
- يجب أن تنفصل منطق القالب عن منطق توليد الصور
- يجب الحفاظ على هيكل routes/services/models منظم

### NFR-05: تجربة المستخدم
- كل شاشة يجب أن تقدم موضوع SaaS احترافي وليس واجهة prototype
- كل مرحلة يجب أن تكون متسقة في اللون والطباعة والصور

---

## 6. المتطلبات التشغيلية

### 6.1 بيئة التطوير
- Next.js 14
- FastAPI
- Supabase
- Docker

### 6.2 المتطلبات البيئية
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
- `SUPABASE_URL`
- `SUPABASE_SECRET_KEY`
- `ADMIN_EMAILS`
- `NEXT_PUBLIC_API_URL`
- `NEXT_SERVER_API_URL`

### 6.3 متطلبات الأمان التشغيلية
- لا يمكن حفظ المفاتيح في repo
- لا يمكن تخزينها في `frontend` كـ static secret
- كل API key يوجد داخل Vault أو مفتاح المستخدم الخاص

---

## 7. حالات الاستخدام الأساسية

### Use Case 1: تسجيل الدخول وتحديد طريقة الاستخدام
- المستخدم يدخل التطبيق
- يختار: استخدام مفتاحه الخاص أو الاشتراك
- يتم توجيهه إلى dashboard المناسب

### Use Case 2: إنشاء علامة تجارية
- المستخدم ينشئ brand
- يضيف شعار
- يملأ Brand Kit

### Use Case 3: إنشاء صورة تنفيذاً لقالب جاهز
- المستخدم يختار قالب
- يحدد المدة أو الهدف
- يختار المنصة
- يختار النبرة
- يضغط Generate

### Use Case 4: إنشاء نصوص كابشن إضافية
- النظام يولّد caption
- يحدد CTA
- يضيف النص داخل الصورة حسب الخطة

### Use Case 5: إعادة الاستخدام والتاريخ
- المستخدم يراجع الصور السابقة
- يختار صورة واحدة
- يفتح التفاصيل
- يزيلها أو يحمّلها

---

## 8. Business Rules

1. Brand is owned by one user only.
2. Provider key belongs to a brand.
3. User can have one active key per provider.
4. A generation must belong to one brand.
5. Generation must have a valid output or failed status.
6. Storage objects are private unless public bucket policy allows access.
7. Admin pages are hidden for non-admin users.
8. Uploads must be validated by size and image type.

---

## 9. Functional decomposition into product modules

### Module A: Auth & Access
- login, signup
- redirect logic
- protected routes
- admin gate

### Module B: Brand Management
- create brand
- rename brand
- delete brand
- upload logo

### Module C: Brand Kit
- questions and answers
- completion logic
- summary generation

### Module D: API Key Management
- add key
- validate key
- activate/deactivate key
- remove key

### Module E: Templates & Generation
- template catalog
- skill selection
- prompt generation
- AI provider selection
- image post-processing

### Module F: Groq Content Layer
- caption generation
- hook generation
- CTA generation
- overlay text composition

### Module G: History & Output
- list generations
- display details
- delete generation
- download image

### Module H: Billing & Usage
- plan selection
- usage limits
- payment integration
- quota management

### Module I: Admin Analytics
- stats
- brand overview
- active keys

---

## 10. Acceptance Criteria Overview

### AC-01: Landing Page
Given a new visitor, when opening the root URL, then they see a landing page with clear value proposition and CTA.

### AC-02: Auth Choice
Given a user without session, when entering the app, then they are offered a clear choice between using personal API key or paid subscription.

### AC-03: Brand Creation
Given an authenticated user, when creating a brand, then a new brand record is created and visible in the dashboard.

### AC-04: Brand Kit Completion
Given a brand exists, when the user fills the kit, then status changes to complete and summary is generated.

### AC-05: Template Generation
Given a user selects a template, when generating content, then the backend builds a structured prompt based on the template and brand context.

### AC-06: Hybrid Output
Given a generation request, when the process succeeds, then the resulting response includes image, caption, and metadata.

### AC-07: Groq Integration
Given a generation request with text generation enabled, when Groq is available, then the caption text is generated and returned alongside the image.

### AC-08: Key Security
Given a provider key is uploaded, when it is saved, then it is stored via Vault and never stored plain text in DB.

### AC-09: Admin Access
Given a user with admin email, when navigating to admin routes, then they can view admin dashboards.

---

## 11. Risks and assumptions

### Risks
- OpenAI/Gemini API instability
- High cost of image generation in production
- User confusion between BYOK and subscription
- quality inconsistency in generated text

### Assumptions
- User will accept a hybrid model of personal API or subscription
- Supabase remains primary platform
- Groq can be integrated with clear text generation overhead
- Frontend design can be modernized without major structural rewrite

---

## 12. Definition of Done

المنتج يكتمل عندما:
- تم تنفيذ Landing Page
- تم redesign auth flow
- تم إضافة Choice between API key and plan
- تم redesign Generator page with templates and skills
- تم إضافة Groq caption engine
- تم إعادة توجيه توليد الصور إلى Hybrid pipeline
- تم اختبار كامل للسيناريوهات الأساسية
- تم التحقق من الأمان والـ RLS
- تم تجهيز release checklist

---

## 13. Recommended MVP Priority

### MVP v1
- Landing Page
- Auth redesign
- API-key option
- Brand Kit enhancement
- Template-first Generator
- Groq caption generation
- History and download

### MVP v2
- Subscription billing
- usage quotas
- plan gating
- admin analytics improvements
- team roles

---

## 14. الخلاصة التنفيذية

المنتج الحالي هو قاعدة تقنية قوية، لكنه يحتاج في المرحلة القادمة إلى:
- إعادة صياغة تجربة المستخدم كمنتج SaaS
- توضيح القيمة التجارية
- تحويل Generator من free prompt إلى نظام قائم على المهارات والقوالب
- دمج Groq للنصوص
- توسيع المصادقة والاشتراك
- ترسيخ أمان المفاتيح والنسخة الإنتاجية

هذا الملف يُعد نقطة الانطلاق الرسمية لتحديد أولويات التطوير والفريق.
