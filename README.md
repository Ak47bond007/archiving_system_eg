# نظام المراسلات - التمثيل التجاري المصري (Archiving System EG)

ملخص
-----
نظام لإدارة المراسلات الواردة والصادرة مع:
- إدارة جهات الاتصال والإدارات.
- رفع المرفقات (pdf, jpg, docx, xlsx).
- نافذة عرض وطباعة المكاتبات والمرفقات.
- حفظ توقيت UTC في قاعدة البيانات وعرض الوقت بالعربي (ميلادي/هجري) مع تحديث كل ثانية.
- إشعارات فورية عند تسجيل مكاتبة جديدة.
- نسخ احتياطي وتصدير/استيراد Excel و JSON.
- شات بوت بسيط للبحث (البوسطجي).

التقنيات المستخدمة
-------------------
- Python 3.10+
- Flask, Flask-SocketIO, Flask-Login
- SQLAlchemy (MySQL أو SQLite)
- pandas (تصدير/استيراد Excel/JSON)
- hijri-converter (تحويل التاريخ للهجري)
- واجهة HTML/CSS + JavaScript للـ widget الوقت وnotifications

إعداد سريع (محلي / تجريبي)
--------------------------
1. أنشئ بيئة افتراضية:
   python -m venv venv
   source venv/bin/activate   # أو venv\Scripts\activate على ويندوز

2. ثبّت المتطلبات:
   pip install -r requirements.txt

3. أنشئ قاعدة بيانات MySQL (موصى به للشبكة) أو استعمل SQLite. راجع schema_mysql.sql أو استخدم init_db.py.

4. إعداد المتغيرات (انسخ .env.example إلى .env وعدّل):
   export FLASK_APP=app.py
   export FLASK_ENV=development
   export DATABASE_URL=mysql+pymysql://user:pass@DB_HOST/archiving_system_eg

5. شغّل السكربت لتهيئة القاعدة:
   python init_db.py

6. شغّل الخادم:
   flask run --host=0.0.0.0 --port=5000
   أو
   python app.py

ربط أجهزة متعددة على الشبكة الداخلية
-----------------------------------
- أنشئ MySQL مركزي على خادم في الشبكة (مثال: 192.168.1.10) وامنح المستخدم صلاحيات اتصال من الشبكة.
- في كل جهاز غيّر DATABASE_URL للإشارة لقاعدة الخادم المركزي.
- شغّل تطبيق Flask كخادم مركزي على جهاز واحد (أو بصيغة WSGI مع nginx + gunicorn)، وباقي الأجهزة تفتح الواجهة عبر IP الخادم.
- SocketIO يعمل لحظياً عبر نفس الخادم (WebSocket).

ملفات هامة
----------
- app.py         : الخادم وتوجيه الطلبات
- models.py      : نماذج SQLAlchemy
- init_db.py     : إنشاء القاعدة وبيانات تجريبية
- backup.py      : تصدير/استيراد ونسخ احتياطي أسبوعي
- templates/     : واجهات HTML
- static/        : ملفات JS و CSS
- schema_mysql.sql: سكربت إنشاء الجداول لـ MySQL

حقوق الملكية
------------
© 2024 التمثيل التجاري المصري - الملحق الإداري/ عبدالرحمن قاعود

---