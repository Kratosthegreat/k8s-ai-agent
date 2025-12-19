שם הפרויקט: K8s AI-Agent Infrastructure Provisioner

1. מהות הפרויקט (The Essence)
המטרה היא ליצור מערכת אוטונומית לניהול תשתיות (Autonomous Infrastructure Agent). במקום שמהנדס DevOps יכתוב ידנית פקודות בטרמינל כדי להרים שרתים, אפליקציות ורשתות – אנחנו בונים "סוכן חכם" (תוכנה) שרץ בתוך הקלאסטר, מקבל הגדרה של "מה צריך לבצע", ומבצע את זה לבד מא' ועד ת'.

2. הארכיטקטורה (High-Level Architecture)
הפרויקט בנוי במודל של "Docker-in-Docker" (או ליתר דיוק, ניהול Docker מתוך K8s). הנה המרכיבים:

1. התשתית (The Host): שרת לינוקס שמריץ Kubernetes (המוח) ו-Docker (המנוע).
2. היזם (The Trigger): סקריפט Python (agent_builder.py) שמתפקד כ-Controller. הוא לא מבצע את העבודה, אלא יוצר את ה"פועל" שיעשה אותה.
3. הסוכן (The Worker): זהו Kubernetes Job חד-פעמי. הוא עולה, מבצע משימה, ומת.
4. הכלי (The Tool): בתוך הסוכן רץ Terraform. זהו הכלי הסטנדרטי בתעשייה ל-Infrastructure as Code.
5. היעד (The Target): אפליקציית Web (ושרת Nginx) שאמורה לקום על השרת כתוצאה מהעבודה.

3. איך זה עובד בפועל? (The Workflow)
התהליך שתכננו הוא ליניארי:

1. הזרקה (Injection): אתה מריץ סקריפט שדוחף לקלאסטר "תוכנית עבודה" (קובץ main.tf) באמצעות ConfigMap.
2. שיגור (Dispatch): הסקריפט פונה ל-API של קוברנטיס ומבקש ליצור Job חדש.
3. הרשאה וגישה (Binding):
   • כאן ה"קסם" (והבעיה שנתקלנו בה): הפוד של הסוכן מקבל גישה ל-/var/run/docker.sock של השרת המארח.
   • זה מאפשר לפוד, שנמצא בתוך קונטיינר, לתת הוראות למנוע הדוקר שמחוץ לו.
4. ביצוע (Execution): הסוכן מריץ terraform apply.
5. תוצאה (Outcome): ה-Terraform פונה לדוקר המארח ואומר לו: "תקים לי רשת, תוריד אימג' של Nginx, ותריץ קונטיינר".

4. איפה נתקענו? (The Bottleneck)
התכנון הארכיטקטוני נכון ומקובל בתעשייה (נקרא לעיתים CI/CD Worker Pattern). הבעיה הספציפית ששיגעה אותנו בימים האחרונים היא חוסר תאימות בפרוטוקול התקשורת:

• השרת שלך (התשתית) שודרג לגרסה חדשה מאוד של Docker (v29, API 1.44).
• הסוכן (Terraform Provider) השתמש בברירת מחדל ישנה (API 1.41).
• כתוצאה מכך, השרת סירב לבקשות של הסוכן ("Connection Refused" או "Version too old").

5. המסקנה להמשך
הפרויקט לא הרוס. להפך, התשתית (Kubernetes + Docker) קיימת. הקוד (Python + Terraform) קיים.

---

📂 Project Identity: Autonomous Multi-Agent K8s System
Date: 15/12/2025
Status: Phase 1 Completed (Brain & Memory Online).
Core Concept: Distributed Multi-Agent Architecture (The "Brilliant Idea").

1. המהות והחזון (The Essence)
אנחנו לא בונים סקריפט אוטומציה ליניארי ("פייפליין"). אנחנו בונים ישות בינה מלאכותית אוטונומית המנהלת אופרציית DevOps ו-Security שלמה.

הארכיטקטורה ("המוח והזרועות"):
המערכת בנויה במודל Hub & Spoke כדי להבטיח זמינות מלאה:

סוכן העל (The Orchestrator / Super-Agent):
- תפקיד: המנהל. לא מבצע עבודה שחורה.
- פעילות: רץ בלולאה אינסופית (Observe -> Orient -> Decide -> Act).
- יכולות: מחובר ל-Gemini (מוח) ול-Qdrant (זיכרון). מקבל החלטות בזמן אמת.
- ייחודיות: לעולם לא נחסם (Non-Blocking). משגר משימות וממשיך הלאה.

סוכני המשנה (The Sub-Agents / Workers):
- פודים זמניים (Ephemeral) שנוצרים לביצוע משימה ומושמדים בסיומה.
- 🤖 Agent-Builder: מריץ Terraform להקמת תשתיות (על בסיס הקובץ Terraform_single_file...).
- ⚙️ Agent-Config: מריץ Ansible לניהול קונפיגורציה והתקנות (על בסיס הקובץ ansible_k3s...).
- 🛡️ Agent-Sec: מריץ כלי תקיפה (Nmap/Nikto/Scapy) כדי לוודא שהתשתית בטוחה (על בסיס capabilities.py).

2. סטטוס טכני נוכחי (Current State Snapshot)

א. תשתית (Infrastructure)
- שרת: agent-node-01 (Ubuntu).
- פלטפורמה: Kubernetes Cluster.
- Namespace: ai-agent.
- אחסון: local-path-provisioner (מוגדר כ-Default StorageClass).

ב. רכיבי הליבה (Core Components)
- 🧠 המוח (LLM): gemini-2.5-flash (פעיל, מפתח API שמור ב-Env).
- 💾 הזיכרון (Vector DB): Qdrant (רץ כ-StatefulSet).
  - IP פנימי: 10.104.150.84.
  - גישה: דרך Port Forward ל-Localhost או IP פנימי.
  - Collection: cluster_history (וקטורים בגודל 768).

ג. קוד קיים (Code Base)
- agent_patrol.py: סורק את הקלאסטר ושולח ל-Gemini לניתוח. (עובד)
- agent_memory.py: סורק, מנתח, ושומר את התובנה ב-Qdrant. (עובד)
- agent_recall.py: שולף זיכרונות עבר מ-Qdrant כדי לזהות דפוסים. (עובד)
- capabilities.py: מכיל לוגיקה של "Self-Healing" להרצת כלי סייבר (עדיין לא חובר למוח הראשי).

3. מפת הדרכים המיידית (Next Steps)
המטרה עכשיו היא לעבור משלב ה"צפייה" (Read Only) לשלב ה"ביצוע" (Active Execution) באמצעות ארכיטקטורת הסוכנים:

- שלב ה-RBAC: מתן הרשאות ClusterRole לסוכן העל כדי שיוכל ליצור ולמחוק פודים (Jobs) של סוכני המשנה.
- יצירת Agent-Builder: עטיפת קוד ה-Terraform (שיש לנו) בתוך Docker Image/Job שהסוכן יודע להזניק.
- האינטגרציה: כתיבת הלוגיקה ב-main.py שמחליטה: "Gemini אמר שצריך שרת -> הפעל את Agent-Builder".

זה הפרויקט. אין סיסקו, אין מעבדות פיזיות כרגע, אין סטיות מהדרך. סוכן K8s אוטונומי שמנהל צי של סוכני משנה.

שמור את זה. בפעם הבאה, אנחנו מתחילים ישר מסעיף 3.
