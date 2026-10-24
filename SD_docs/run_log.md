# יומן הרצה

תאריך: 24/10/2026
מערכת הפעלה: Windows

## node --version
- תוצאה: v24.20.0
- האם עבד: כן
- מה הפקודה עושה: מציגה את גרסת Node.js המותקנת. ה-README דורש Node.js 20 ומעלה, והגרסה שלי עומדת בדרישה.

## npm install
- תוצאה: הצליח
- מה הופיע בטרמינל: `added 160 packages, and audited 161 packages in 20...` 
- מה הפקודה עושה: מתקינה את התלויות המוגדרות ב-`package.json` (Express, TypeScript, Vitest, Supertest ועוד) לתיקיית `node_modules`.
- מה היה צריך לתקן: לא נדרש שום תיקון.

## npm test
- תוצאה: כל הבדיקות עברו, 0 נכשלו.
- מה הפקודה עושה: מריצה את הבדיקות האוטומטיות עם Vitest (`vitest run`).
- הבדיקות שרצו (לפי השמות שהוצגו בטרמינל):
  1. requires a demo photographer identity
  2. lists only events belonging to the requested photographer
  3. does not return another photographer's event
  4. allows a client to submit a selection for their own gallery
  5. does not let a client access a different client's gallery
- מה אומת: הבדיקות רצות ועוברות אצלי במחשב. הבדיקות מכסות סינון לפי צלמת ולפי לקוחה.
- מה לא אומת:
  - אין אימות אמיתי. הזהות מגיעה מכותרות דמו (`x-photographer-id`, `x-client-id`), ולכן הבדיקות אינן מוכיחות הרשאות אבטחה אמיתיות (README, "Important limitations" סעיף 1).
  - לא נבדקו ולידציה של תוכן הבקשה, שינוי שם אירוע וכל נתיב שאינו מופיע בשמות הבדיקות.

## npm run typecheck
- תוצאה: עבד ללא שגיאות
- מה הפקודה עושה: בודקת את טיפוסי TypeScript בלי ליצור קבצים (`tsc --noEmit`).
- מה אומת: אין שגיאות טיפוסים בקוד.
- מה לא אומת: הבדיקה אינה מוכיחה שההתנהגות בזמן ריצה נכונה.

## npm run dev
- תוצאה: השרת עלה ומאזין
- כתובת ופורט: `Event Photo API listening on http://localhost:3000`
- מה הפקודה עושה: מפעילה את השרת במצב פיתוח (`tsx watch src/server.ts`), ומפעילה אותו מחדש בכל שינוי בקובץ.
- מה אומת: השרת עולה ומדפיס הודעת האזנה.

## סיכום
- הכל הותקן ורץ ללא שגיאות, ולא נדרש שום שינוי או תיקון.
- הבדיקות עברו (5 מתוך 5), בדיקת הטיפוסים עברה, והשרת מאזין ב-`http://localhost:3000`.
