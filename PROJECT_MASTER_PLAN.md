📂 Project Identity: Autonomous Multi-Agent K8s System
Date: 15/12/2025 Status: Phase 1 Completed (Brain & Memory Online). Core Concept: Distributed Multi-Agent Architecture (The "Brilliant Idea").

1. המהות והחזון (The Essence)
אנחנו לא בונים סקריפט אוטומציה ליניארי ("פייפליין"). אנחנו בונים ישות בינה מלאכותית אוטונומית המנהלת אופרציית DevOps ו-Security שלמה.

הארכיטקטורה ("המוח והזרועות"):
המערכת בנויה במודל Hub & Spoke כדי להבטיח זמינות מלאה:

סוכן העל (The Orchestrator / Super-Agent):

תפקיד: המנהל. לא מבצע עבודה שחורה.

פעילות: רץ בלולאה אינסופית (Observe -> Orient -> Decide -> Act).

יכולות: מחובר ל-Gemini (מוח) ול-Qdrant (זיכרון). מקבל החלטות בזמן אמת.

ייחודיות: לעולם לא נחסם (Non-Blocking). משגר משימות וממשיך הלאה.

סוכני המשנה (The Sub-Agents / Workers):

פודים זמניים (Ephemeral) שנוצרים לביצוע משימה ומושמדים בסיומה.

🤖 Agent-Builder: מריץ Terraform להקמת תשתיות (על בסיס הקובץ Terraform_single_file...).

⚙️ Agent-Config: מריץ Ansible לניהול קונפיגורציה והתקנות (על בסיס הקובץ ansible_k3s...).

🛡️ Agent-Sec: מריץ כלי תקיפה (Nmap/Nikto/Scapy) כדי לוודא שהתשתית בטוחה (על בסיס capabilities.py).

2. סטטוס טכני נוכחי (Current State Snapshot)
א. תשתית (Infrastructure)
שרת: agent-node-01 (Ubuntu).

פלטפורמה: Kubernetes Cluster.

Namespace: ai-agent.

אחסון: local-path-provisioner (מוגדר כ-Default StorageClass).

ב. רכיבי הליבה (Core Components)
🧠 המוח (LLM): gemini-2.5-flash (פעיל, מפתח API שמור ב-Env).

💾 הזיכרון (Vector DB): Qdrant (רץ כ-StatefulSet).

IP פנימי: 10.104.150.84.

גישה: דרך Port Forward ל-Localhost או IP פנימי.

Collection: cluster_history (וקטורים בגודל 768).

ג. קוד קיים (Code Base)
agent_patrol.py: סורק את הקלאסטר ושולח ל-Gemini לניתוח. (עובד)

agent_memory.py: סורק, מנתח, ושומר את התובנה ב-Qdrant. (עובד)

agent_recall.py: שולף זיכרונות עבר מ-Qdrant כדי לזהות דפוסים. (עובד)

capabilities.py: מכיל לוגיקה של "Self-Healing" להרצת כלי סייבר (עדיין לא חובר למוח הראשי).

3. מפת הדרכים המיידית (Next Steps)
המטרה עכשיו היא לעבור משלב ה"צפייה" (Read Only) לשלב ה"ביצוע" (Active Execution) באמצעות ארכיטקטורת הסוכנים:

שלב ה-RBAC: מתן הרשאות ClusterRole לסוכן העל כדי שיוכל ליצור ולמחוק פודים (Jobs) של סוכני המשנה.

יצירת Agent-Builder: עטיפת קוד ה-Terraform (שיש לנו) בתוך Docker Image/Job שהסוכן יודע להזניק.

האינטגרציה: כתיבת הלוגיקה ב-main.py שמחליטה: "Gemini אמר שצריך שרת -> הפעל את Agent-Builder".

זה הפרויקט. אין סיסקו, אין מעבדות פיזיות כרגע, אין סטיות מהדרך. סוכן K8s אוטונומי שמנהל צי של סוכני משנה.

שמור את זה. בפעם הבאה, אנחנו מתחילים ישר מסעיף 3.