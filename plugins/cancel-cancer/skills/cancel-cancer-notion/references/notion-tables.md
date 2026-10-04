# הטבלאות של נושן: העמוד הראשי, שש הטבלאות והשדות

הסקיל `cancel-cancer-notion` בונה מכאן את נושן של הערכה. הקובץ משמש בשלוש דרכים:

- **בנייה דרך החיבור:** מעתיקים את פקודת `CREATE TABLE` של כל טבלה כמו שהיא. לפני כן מחליפים את `PEOPLE_ID` במזהה של טבלת אנשים (ראו `notion-format.md`).
- **בנייה ביד:** כשנושן לא מחובר, או כשהחיבור לא הצליח ליצור טבלה, מציגים למשתמש את טבלת השדות של כל טבלה, ואת הסעיף "בנייה ביד" שבסוף.
- **שמות באנגלית:** רק אם המשתמש ביקש. אז לוקחים את השמות מהעמודה "באנגלית", וגם את האפשרויות שבה, באותו סדר.

**הסדר קבוע:** אנשים ומקורות קודם, כי תרופות, תורים ואירועים ומשימות מקושרות לאנשים. ממתין למחשב אחרון.

**שורת עובדה:** בכל טבלה של עובדות יש `מקור` (מספר המסמך בתיקייה, או "דיווח" ומי ומתי) ו`תאריך המקור`. בטבלת מקורות המספר הוא שם השורה, ויש `תאריך המסמך`. **אין שדה של המלצה באף טבלה.**

## העמוד הראשי

- **השם:** `Cancel Cancer · ` ואחריו למי התיק מ-`kit.md` (למשל `Cancel Cancer · אמא`). בלי `kit.md` ובלי שם מהשיחה: `Cancel Cancer`. כשבונים בתוך עמוד ריק שהמשתמש בחר, העמוד שלו הוא העמוד הראשי, ולא משנים את השם שלו.
- **שתי השורות בראש העמוד,** כמו שהן (Notion Markdown):

```markdown
**עובדה קצרה כאן, והמקור המלא בתיקייה.** כל שורה בטבלאות שבעמוד הזה מפנה למסמך בתיקייה במחשב, לפי המספר שלו.
לדוגמה: "תור לאונקולוג, יום שלישי בעשר · מקור: 0007". צריך יותר מהשורה? פותחים את 0007 בתיקייה.
```

- **באנגלית,** רק אם המשתמש ביקש שמות באנגלית:

```markdown
**A short fact here, the full source in the folder.** Every row in the tables on this page points to a document in the computer folder, by its number.
For example: "Oncologist appointment, Tuesday at ten · source: 0007". Need more than the row? Open 0007 in the folder.
```

## 1. אנשים (`people`)

אנשי מקצוע, מוסדות ומי מהמשפחה שעוזר. על כל אדם רק מה שצריך לתפקיד שלו. **בלי כתובת מגורים ובלי מספר זהות.** טלפון ומייל רק לאנשי מקצוע ולמוסדות.

```sql
CREATE TABLE (
  "שם" TITLE,
  "סוג" SELECT('איש מקצוע':blue, 'מוסד':gray, 'משפחה':green, 'אחר':default),
  "תפקיד" RICH_TEXT COMMENT 'למשל אונקולוגית, אחות מתאמת, עובדת סוציאלית, קניות',
  "מוסד / מחלקה" RICH_TEXT,
  "טלפון" PHONE_NUMBER COMMENT 'רק לאנשי מקצוע ולמוסדות',
  "מייל" EMAIL COMMENT 'רק לאנשי מקצוע ולמוסדות',
  "הערות" RICH_TEXT,
  "מקור" RICH_TEXT COMMENT 'מספר המסמך בתיקייה, למשל 0007. או: דיווח, מי ומתי',
  "תאריך המקור" DATE,
  "עודכן" LAST_EDITED_TIME
)
```

| שדה | סוג ב-Notion | באנגלית | מה נכנס |
|---|---|---|---|
| שם | Name (העמודה הראשונה) | Name | השם, כמו שכתוב במקור |
| סוג | Select | Type: Professional, Institution, Family, Other | איש מקצוע, מוסד, משפחה, אחר |
| תפקיד | Text | Role | מה האדם עושה בתיק |
| מוסד / מחלקה | Text | Institution / department | איפה הוא עובד |
| טלפון | Phone | Phone | רק לאנשי מקצוע ולמוסדות |
| מייל | Email | Email | רק לאנשי מקצוע ולמוסדות |
| הערות | Text | Notes | מה שחשוב לדעת על התפקיד |
| מקור | Text | Source | מספר המסמך, או דיווח |
| תאריך המקור | Date | Source date | התאריך של המסמך או של הדיווח |
| עודכן | Last edited time | Updated | Notion ממלא לבד |

## 2. מקורות (`sources`)

שורה לכל מסמך בתיקייה. זה הקטלוג: העובדות בשאר הטבלאות מפנות לכאן לפי המספר.

```sql
CREATE TABLE (
  "מספר" TITLE COMMENT 'ארבע ספרות, כמו שם הקובץ בתיקייה, למשל 0007. קובץ בלי מספר: שם הקובץ',
  "מה זה" RICH_TEXT COMMENT 'כמה מילים, למשל סיכום ביקור',
  "סוג" SELECT('סיכום ביקור':blue, 'תוצאות בדיקה':purple, 'פענוח':purple, 'מרשם':red, 'מכתב שחרור':orange, 'מכתב ממוסד':gray, 'טופס':gray, 'מייל':gray, 'תמלול':pink, 'מחקר':brown, 'אחר':default),
  "תאריך המסמך" DATE,
  "מאת" RICH_TEXT COMMENT 'מי כתב, או איזה מוסד',
  "בתיקייה" RICH_TEXT COMMENT 'הנתיב של הקובץ בתיקייה',
  "סטטוס" SELECT('בתוקף':green, 'הוחלף':gray, 'כפול':yellow),
  "עודכן" LAST_EDITED_TIME
)
```

| שדה | סוג ב-Notion | באנגלית | מה נכנס |
|---|---|---|---|
| מספר | Name (העמודה הראשונה) | Number | ארבע ספרות, כמו שם הקובץ |
| מה זה | Text | What it is | כמה מילים |
| סוג | Select | Type: Visit summary, Test results, Imaging report, Prescription, Discharge letter, Letter from an institution, Form, Email, Transcript, Research, Other | סיכום ביקור, תוצאות בדיקה, פענוח, מרשם, מכתב שחרור, מכתב ממוסד, טופס, מייל, תמלול, מחקר, אחר |
| תאריך המסמך | Date | Document date | התאריך שכתוב על המסמך |
| מאת | Text | From | מי כתב, או איזה מוסד |
| בתיקייה | Text | In the folder | הנתיב, בתוך גרשיים הפוכים |
| סטטוס | Select | Status: Current, Replaced, Duplicate | בתוקף, הוחלף, כפול |
| עודכן | Last edited time | Updated | Notion ממלא לבד |

## 3. תרופות (`medications`)

רק ממסמך. השם, המינון והאופן כמו שכתוב במקור, בלי לפרש. דיווח בעל פה על תרופה נרשם כשאלה לצוות בטבלת משימות.

```sql
CREATE TABLE (
  "תרופה" TITLE COMMENT 'השם כמו שכתוב במקור',
  "מינון" RICH_TEXT COMMENT 'כמו שכתוב במקור',
  "איך ומתי" RICH_TEXT COMMENT 'כמו שכתוב במקור',
  "מי רשם" RELATION('PEOPLE_ID', DUAL 'תרופות שרשם'),
  "התחלה" DATE,
  "הפסקה" DATE,
  "סטטוס" SELECT('פעילה':green, 'הופסקה':gray, 'מתוכננת':blue, 'לא ברור':yellow),
  "הערות" RICH_TEXT,
  "מקור" RICH_TEXT COMMENT 'מספר המסמך בתיקייה, למשל 0007',
  "תאריך המקור" DATE,
  "עודכן" LAST_EDITED_TIME
)
```

| שדה | סוג ב-Notion | באנגלית | מה נכנס |
|---|---|---|---|
| תרופה | Name (העמודה הראשונה) | Medication | השם כמו שכתוב במקור |
| מינון | Text | Dose | כמו שכתוב במקור |
| איך ומתי | Text | How and when | כמו שכתוב במקור |
| מי רשם | Relation ← אנשים | Prescribed by | קישור לשורה באנשים |
| התחלה | Date | Start | |
| הפסקה | Date | Stop | |
| סטטוס | Select | Status: Active, Stopped, Planned, Unclear | פעילה, הופסקה, מתוכננת, לא ברור |
| הערות | Text | Notes | |
| מקור | Text | Source | מספר המסמך |
| תאריך המקור | Date | Source date | התאריך של המסמך |
| עודכן | Last edited time | Updated | Notion ממלא לבד |

## 4. תורים ואירועים (`events`)

תורים, בדיקות, טיפולים, אשפוזים, ועדות ומועדי הגשה. תור שזז מקבל שורה חדשה. השורה הקודמת לא נמחקת: הסטטוס שלה משתנה, או שהיא מסומנת "לבדיקה".

```sql
CREATE TABLE (
  "אירוע" TITLE,
  "מועד" DATE,
  "סוג" SELECT('תור':blue, 'בדיקה':purple, 'טיפול':red, 'אשפוז':orange, 'ועדה':brown, 'מועד הגשה':yellow, 'שיחה':gray, 'אחר':default),
  "סטטוס" SELECT('מתוכנן':blue, 'התקיים':green, 'בוטל':gray, 'לבדיקה':yellow),
  "עם מי" RELATION('PEOPLE_ID', DUAL 'תורים ואירועים'),
  "איפה" RICH_TEXT COMMENT 'מוסד, מחלקה, חדר',
  "הערות" RICH_TEXT,
  "אמינות" SELECT('מסמך':green, 'הקלטה':blue, 'דיווח':yellow),
  "מקור" RICH_TEXT COMMENT 'מספר המסמך בתיקייה, למשל 0007. או: דיווח, מי ומתי',
  "תאריך המקור" DATE,
  "עודכן" LAST_EDITED_TIME
)
```

| שדה | סוג ב-Notion | באנגלית | מה נכנס |
|---|---|---|---|
| אירוע | Name (העמודה הראשונה) | Event | מה זה, בכמה מילים |
| מועד | Date | When | תאריך, ושעה אם יש |
| סוג | Select | Type: Appointment, Test, Treatment, Hospital stay, Committee, Filing deadline, Call, Other | תור, בדיקה, טיפול, אשפוז, ועדה, מועד הגשה, שיחה, אחר |
| סטטוס | Select | Status: Planned, Took place, Cancelled, To check | מתוכנן, התקיים, בוטל, לבדיקה |
| עם מי | Relation ← אנשים | With | קישור לשורה באנשים |
| איפה | Text | Where | מוסד, מחלקה, חדר |
| הערות | Text | Notes | מה להביא, מה לשאול |
| אמינות | Select | Reliability: Document, Recording, Report | מסמך, הקלטה, דיווח |
| מקור | Text | Source | מספר המסמך, או דיווח |
| תאריך המקור | Date | Source date | התאריך של המסמך או של הדיווח |
| עודכן | Last edited time | Updated | Notion ממלא לבד |

## 5. משימות (`tasks`)

משימות, ושאלות לצוות. התשובה של הצוות נרשמת כמו שנאמרה, עם המקור.

```sql
CREATE TABLE (
  "משימה" TITLE,
  "סוג" SELECT('משימה':blue, 'שאלה לצוות':yellow),
  "סטטוס" SELECT('לביצוע':red, 'בתהליך':blue, 'ממתין':yellow, 'בוצע':green, 'בוטל':gray),
  "אחראי" RELATION('PEOPLE_ID', DUAL 'משימות'),
  "יעד" DATE,
  "תשובה" RICH_TEXT COMMENT 'לשאלה לצוות: מה ענו, כמו שנאמר, ומאיפה',
  "תחום" SELECT('רפואי':red, 'זכויות וביטוח':blue, 'כסף':green, 'אוכל':orange, 'בית ומשפחה':pink, 'כללי':gray),
  "מקור" RICH_TEXT COMMENT 'מספר המסמך בתיקייה, למשל 0007. או: דיווח, מי ומתי',
  "תאריך המקור" DATE,
  "עודכן" LAST_EDITED_TIME
)
```

| שדה | סוג ב-Notion | באנגלית | מה נכנס |
|---|---|---|---|
| משימה | Name (העמודה הראשונה) | Task | מה צריך לעשות או לשאול |
| סוג | Select | Type: Task, Question for the team | משימה, שאלה לצוות |
| סטטוס | Select | Status: To do, In progress, Waiting, Done, Cancelled | לביצוע, בתהליך, ממתין, בוצע, בוטל |
| אחראי | Relation ← אנשים | Owner | מי לוקח |
| יעד | Date | Due | עד מתי |
| תשובה | Text | Answer | לשאלה לצוות: מה ענו, ומאיפה |
| תחום | Select | Area: Medical, Rights and insurance, Money, Food, Home and family, General | רפואי, זכויות וביטוח, כסף, אוכל, בית ומשפחה, כללי |
| מקור | Text | Source | מספר המסמך, או דיווח |
| תאריך המקור | Date | Source date | התאריך של המסמך או של הדיווח |
| עודכן | Last edited time | Updated | Notion ממלא לבד |

## 6. ממתין למחשב (`pending`)

בקשה שהגיעה בטלפון וצריכה קבצים מהמחשב נרשמת כאן. כשדינו נפתח במחשב, הוא מבצע אותה, מסמן "בוצע" וכותב איפה התוצר. הבקשה המלאה נכתבת בתוך העמוד של השורה, כי לשדה טקסט יש תקרת אורך.

```sql
CREATE TABLE (
  "מה התבקש" TITLE COMMENT 'הבקשה במשפט אחד. הבקשה המלאה בתוך העמוד של השורה',
  "מתי" CREATED_TIME,
  "קבצים" RICH_TEXT COMMENT 'אילו קבצים זה צריך, לפי מספר או תיאור',
  "סטטוס" SELECT('חדש':red, 'בתהליך':blue, 'בוצע':green, 'בוטל':gray),
  "איפה התוצר" RICH_TEXT COMMENT 'הנתיב בתיקייה',
  "עודכן" LAST_EDITED_TIME
)
```

| שדה | סוג ב-Notion | באנגלית | מה נכנס |
|---|---|---|---|
| מה התבקש | Name (העמודה הראשונה) | Request | הבקשה במשפט אחד |
| מתי | Created time | Requested | Notion ממלא לבד |
| קבצים | Text | Files | אילו קבצים זה צריך |
| סטטוס | Select | Status: New, In progress, Done, Cancelled | חדש, בתהליך, בוצע, בוטל |
| איפה התוצר | Text | Output location | הנתיב בתיקייה |
| עודכן | Last edited time | Updated | Notion ממלא לבד |

## בנייה ביד

מציגים את זה כשנושן לא מחובר, או לטבלה שהחיבור לא הצליח ליצור. בעברית פשוטה, צעד אחרי צעד:

1. פותחים ב-Notion עמוד חדש, ונותנים לו שם, למשל `Cancel Cancer · אמא`. מעתיקים לראש העמוד את שתי השורות מהסעיף "העמוד הראשי".
2. לכל טבלה, לפי הסדר (אנשים, מקורות, תרופות, תורים ואירועים, משימות, ממתין למחשב): כותבים בעמוד `/database` ובוחרים **Database - Full page**. נותנים לה את השם מהכותרת שלמעלה.
3. העמודה הראשונה נקראת **Name**. לוחצים עליה, בוחרים **Edit property**, ומשנים את השם לשם מהשורה הראשונה בטבלת השדות.
4. כל שדה אחר: לוחצים על **+** מימין לעמודות, בוחרים את הסוג מהעמודה "סוג ב-Notion", ונותנים את השם. ב-**Select** מוסיפים את האפשרויות מהעמודה "מה נכנס".
5. ב-**Relation** בוחרים את הטבלה אנשים, ומדליקים **Show on אנשים**, כדי שהקישור יופיע גם שם.
6. בסוף: מעתיקים את הקישור של העמוד ושל כל טבלה (**Share** ← **Copy link**), ושומרים אותם. דינו יצטרך אותם בפרק 3.

אפשר לבנות רק חלק עכשיו, ולהשלים אחר כך. כשנושן יהיה מחובר, הרצה של הסקיל תמצא את מה שנבנה ותשלים רק מה שחסר.
