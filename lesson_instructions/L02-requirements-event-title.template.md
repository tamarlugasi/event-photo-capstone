# Requirements — Rename event title (L-02)

**Student:**Tamar_Lugasi **Date:**24/10/2026  **Fork URL:**[ ...](https://github.com/tamarlugasi/event-photo-capstone)

## 1. Original request (do not reinterpret)
> I entered the wrong event name. I want to be able to change the event name displayed in the client's gallery.

## 2. Current behavior — verified facts
| Claim | Evidence: file/function/line or HTTP result | Verified? |
| --- | --- | --- |
| Event title exists in data model | `src/types.ts`, הטיפוס `EventRecord`, שדה `title: string` | כן, נקרא בקוד |
| Current client gallery presents title | הרצת `curl.exe -H "x-client-id: client-100" http://localhost:3000/client/events/event-100/gallery`. התשובה: `{"eventId":"event-100","title":"Cohen Wedding","photos":["photo-1","photo-2","photo-3"],"availableServices":[...]}` | כן, אומת בהרצה |
| No rename endpoint exists in inspected route file | `app.ts`: מוגדרים רק `GET /health`, `GET /api/events`, `GET /api/events/:eventId/gallery`, `GET /client/events/:eventId/gallery`, `POST /api/events/:eventId/selections`. אין `PUT` / `PATCH` / `DELETE` | כן, נקרא בקוד (ב-`app.ts` בלבד) |
| לאירוע יש שדה `clientId` אחד בלבד | `src/types.ts`, `EventRecord.clientId: string` | כן, נקרא בקוד |
| האימות הוא כותרות הדגמה בלבד | הערה ב-`app.ts` ("Demo identity only"); `README.md`, Important limitations, סעיף 1 | כן |
| הנתונים נשמרים בזיכרון ומתאפסים באתחול | `README.md`, Important limitations, סעיף 2 | כן, לפי README (לא נבדק בהרצה) |
## 3. Actors / users / use cases / permissions
- Verified: יש בקוד שני סוגי זהות: צלמת (`x-photographer-id`) ולקוחה (`x-client-id`). לאירוע יש `photographerId` ו-`clientId` אחד. אין אימות אמיתי, רק כותרות הדגמה (`app.ts`, `README.md` סעיף 1).
- Must clarify: מי רשאי לשנות שם אירוע: צלמת, לקוחה או שתיהן?

## 4. Goal, scope, out of scope
- Goal: לאפשר תיקון של שם אירוע קיים, כך שהשם המעודכן יוצג בגלריית הלקוחה.
- In scope (confirmed): שינוי שדה `title` של אירוע קיים; הצגת השם המעודכן בגלריית הלקוחה (בקשת המקור, סעיף 1).
- Out of scope (confirmed): עיצוב מסך, בחירת API וכתיבת קוד (גבולות הגזרה של המטלה, שקף 13).
- Undecided: מי רשאי לשנות; הגדרת שם תקין; האם גלריית הצלמת מתעדכנת; האם נדרש תיעוד של השם הקודם (Audit Log, נדחה בשלב זה).

## 5. Functional requirements
- FR-1 (provisional): המערכת תאפשר לגורם מורשה לשנות את שם (`title`) של אירוע קיים. זהות הגורם המורשה טרם הוכרעה.
- FR-2 (provisional): לאחר שינוי השם, גלריית הלקוחה של אותו אירוע תציג את השם החדש.
- FR-3 (provisional): ניסיון לשנות שם של אירוע שאינו קיים יחזיר הודעת "לא נמצא" ולא ישנה נתונים.
## 6. Non-functional requirements / constraints
- NFR-1 (unresolved): הרשאות. כרגע יש רק כותרות הדגמה ולא אימות אמיתי (`README.md` סעיף 1), ולכן לא ניתן להגדיר דרישת אבטחה לפעולה זו לפני החלטה על מי רשאי לשנות.
- NFR-2 (unresolved): שמירה. הנתונים נשמרים בזיכרון ומתאפסים באתחול (`README.md` סעיף 2), ולכן לא ברור אם שינוי שם צריך להישמר לאורך זמן.

## 7. Ten analysis lenses from the preparation assignment
| Term | Question relevant to this change | Answer known? / evidence |
| --- | --- | --- |
| functional requirements | האם השם החדש צריך להתעדכן רק בגלריית הלקוחה, או גם ברשימת האירועים ובגלריה של הצלמת? | לא הוכרע. קיימים שני נתיבי גלריה (`/api/...` ו-`/client/...`) |
| non-functional requirements | האם שינוי השם צריך להישמר לאורך זמן, או שמספיק בזיכרון? | לא הוכרע. הנתונים בזיכרון ומתאפסים באתחול (`README.md`, סעיף 2) |
| actors / users | מי רשאי לשנות את הכותרת: הצלם, הלקוח או שניהם? | לא הוכרע. יש שתי זהויות בקוד (`app.ts`) |
| use cases | הלקוחה מבקשת לתקן שם, ומי מבצע את התיקון בפועל? | לא הוכרע |
| constraints | אילו מגבלות קיימות כרגע? | ידוע: אין אימות אמיתי (`README.md` סעיף 1). לא נמצא נתיב עדכון ב-`app.ts` |
| assumptions | האם גלריית הלקוחה לוקחת את `title` ישירות מ-`EventRecord`? | לא אומת. נדרש `event-service.ts` |
| acceptance criteria | איך נדע שהשינוי הצליח? | מוגדר זמנית בסעיף 9 |
| ambiguity | מה נחשב שם תקין ומה שם שגוי? | לא הוכרע |
| missing information | איפה השם "הוכנס" מלכתחילה, כשב-`app.ts` לא מצאתי נתיב ליצירת אירוע? | לא אומת. ייתכן שהנתונים נטענים מראש מ-`memory-store` |
| scope | האם הבקשה היא רק לשם האירוע, או גם לפרטים אחרים? | בקשת המקור מדברת רק על שם האירוע בגלריית הלקוחה |
## 8. Questions, assumptions, ambiguities
| Question or assumption | Why it matters | Evidence/status | Who decides? |
| --- | --- | --- | --- |
| מי רשאי לשנות שם אירוע: צלמת, לקוחה או שתיהן? | קובע מי יכול להפעיל את הפעולה, ואין הרשאות אמיתיות במערכת | פתוח. רק כותרות הדגמה (`README.md` סעיף 1) | החלטת מוצר |
| מהו שם תקין: ריק, אורך מקסימלי, כפילויות? | בלי זה אי אפשר לכתוב קריטריון קבלה מלא | פתוח. לא נבדק בקוד | החלטת מוצר |
| האם גם גלריית הצלמת (`/api/events/:eventId/gallery`) צריכה להתעדכן? | יש שני נתיבי גלריה | פתוח | החלטת מוצר |
| איפה השם "הוכנס" מלכתחילה? לא מצאתי נתיב ליצירת אירוע ב-`app.ts` | ייתכן שהאירועים נטענים מראש מהנתונים | פתוח. נדרש לבדוק את `memory-store.ts` | בדיקה במאגר |
| האם יש נתיב עדכון בקובץ אחר שאינו `app.ts`? | קובע אם נדרש נתיב חדש | לא אומת. נדרש חיפוש גלובלי | בדיקה במאגר |

## 9. Acceptance criteria (provisional if decisions unresolved)
- Given אירוע קיים ומשתמשת מורשית (מותנה: ההרשאות עדיין לא הוכרעו), when היא משנה את שם האירוע לשם תקין, then גלריית הלקוחה מציגה את השם החדש.
- Given אירוע שאינו קיים, when מבקשים לשנות את שמו, then המערכת מחזירה שגיאה שהאירוע לא נמצא ושום נתון לא משתנה.

## 10. Error cases and tests
- Unknown event: שגיאת "לא נמצא" 
- Unauthorized requester: מותנה בהחלטה על הרשאות
- Empty title: לא מוגדר עדיין, תלוי בהגדרת שם תקין
- Existing client gallery after change: מציגה את השם החדש

## 11. AI checkpoints: understanding / questioning / spec review
Record one claim you verified in source and at least two AI suggestions with adopted/rejected/open status.

**Claim verified in source:** הטיפוס `EventRecord` ב-`src/types.ts` כולל את השדה `title: string`, והרצת curl לגלריית הלקוחה החזירה `"title":"Cohen Wedding"`.

### AI question and spec review suggestions — reviewed critically
| AI suggestion | Essential / later / irrelevant / unverified | Justification | Evidence |
| --- | --- | --- | --- |
| האם `GET /client/events/:eventId/gallery` מחזיר את `title`? | Essential (אומת, אומץ) | זו ליבת הבקשה: מה הלקוחה רואה בגלריה | אומת בהרצת curl: התשובה כוללת `"title":"Cohen Wedding"` |
| האם קיים ראוטר אחר שמטפל בעדכון אירועים? | Essential (פתוח) | הקובע אם צריך נתיב חדש או שכבר קיים | `app.ts` מכיל רק GET ו-POST אחד; נדרש חיפוש גלובלי על put / patch |
| מקומות נוספים שבהם השם מוצג (חשבוניות, מיילים) | Later (נדחה) | ה-README מציין שאין תשלומים ושילוב מעבדות הדפסה | `README.md`, Important limitations, סעיף 6 |
| לשמור היסטוריית שינויים (Audit Log) | Later (נדחה) | אין בקשה כזו מהלקוחה; מרחיב את ה-Scope בלי בסיס | בקשת המקור, סעיף 1 |
| הנחה: הלקוחה רואה את `title` ולא שדה נגזר | Unverified (פתוח) | התשובה בהרצה כוללת `title`, אבל לא נבדק איך שכבת השירות בונה אותה | `types.ts` מגדיר `title`; חסר אימות ב-`event-service.ts` |
| הנחה: היעדר נתיב ב-`app.ts` אומר שאין פיצ'ר בכלל | Unverified (פתוח) | נבדק קובץ אחד בלבד | `app.ts`; נדרש חיפוש גלובלי |
| הנחה: ליוזם הבקשה יש הרשאה לשנות שמות | Essential (פתוח) | קשור ל"מי רשאי לשנות", ואין הרשאות אמיתיות במערכת | `README.md` סעיף 1; הערה ב-`app.ts` ("Demo identity only") |
| UI ייעודי או תיקון חד-פעמי במסד נתונים | Irrelevant (נדחה) | המטלה אוסרת עיצוב מסך ובחירת API, ואין מסד נתונים אלא זיכרון | שקף 13; `README.md` Stack |
| שינוי השם משפיע על כתובת ה-URL (Slug) | Unverified (נדחה) | ה-URL מבוסס על `eventId` ולא על השם | `app.ts`: `/api/events/:eventId/...` |
| ה-AI התייחס לקובץ `image_8b8726.png` שלא סיפקתי לו | Unverified | מקור שלא הובא לו, ולכן אי אפשר לסמוך על טענות שנשענות עליו | התשובה של ה-AI |

## 12. Remaining decisions (do not silently invent answers)
- מי רשאי לשנות שם
- הגדרת שם תקין
- האם גלריית הצלמת מתעדכנת גם היא

## 13. Personal reflection
- One fact discovered through code review: הגלריה של הלקוחה (`GET /client/events/:eventId/gallery`) מחזירה את השדה `title` של האירוע (בהרצה: "Cohen Wedding" באירוע `event-100`), ובקובץ `app.ts` אין אף נתיב שמשנה אירוע קיים. הנתיב היחיד שכותב נתונים הוא `POST /api/events/:eventId/selections`.
- One AI suggestion you rejected or postponed, and why: דחיתי את ההצעה "UI ייעודי או תיקון חד-פעמי במסד נתונים", כי המטלה אוסרת עיצוב מסך ובחירת API, ובפועל אין מסד נתונים אלא אחסון בזיכרון בלבד (`README.md`). דחיתי גם את ההצעה "Audit Log", כי אין בקשה כזו מהלקוחה והיא מרחיבה את ה-Scope בלי בסיס.