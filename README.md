# University-Database-using-SQL
قاعدة بيانات إدارة جامعة/كلية متكاملة. بتمسك كل شيء: مباني، كليات، أقسام، قاعات، مواد، محاضرات، طلاب، أساتذة، موظفين أكاديميين، مكتبة وكتب، تسجيل المواد، الحضور، الاستعارة، والرسوم.


الجداول الأساسية 

كيانات رئيسية (Entities):

BUILDING (المباني)

COLLEGES (الكليات)

DEPARTMENT (الأقسام)

CLASSROOMS (القاعات)

COURSE (المقررات)

LECTURES (المحاضرات)

LIBRARIES (الكتب/المكتبات — واضح فيها Book_ID)

STUDENT (الطلاب)

PROFESSOR (الأساتذة)

ACADEMIC_STAFF (الموظفون الأكاديميون)

DEANS (العمادة)

TUITION_FEES (الرسوم الدراسية)

تفاصيل/ملاحق للكيانات:

PROFESSOR_PHONE (أرقام الأساتذة)

PROFESSOR_QUALIFICATIONS (مؤهلات الأساتذة)

staff_address (عناوين الموظفين)

جداول علاقات (Junction/Link Tables) تُمثل Many-to-Many:

enrolled_students (تسجيل الطلاب في المقررات)

student_attending (حضور الطلاب للمحاضرات)

student_borrowing (استعارة الطالب للكتب)

offering_courses (طرح المقررات عبر الأقسام/الكليات)

teaching_courses (المقررات التي يدرّسها الأستاذ)

professor_belonging (انتماء الأستاذ لقسم/كلية)

student_payments (دفعات رسوم الطلاب)

having_departmen / having_deans (ربط إداري بين الكليات/الأقسام والعمادة… التسمية فيها خطأ إملائي بسيط غالبًا تقصد “having_department”)

العلاقات (Relationships) — أهم النقاط

في الـDOCX موجود قيود FOREIGN KEY … REFERENCES … كثير، ومعها ON DELETE CASCADE في مواقع حساسة:
.

ربط قياسي بين:

COLLEGES ↔ DEPARTMENT (كلية فيها أقسام)

DEPARTMENT ↔ COURSE (القسم يقدّم مقررات)

CLASSROOMS ↔ LECTURES (المحاضرات تُعقد في قاعات)

STUDENT ↔ COURSE عبر enrolled_students

STUDENT ↔ LECTURES عبر student_attending

STUDENT ↔ LIBRARIES عبر student_borrowing

PROFESSOR ↔ COURSE عبر teaching_courses

PROFESSOR ↔ DEPARTMENT/COLLEGE عبر professor_belonging

STUDENT ↔ TUITION_FEES عبر student_payments

فيه جداول تفاصيل (1-to-Many) مثل PROFESSOR_PHONE و PROFESSOR_QUALIFICATIONS بدل ما تنحشر جوّا جدول الأستاذ — هذا تصميم Normalized ومرتب.

قواعد عمل (Business Rules) واضحة من الـDDL

ON DELETE CASCADE مستخدم بحكمة: حذف كيان أبوي يمسح الأبناء (يوفّر عليك تنظيف يدوي).

فصل بيانات الاتصال/المؤهلات/العناوين في جداول مستقلة → يقلل التكرار (Normalization).

وجود جداول ربط كثيرة يعني الدعم الكامل لعلاقات Many-to-Many (التسجيل، التدريس، الاستعارة، الحضور…).
