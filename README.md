# Cancel Cancer · הערכה

ערכה חינמית של Benefits: סקילים ל-Claude שעוזרים לסדר את כל מה שמסביב למחלה של מישהו שאתם אוהבים. מסמכים, מיילים, תורים, הקלטות, כסף ומשימות במשפחה. כל פרק בסדרה מוסיף סקיל אחד.

כלי לסידור, לא ייעוץ רפואי, פיננסי או משפטי. כל החלטה ← עם הצוות המטפל. בחירום: מד"א 101.

## מה יש בריפו

| נתיב | מה זה |
|---|---|
| `plugins/cancel-cancer/` | הפלאגין. הסקילים נמצאים ב-`skills/`, ומה השתנה ב-`CHANGELOG.md` |
| `downloads/start-here.md` | הקובץ ששמים בתיקייה ריקה במחשב, לפני שמתחילים |
| `shared/cancel-cancer-contract.md` | החוזה של הערכה. זה המקור, ועותק זהה שלו נמצא בכל סקיל |
| `.claude-plugin/marketplace.json` | ה-marketplace, שדרכו מתקינים |

## איך מתקינים (פעם אחת)

צריך Claude בתשלום, ואת האפליקציה של Claude במחשב.

1. ב-Settings ← Capabilities מדליקים את Code execution and file creation.
2. Customize ← Plugins ← Add ← Add marketplace. מדביקים את הכתובת של העמוד הזה ב-GitHub, ולוחצים Add.
3. ב-Discover בוחרים את Cancel Cancer, ולוחצים Add.
4. מדליקים את Sync automatically, כדי שעדכונים יגיעו לבד.

## אחרי ההתקנה

1. יוצרים תיקייה ריקה במחשב, ושומרים בה את `downloads/start-here.md`.
2. פותחים שיחה, מוסיפים את התיקייה מתחת לתיבת ההודעה, וכותבים: "תקים לי את מפת התיקיות לפי ההסבר בקובץ start-here."
3. מחברים את Notion: Customize ← Connectors, מחפשים Notion ולוחצים Connect to Claude (או בדף הפלאגין, בלשונית Connectors). אחר כך כותבים בשיחה: "תקים לי את נושן של הסדרה."

## עדכונים

גרסה חדשה מגיעה לבד כש-Sync automatically דלוק. אפשר גם ללחוץ Check for updates בדף הפלאגין.

## למי שמתחזק את הערכה

- **כל העלאה מעלה את `version`** ב-`plugins/cancel-cancer/.claude-plugin/plugin.json`. בלי זה, המשתמשים לא מקבלים את השינוי.
- **באותה העלאה,** בכל סקיל של הערכה: `kit-version` ב-`metadata` ובשורת הגרסה עולה לאותו מספר. `skill-version` עולה רק כשהסקיל עצמו השתנה.
- **לא משנים אף פעם `name`** של הפלאגין או של סקיל.
- **החוזה נערך רק ב-`shared/`,** ומועתק כמו שהוא ל-`references/cancel-cancer-contract.md` של כל סקיל.
- **שורה ב-`CHANGELOG.md`** לכל גרסה.

---

Cancel Cancer · Benefits · כלי לסידור, לא ייעוץ רפואי, פיננסי או משפטי · החלטות ← עם הצוות המטפל · בחירום 101
