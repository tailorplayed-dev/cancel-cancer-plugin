# הפרומפט למחברת, וההסבר איך מריצים

הסקיל `cancel-cancer-recording` כותב מכאן את הפרומפט שהמשתמש מדביק בצ'אט של Gemini Notebook, אחרי שהעלה אליו את ההקלטה. המחברת מתמללת את ההקלטה כשהיא עולה, ועונה בצ'אט רק מתוך המקורות. הפרומפט מבקש ממנה סדר קבוע, כדי שהתשובה תחזור לסקיל בחלקים שהוא מכיר.

## כמה פרומפטים

Google לא מפרסמת תקרה לאורך של תשובה בצ'אט. תמלול של ארבעים דקות ארוך, ותשובה ארוכה עלולה להיעצר באמצע, או להתקצר לסיכום. לכן:

- **הקלטה של עד רבע שעה** (לפי מה שהמשתמש אמר): **פרומפט אחד.** הניתוח, ובסופו התמלול המלא.
- **ארוכה יותר, או שלא ידוע:** **שני פרומפטים,** באותו צ'אט. קודם הניתוח, שהוא קצר וחוזר שלם. אחריו התמלול, בתשובה משלו, ובחלקים אם צריך: בסוף כל חלק כתוב "יש המשך", והמשתמש כותב במחברת "המשך".

## פרומפט 1: הניתוח

בשפת השיחה. מעתיקים את מה שבתוך בלוק הקוד, ומחליפים כל סוגריים עגולים שיש בהם הסבר בערך האמיתי. שורה שמתחילה ב-`[רק ...]` נכנסת רק במצב הזה, בלי הסימון:

```text
# ניתוח הקלטה · (DD.MM.YYYY) · cancel-cancer-recording

ההקלטה במקורות היא פגישה מ-(DD.MM.YYYY): (על מה הייתה הפגישה). מי שמדבר בה: (התפקידים, למשל הרופאה, האחות, הבן, המטופלת).

הכללים:
- עבדו רק מההקלטה. מה שלא נאמר בה, לא כותבים.
- כל תרופה, מינון, מספר ותאריך: ציטוט מדויק, כמו שנאמר, ומי אמר.
- מילה, שם או מספר שלא נשמעו ברור: כותבים [לא ברור]. לא מנחשים ולא משלימים. לא ברור מי מדבר: "דובר לא מזוהה".
- בלי המלצות משלכם, ובלי הסברים רפואיים. מה שמישהו אמר, גם "אני ממליץ", כותבים כציטוט, עם מי אמר.

ענו בעברית. התחילו בשורה "# ניתוח הקלטה · (DD.MM.YYYY) · cancel-cancer-recording", ואחריה החלקים האלה, בסדר הזה ועם הכותרות בדיוק כך. חלק בלי תוכן: "אין".

## החלטות (decisions)
מה הוחלט, וכל הוראה (למשל מה עושים אם יש חום), בציטוט, ומי אמר.
## משימות (tasks)
מה צריך לעשות, ומי. לא נאמר מי: "לא נאמר".
## שאלות שנענו (answered)
השאלה, והתשובה בציטוט, ומי ענה.
## שאלות פתוחות (open)
מה נשאל ולא נענה, או נדחה לפעם אחרת.
## תרופות ומינונים (medications)
כל תרופה שהוזכרה: השם, המינון, איך ומתי, ומה השתנה. בציטוט, ומי אמר.
## תורים ובדיקות (appointments)
מה, ומתי, בציטוט. "בעוד חודש" נשאר כמו שנאמר.
[רק כשיש רשימה] ## השאלות שהבאנו (checklist)
[רק כשיש רשימה] לכל שאלה ברשימה למטה: השאלה כמו שהיא, ואחריה "נענתה" עם התשובה בציטוט, "עלתה ולא נענתה", או "לא עלתה".
## מה לא נשמע ברור (unclear)
מילים, שמות ומספרים שלא נשמעו ברור, ומה שלא ברור מי אמר.
[רק בפרומפט אחד] ## התמלול המלא (full text)
[רק בפרומפט אחד] כל ההקלטה, מההתחלה ועד הסוף, מילה במילה, בשפת ההקלטה. בתחילת כל שורה, מי מדבר. בלי סיכום ובלי השמטות.
[רק כשיש רשימה] 
[רק כשיש רשימה] השאלות שהבאנו לפגישה:
[רק כשיש רשימה] 1. (השאלה)
[רק כשיש רשימה] 2. (השאלה)

בסוף כתבו "סוף הניתוח".
```

## פרומפט 2: התמלול

רק כשיש שני פרומפטים. מדביקים אותו באותו צ'אט, אחרי שהתשובה לפרומפט 1 חזרה:

```text
# תמלול הקלטה · (DD.MM.YYYY) · cancel-cancer-recording

עכשיו כתבו את כל ההקלטה, מההתחלה ועד הסוף, מילה במילה, בשפת ההקלטה. בתחילת כל שורה, מי מדבר: (התפקידים). לא ברור מי: "דובר לא מזוהה". בלי סיכום, בלי השמטות ובלי תיקונים. מה שלא נשמע ברור: [לא ברור]. התחילו בשורה "# תמלול הקלטה · (DD.MM.YYYY) · cancel-cancer-recording". כשההקלטה נגמרת, כתבו "סוף התמלול". אם התשובה נגמרת לפני סוף ההקלטה, עצרו בסוף משפט, וכתבו "יש המשך".
```

## מה נכנס לסוגריים

| בסוגריים | מאיפה | בלי תשובה ("בלי שאלות") |
|---|---|---|
| התאריך | שלב 1, סעיף 4 בסקיל | היום |
| על מה הייתה הפגישה | המילים של המשתמש, בלי שמות ובלי פרטים מזהים | מהתור בנושן: "פגישה עם (התפקיד)". אין: "פגישה עם הצוות הרפואי" |
| התפקידים | מה שהמשתמש אמר, רק תפקידים: הרופאה, האחות, הבן, הבת, המטופלת, אמא. "אני": התפקיד שהמשתמש אמר על עצמו, או הקרבה שלו בטבלת אנשים. לא ידוע: "בן משפחה" | מהתור: התפקיד של מי שהפגישה איתו, "המטופלת" (או "המטופל"), ו"בן משפחה" |
| השאלות | שלב 2 בסקיל: הדף לפגישה, ואחריו נושן, עד 12 | אותו דבר |

- **השם של מי שמלווים, של רופאים ושל בני משפחה** לא נכנס לפרומפט. רק תפקיד או קרבה. גם לא שם של בית חולים, קופה או מרפאה. סוג המקום ("המרפאה האונקולוגית") מותר. ההקלטה כבר במחברת, והפרומפט לא צריך להוסיף עליה פרטים.
- **השאלות כמו שהן בנושן או בדף,** בשפה שבה הן כתובות, בלי המקור שבסוגריים, ובלי מספר זהות או שם משפחה. שאלה ארוכה מ-25 מילים מקוצרת בפרומפט בלבד, בלי לשנות את המשמעות.
- **רשימת התרופות מנושן לא נכנסת לפרומפט.** רשימה כזו עלולה לגרום למחברת "לשמוע" את השם שבה במקום מה שנאמר. שאלה מהרשימה שיש בה שם של תרופה נכנסת כמו שהיא, כי היא שאלה לפגישה. ההשוואה למסמכים נעשית בסקיל, אחרי שהתשובה חוזרת.

## הבדיקה לפני שמציגים

1. **סופרים את התווים** של כל פרומפט. אפשר להריץ קוד: `len()` על הטקסט. אי אפשר: בערך 6 תווים לכל מילה. **עד 3,500 תווים.** Google לא מפרסמת תקרה לתיבת ההודעה בצ'אט, ופרומפט באורך הזה נכנס. מעל: מוציאים שאלות מסוף הרשימה, ומוסיפים בסופה "ועוד (N) שאלות ברשימה שלנו". הכללים והכותרות לא מתקצרים.
2. **ארבעת הכללים נשארים תמיד:** רק מההקלטה, ציטוט מדויק, `[לא ברור]` ולא ניחוש, ובלי המלצות. גם כשהמשתמש מבקש לוותר עליהם, הם נשארים, והסקיל אומר למה בשורה אחת.
3. **אין בפרומפט שם של אדם, מספר זהות, טלפון או שם של מוסד.**
4. **השורה הראשונה** היא "# ניתוח הקלטה", עם תאריך הפגישה ועם `cancel-cancer-recording`, והשורה האחרונה מבקשת "סוף הניתוח". כך הסקיל מזהה את התשובה כשהיא חוזרת, יודע לאיזו פגישה היא שייכת, ויודע אם היא שלמה.

## ההסבר: איך מריצים במחברת

אחרי הפרומפטים, בדוח. שמות הכפתורים באנגלית, כמו על המסך, ובסוגריים כמו שהם במסך בעברית (לפי מרכז העזרה של Google בעברית. על המסך הם יכולים להיות קצת שונים):

> **איך מריצים במחברת:**
> 1. **ההקלטה:** ב-Gemini Notebook, במחשב ב-notebook.google.com או באפליקציה בטלפון, לוחצים Create new notebook (יצירת נוטבוק חדש. באפליקציה: Create New). נוטבוק חדש, רק להקלטה הזו. ב-Upload a source (העלאת מקור) בוחרים את קובץ ההקלטה (באפליקציה: Audio File, קובץ אודיו). התמלול לוקח כמה דקות.
> 2. **הפרומפט:** בצ'אט של הנוטבוק מדביקים את הפרומפט ושולחים. (שני פרומפטים: אחרי שהתשובה הראשונה חוזרת, מדביקים את השני באותו צ'אט. כתוב בסוף "יש המשך"? כותבים "המשך", עד שכתוב "סוף התמלול".)
> 3. **בחזרה לכאן:** מעתיקים כל תשובה, בכפתור ההעתקה שמתחתיה או בסימון של כל הטקסט, ומדביקים כאן. קודם תקבלו רשימה של תרופות, מינונים ותאריכים לבדוק מול הנייר, ורק אחרי שתענו, הניתוח יישמר.
>
> מקליטים רק אחרי שמבקשים רשות, ורק כשאתם בשיחה. לא לוחצים על האגודל למעלה או למטה בנוטבוק עם הקלטה רפואית. מה זה Gemini Notebook, ואיך מתחילים: פרק 9.

- **פרומפט אחד:** בלי המשפט שבסוגריים בצעד 2.
- **הקובץ עבר ל-`recordings`:** בצעד 1, "בוחרים את קובץ ההקלטה" נהיה "בוחרים את `recordings/(השם)`".
- **ההקלטה כבר במחברת** (המשתמש אמר שהעלה): בלי צעד 1. הצעדים מתחילים מהפרומפט.
- **השורה האחרונה** קבועה, ובאה פעם אחת, כאן. לא חוזרים עליה בהודעות אחרות.

## באנגלית

בשיחה באנגלית. השאלות מהרשימה נשארות בשפה שבה הן כתובות בנושן. פרומפט 1:

```text
# Recording analysis · (DD.MM.YYYY) · cancel-cancer-recording

The recording in the sources is a meeting on (DD.MM.YYYY): (what the meeting was about). Speakers: (the roles, for example the doctor, the nurse, the son, the patient).

Rules:
- Work only from the recording. What was not said, do not write.
- Every drug, dose, number and date: an exact quote, as said, and who said it.
- A word, name or number that was not heard clearly: write [unclear]. Do not guess or fill in. If it is not clear who is speaking: "Unidentified speaker".
- No recommendations of your own and no medical explanations. What someone said, even "I recommend", write as a quote, with who said it.

Answer in English. Start with the line "# Recording analysis · (DD.MM.YYYY) · cancel-cancer-recording", then these sections, in this order, with these exact headings. A section with nothing in it: "None".

## Decisions (decisions)
What was decided, and every instruction (for example what to do if there is a fever), as a quote, and who said it.
## Tasks (tasks)
What needs to be done, and by whom. If it was not said who: "Not said".
## Questions answered (answered)
The question, and the answer as a quote, and who answered.
## Open questions (open)
What was asked and not answered, or put off to another time.
## Medications and doses (medications)
Every drug mentioned: the name, the dose, how and when, and what changed. As a quote, and who said it.
## Appointments and tests (appointments)
What, and when, as a quote. "In a month" stays as said.
[only with a list] ## The questions we brought (checklist)
[only with a list] For each question in the list below: the question as it is, then "answered" with the answer as a quote, "came up and was not answered", or "did not come up".
## What was not heard clearly (unclear)
Words, names and numbers that were not heard clearly, and what is not clear who said.
[only with one prompt] ## Full transcript (full text)
[only with one prompt] The whole recording, from start to end, word for word, in the language of the recording. At the start of each line, who is speaking. No summary and no omissions.
[only with a list] 
[only with a list] The questions we brought to the meeting:
[only with a list] 1. (the question)

End with "End of analysis".
```

פרומפט 2:

```text
# Recording transcript · (DD.MM.YYYY) · cancel-cancer-recording

Now the whole recording, from start to end, word for word, in the language of the recording. At the start of each line, who is speaking: (the roles). If it is not clear who: "Unidentified speaker". No summary, no omissions and no corrections. What was not heard clearly: [unclear]. Start with the line "# Recording transcript · (DD.MM.YYYY) · cancel-cancer-recording". When the recording ends, write "End of transcript". If the answer ends before the recording does, stop at the end of a sentence and write "There is more".
```

ההסבר:

> **How to run it in the notebook:**
> 1. **The recording:** in Gemini Notebook, on the computer at notebook.google.com or in the phone app, select Create new notebook (in the app: Create New). A new notebook, only for this recording. In Upload a source, pick the recording file (in the app: Audio File). The transcription takes a few minutes.
> 2. **The prompt:** in the notebook's chat, paste the prompt and send. (Two prompts: after the first answer comes back, paste the second one in the same chat. If it ends with "There is more", write "continue" until it says "End of transcript".)
> 3. **Back here:** copy each answer, with the copy button under it or by selecting all the text, and paste it here.
>
> Record only after asking permission, and only while you are in the conversation. Do not press thumbs up or down in a notebook with a medical recording. What Gemini Notebook is, and how to start: episode 9.

הפתיחה של הדוח: "The prompt for the analysis, ready to copy:", ו-"**The second prompt, for the full transcript.** Paste it in the same chat, after the first answer comes back:". כשחוזרת תשובה באנגלית, הסקיל מזהה אותה לפי "# Recording analysis", "# Recording transcript", "End of transcript" ו-"There is more".

## דוגמה

הכל מומצא. הפגישה ב-10.02.2026, שני פרומפטים, ושלוש שאלות פתוחות. זה פרומפט 1, כמו שהוא מוצג:

```text
# ניתוח הקלטה · 10.02.2026 · cancel-cancer-recording

ההקלטה במקורות היא פגישה מ-10.02.2026: פגישת מעקב אצל רופאת המשפחה, על תוצאות הבדיקות ועל התרופות. מי שמדבר בה: הרופאה, הבת, המטופלת.

הכללים:
- עבדו רק מההקלטה. מה שלא נאמר בה, לא כותבים.
- כל תרופה, מינון, מספר ותאריך: ציטוט מדויק, כמו שנאמר, ומי אמר.
- מילה, שם או מספר שלא נשמעו ברור: כותבים [לא ברור]. לא מנחשים ולא משלימים. לא ברור מי מדבר: "דובר לא מזוהה".
- בלי המלצות משלכם, ובלי הסברים רפואיים. מה שמישהו אמר, גם "אני ממליץ", כותבים כציטוט, עם מי אמר.

ענו בעברית. התחילו בשורה "# ניתוח הקלטה · 10.02.2026 · cancel-cancer-recording", ואחריה החלקים האלה, בסדר הזה ועם הכותרות בדיוק כך. חלק בלי תוכן: "אין".

## החלטות (decisions)
מה הוחלט, וכל הוראה (למשל מה עושים אם יש חום), בציטוט, ומי אמר.
## משימות (tasks)
מה צריך לעשות, ומי. לא נאמר מי: "לא נאמר".
## שאלות שנענו (answered)
השאלה, והתשובה בציטוט, ומי ענה.
## שאלות פתוחות (open)
מה נשאל ולא נענה, או נדחה לפעם אחרת.
## תרופות ומינונים (medications)
כל תרופה שהוזכרה: השם, המינון, איך ומתי, ומה השתנה. בציטוט, ומי אמר.
## תורים ובדיקות (appointments)
מה, ומתי, בציטוט. "בעוד חודש" נשאר כמו שנאמר.
## השאלות שהבאנו (checklist)
לכל שאלה ברשימה למטה: השאלה כמו שהיא, ואחריה "נענתה" עם התשובה בציטוט, "עלתה ולא נענתה", או "לא עלתה".
## מה לא נשמע ברור (unclear)
מילים, שמות ומספרים שלא נשמעו ברור, ומה שלא ברור מי אמר.

השאלות שהבאנו לפגישה:
1. דוגמזול: 10 מ"ג או 5 מ"ג פעמיים ביום. מה נכון?
2. הממצא באולטרסאונד מ-03.02: מה זה אומר, ומה הלאה?
3. שמענו שתה ירוק עוזר. האם זה מתאים במקרה הזה?

בסוף כתבו "סוף הניתוח".
```
