# 🗺️ Live Tracking System - Architecture & Learning Roadmap

> **وثيقة المشروع التوجيهية (Project Guide & Progress Tracker)**  
> هذا الملف هو مرجعك الدائم لمتابعة تقدم المشروع، مراجعة المعمارية، وتفاصيل كل خطوة بدون الحاجة للرجوع للشات. يتم تحديثه مع إنجاز كل خطوة.

---

## 🏗️ 1. التقنيات والمعمارية (Tech Stack & Architecture)
- **Framework:** Flutter (Clean/Feature-first Architecture)
- **Maps:** `flutter_map` + OpenStreetMap (OSM Tile Server - بدون API Keys)
- **Backend & Realtime:** Supabase (PostgreSQL + Realtime Channels)
- **Location Engine:** `geolocator`
- **State Management:** (سنحدده لاحقاً عند الوصول للمرحلة - مثل Bloc / Riverpod)

---

## 📜 2. قواعد العمل الإرشادية (Mentor Rules)
1. **لا للأكواد الجاهزة للنسخ واللصق (Zero Spoon-Feeding):** المطور يكتب كل سطر بنفسه لضمان الفهم والتمكن.
2. **العمل خطوة بخطوة (Step-by-Step Milestones):** إنجاز ومراجعة خطوة واحدة فقط في المرة الواحدة.
3. **مراجعة الكود الصارمة (Strict Code Review):** التدقيق في Memory Leaks، Concurrency، إغلاق الـ Streams، والـ Clean Code.

---

## 🗺️ 3. خارطة المراحل (Milestones Roadmap)

### 📌 Milestone 1: إعداد قاعدة البيانات والـ Realtime في Supabase
- [ ] **الخطوة 1.1:** تصميم جدول الإحداثيات (`locations`) في PostgreSQL. *(الحالية)*
- [ ] **الخطوة 1.2:** تفعيل الـ Row Level Security (RLS) وسياسات الأمان (Policies).
- [ ] **الخطوة 1.3:** تفعيل Realtime Replication على الجدول وتجهيز الـ Publication.

### 📌 Milestone 2: محرك التتبع المحلي (Location Engine)
- [ ] **الخطوة 2.1:** إعداد الصلاحيات في Android/iOS (`AndroidManifest.xml` و `Info.plist`).
- [ ] **الخطوة 2.2:** فحص ومعالجة حالات إذن الموقع وخدمة الـ GPS مع الـ Edge cases.
- [ ] **الخطوة 2.3:** بناء الـ Location Stream بالمعايير المثالية للبطارية والدقة (`LocationSettings`).

### 📌 Milestone 3: ربط التطبيق بـ Supabase وبث المواقع
- [ ] **الخطوة 3.1:** تهيئة مكتبة `supabase_flutter` في التطبيق.
- [ ] **الخطوة 3.2:** إرسال الموقع لحظياً (Sender - Upsert / Broadcast).
- [ ] **الخطوة 3.3:** الاستماع للمواقع لحظياً (Receiver - Realtime Stream).

### 📌 Milestone 4: واجهة الخريطة الحية (Live Map & Markers)
- [ ] **الخطوة 4.1:** إعداد `flutter_map` مع OpenStreetMap وضبط الـ TileLayer.
- [ ] **الخطوة 4.2:** رسم وإدارة الـ Markers الحية لتمثيل الأجهزة المتتبعة.
- [ ] **الخطوة 4.3:** تنعيم حركة الماركر وضبط زاوية التوجيه (Heading/Bearing).

### 📌 Milestone 5: الصقل، المعمارية والـ Edge Cases
- [ ] **الخطوة 5.1:** التعامل مع انقطاع الإنترنت وإعادة الاتصال التلقائي.
- [ ] **الخطوة 5.2:** غلق الـ Streams والـ Subscriptions لمنع الـ Memory Leaks عند مغادرة الشاشات.

---

## 🎯 المهمة الحالية (Active Task): الخطوة 1.1

### الهدف (The "Why")
تصميم جدول خفيف وسريع يخزن أحدث موقع لكل جهاز/مستخدم لتطبيق الـ Live Tracking، بحيث يتحمل التحديثات المتكررة ويكون مهيأ للبث اللحظي.

### الحقول المطلوبة للتفكير فيها:
1. `id` (Primary Key).
2. `user_id` أو `device_id` (فريد لكل مستخدم/جهاز لتسهيل الـ Upsert).
3. `latitude` (خط العرض - دقة عالية مثل `double precision` أو `numeric`).
4. `longitude` (خط الطول - دقة عالية).
5. `heading` (زاوية التوجيه / الدوران - اختياري للماركر).
6. `speed` (السرعة - اختياري).
7. `updated_at` (تاريخ ووقت آخر تحديث - تلقائي بـ `now()`).

### محاذير (Gotchas):
- تجنب أنواع النصوص أو الأعداد الصحيحة للإحداثيات؛ استخدم `double precision`.
- فكر في عمل `UNIQUE constraint` على `user_id` / `device_id` حتى تتمكن لاحقاً من عمل `UPSERT` بدلاً من تراكم ملايين السجلات غير المفيدة للموقع المباشر.

---

*تاريخ الإنشاء: 2026-10-07*
