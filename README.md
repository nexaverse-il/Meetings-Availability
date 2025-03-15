# Airtable Meeting Availability Automation

אוטומציה לניהול זמני פגישות בהתאם להגדרות מותאמות אישית בסביבת Airtable. המערכת מייצרת באופן אוטומטי חלונות זמן פנויים לפגישות לפי שעות פעילות מוגדרות.

🛠 שימוש
יצירה אוטומטית יומית
האוטומציה תרוץ באופן אוטומטי מדי יום ב-6:00 בבוקר ותיצור את חלונות הזמן הפנויים לאותו יום בהתאם להגדרות שבטבלת Business Research.
יצירה מרוכזת לטווח תאריכים
אם ברצונך ליצור חלונות זמן לטווח תאריכים ארוך יותר:

הגדר את "Generate Time Slots Until" לתאריך הסיום הרצוי
סמן את תיבת הסימון "Generate Time Slots"
האוטומציה הייעודית תרוץ ותיצור את כל חלונות הזמן עד לתאריך שהוגדר

## 🌟 סקירה כללית

מערכת אוטומציה זו מאפשרת לך להגדיר שעות פעילות בטבלת הגדרות, והמערכת תיצור באופן אוטומטי חלונות זמן פנויים לפגישות בטבלת זמינות. ניתן להגדיר:

- שעות פעילות לכל יום בשבוע
- משך זמן לכל פגישה
- האם יום מסוים פעיל או לא

האוטומציה פועלת בכל יום ב-6:00 בבוקר ויוצרת את חלונות הזמן ליום הנוכחי בלבד, כדי לשמור על טבלת הזמינות נקייה ומעודכנת.

## 📋 דרישות מקדימות

- חשבון Airtable
- הרשאות ליצירת אוטומציות
- שתי טבלאות ב-Base:
  - `Business Research ⚙️` - להגדרות שעות פעילות
  - `Meetings Availability🌻` - לזמני הפגישות הפנויים

## 🔧 הגדרה

### 1. הגדרת טבלת Business Research

הוסף את השדות הבאים לטבלת `Business Research ⚙️`:

| שדה | סוג | תיאור |
|------|------|--------|
| Availability Settings Active | Checkbox | האם הגדרות הזמינות פעילות |
| Default Meeting Duration | Duration | משך הזמן הסטנדרטי לפגישה |
| Sunday Active | Checkbox | האם יום ראשון פעיל |
| Sunday Start Time | Single line text | שעת התחלת פעילות ביום ראשון (פורמט: HH:MM) |
| Sunday End Time | Single line text | שעת סיום פעילות ביום ראשון (פורמט: HH:MM) |
| Monday Active | Checkbox | האם יום שני פעיל |
| Monday Start Time | Single line text | שעת התחלת פעילות ביום שני |
| Monday End Time | Single line text | שעת סיום פעילות ביום שני |
| Tuesday Active | Checkbox | האם יום שלישי פעיל |
| Tuesday Start Time | Single line text | שעת התחלת פעילות ביום שלישי |
| Tuesday End Time | Single line text | שעת סיום פעילות ביום שלישי |
| Wednesday Active | Checkbox | האם יום רביעי פעיל |
| Wednesday Start Time | Single line text | שעת התחלת פעילות ביום רביעי |
| Wednesday End Time | Single line text | שעת סיום פעילות ביום רביעי |
| Thursday Active | Checkbox | האם יום חמישי פעיל |
| Thursday Start Time | Single line text | שעת התחלת פעילות ביום חמישי |
| Thursday End Time | Single line text | שעת סיום פעילות ביום חמישי |
| Friday Active | Checkbox | האם יום שישי פעיל |
| Friday Start Time | Single line text | שעת התחלת פעילות ביום שישי |
| Friday End Time | Single line text | שעת סיום פעילות ביום שישי |
| Saturday Active | Checkbox | האם יום שבת פעיל |
| Saturday Start Time | Single line text | שעת התחלת פעילות ביום שבת |
| Saturday End Time | Single line text | שעת סיום פעילות ביום שבת |
| Generate Time Slots | Checkbox | סמן כדי להפעיל ייצור מיידי של חלונות זמן |
| Generate Time Slots Until | Date | תאריך סיום לייצור מרוכז של חלונות זמן |

### 2. הגדרת טבלת Meetings Availability

ודא שטבלת `Meetings Availability🌻` כוללת את השדות הבאים:

| שדה | סוג | תיאור |
|------|------|--------|
| Time Slots | Date & Time | תאריך ושעה של חלון הזמן הפנוי |
| Dration⏳ | Duration | משך הזמן של חלון הזמן הפנוי |
| Notes 📋 | Long text | הערות אופציונליות |
| Scheduled Meetings 🤝 | Link to another record | קישור לפגישות שנקבעו |
| Leads 🧲 | Link to another record | קישור ללידים |
| Clients 🌟 | Link to another record | קישור ללקוחות |

### 3. הגדרת האוטומציה היומית

1. צור אוטומציה חדשה ב-Airtable
2. הגדר טריגר "At scheduled time" 
3. בחר בסוג מרווח "Days" וקבע את המרווח ל-1 (כל יום)
4. קבע את שעת ההפעלה ל-6:00 בבוקר
5. הוסף צעד "Run script" והעתק את קוד האוטומציה היומית מהקובץ `daily-availability-generator.js`

## 📝 הקודים

### `daily-availability-generator.js`

סקריפט זה מייצר חלונות זמן פנויים לפגישות עבור היום הנוכחי בלבד, על פי הגדרות שעות הפעילות:

```javascript
// Get business research record
const businessResearchTable = base.getTable('Business Research ⚙️');
const meetingsAvailabilityTable = base.getTable('Meetings Availability🌻');

// Helper function for output that works in any environment
function logOutput(message) {
    console.log(message);
    
    // Try different output methods depending on environment
    try {
        if (typeof output !== 'undefined' && output.text) {
            output.text(message);
        }
    } catch (e) {
        // Ignore errors with output methods
    }
}

// Query for active business research records
const query = await businessResearchTable.selectRecordsAsync({
    filterByFormula: "{Availability Settings Active} = TRUE()"
});

if (query.records.length === 0) {
    logOutput("No records with active availability settings found. Aborting.");
    return;
}

// Use the first matching record
const record = query.records[0];

// Get time slots configuration
const defaultDuration = record.getCellValue('Default Meeting Duration') 
    ? record.getCellValue('Default Meeting Duration').seconds 
    : 1800; // default 30 minutes

// Get current date (run only for today)
const today = new Date();
const dayOfWeek = today.getDay();

// Check if we have settings for this day
const daysConfig = {
    0: { // Sunday
        active: record.getCellValue('Sunday Active'),
        startTime: record.getCellValue('Sunday Start Time'),
        endTime: record.getCellValue('Sunday End Time')
    },
    1: { // Monday
        active: record.getCellValue('Monday Active'),
        startTime: record.getCellValue('Monday Start Time'),
        endTime: record.getCellValue('Monday End Time')
    },
    2: { // Tuesday
        active: record.getCellValue('Tuesday Active'),
        startTime: record.getCellValue('Tuesday Start Time'),
        endTime: record.getCellValue('Tuesday End Time')
    },
    3: { // Wednesday
        active: record.getCellValue('Wednesday Active'),
        startTime: record.getCellValue('Wednesday Start Time'),
        endTime: record.getCellValue('Wednesday End Time')
    },
    4: { // Thursday
        active: record.getCellValue('Thursday Active'),
        startTime: record.getCellValue('Thursday Start Time'),
        endTime: record.getCellValue('Thursday End Time')
    },
    5: { // Friday
        active: record.getCellValue('Friday Active'),
        startTime: record.getCellValue('Friday Start Time'),
        endTime: record.getCellValue('Friday End Time')
    },
    6: { // Saturday
        active: record.getCellValue('Saturday Active'),
        startTime: record.getCellValue('Saturday Start Time'),
        endTime: record.getCellValue('Saturday End Time')
    }
};

const dayConfig = daysConfig[dayOfWeek];

// Skip if this day is not active or missing time settings
if (!dayConfig.active || !dayConfig.startTime || !dayConfig.endTime) {
    const dayNames = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];
    logOutput(`Today (${dayNames[dayOfWeek]}) is not an active day or has incomplete time settings. Skipping.`);
    return;
}

// Time slots to create
const timeSlots = [];

// Parse start and end times
const [startHour, startMinute] = dayConfig.startTime.split(':').map(Number);
const [endHour, endMinute] = dayConfig.endTime.split(':').map(Number);

// Create DateTime objects for today's start and end times
const startDateTime = new Date(today);
startDateTime.setHours(startHour, startMinute, 0, 0);

const endDateTime = new Date(today);
endDateTime.setHours(endHour, endMinute, 0, 0);

// If current time is after the start time, adjust the start time to be the next slot
const now = new Date();
if (now > startDateTime) {
    // Calculate the next available time slot
    const minutesSinceStart = Math.floor((now - startDateTime) / 1000 / 60);
    const slotsSinceStart = Math.ceil(minutesSinceStart / (defaultDuration / 60));
    const additionalMinutes = slotsSinceStart * (defaultDuration / 60);
    
    startDateTime.setMinutes(startDateTime.getMinutes() + additionalMinutes);
    
    // If the adjusted start time is after end time, there are no more slots for today
    if (startDateTime >= endDateTime) {
        logOutput("No more available time slots for today.");
        return;
    }
}

// Generate time slots for today
for (let slotTime = new Date(startDateTime); slotTime < endDateTime; slotTime.setSeconds(slotTime.getSeconds() + defaultDuration)) {
    // Skip slots that have already passed
    if (slotTime > now) {
        timeSlots.push({
            fields: {
                'Time Slots': slotTime.toISOString(),
                'Dration⏳': defaultDuration
            }
        });
    }
}

if (timeSlots.length === 0) {
    logOutput("No future time slots available for today.");
    return;
}

// Create records in batches of 50 (Airtable limit)
const batchSize = 50;
let createdCount = 0;
for (let i = 0; i < timeSlots.length; i += batchSize) {
    const batch = timeSlots.slice(i, i + batchSize);
    const result = await meetingsAvailabilityTable.createRecordsAsync(batch);
    createdCount += result.length;
    logOutput(`Created batch ${Math.floor(i/batchSize) + 1} of ${Math.ceil(timeSlots.length/batchSize)} (${createdCount}/${timeSlots.length} slots)`);
}

const resultMessage = `Created ${timeSlots.length} meeting availability slots for today (${today.toLocaleDateString()}).`;
logOutput(resultMessage);


bulk-availability-generator.js
סקריפט זה מייצר חלונות זמן פנויים לפגישות לטווח תאריכים ארוך, לפי הגדרות שעות הפעילות:

// Get business research record
const businessResearchTable = base.getTable('Business Research ⚙️');
const meetingsAvailabilityTable = base.getTable('Meetings Availability🌻');

// Helper function for output that works in any environment
function logOutput(message) {
    console.log(message);
    
    // Try different output methods depending on environment
    try {
        if (typeof output !== 'undefined' && output.text) {
            output.text(message);
        }
    } catch (e) {
        // Ignore errors with output methods
    }
}

// Query for records with Generate Time Slots = true
const query = await businessResearchTable.selectRecordsAsync({
    filterByFormula: "{Generate Time Slots} = TRUE()"
});

if (query.records.length === 0) {
    logOutput("No records with Generate Time Slots set to true found. Aborting.");
    return;
}

// Use the first matching record
const record = query.records[0];
const recordId = record.id;

// Check if the trigger is active
if (!record.getCellValue('Availability Settings Active')) {
    logOutput('Availability settings are not active. Aborting.');
    return;
}

// Get time slots configuration
const defaultDuration = record.getCellValue('Default Meeting Duration') 
    ? record.getCellValue('Default Meeting Duration').seconds 
    : 1800; // default 30 minutes

const endDate = new Date(record.getCellValue('Generate Time Slots Until'));
const currentDate = new Date();

// Days configuration
const daysConfig = {
    0: { // Sunday
        active: record.getCellValue('Sunday Active'),
        startTime: record.getCellValue('Sunday Start Time'),
        endTime: record.getCellValue('Sunday End Time')
    },
    1: { // Monday
        active: record.getCellValue('Monday Active'),
        startTime: record.getCellValue('Monday Start Time'),
        endTime: record.getCellValue('Monday End Time')
    },
    2: { // Tuesday
        active: record.getCellValue('Tuesday Active'),
        startTime: record.getCellValue('Tuesday Start Time'),
        endTime: record.getCellValue('Tuesday End Time')
    },
    3: { // Wednesday
        active: record.getCellValue('Wednesday Active'),
        startTime: record.getCellValue('Wednesday Start Time'),
        endTime: record.getCellValue('Wednesday End Time')
    },
    4: { // Thursday
        active: record.getCellValue('Thursday Active'),
        startTime: record.getCellValue('Thursday Start Time'),
        endTime: record.getCellValue('Thursday End Time')
    },
    5: { // Friday
        active: record.getCellValue('Friday Active'),
        startTime: record.getCellValue('Friday Start Time'),
        endTime: record.getCellValue('Friday End Time')
    },
    6: { // Saturday
        active: record.getCellValue('Saturday Active'),
        startTime: record.getCellValue('Saturday Start Time'),
        endTime: record.getCellValue('Saturday End Time')
    }
};

// Time slots to create
const timeSlots = [];

// Loop through each day from today until the end date
for (let date = new Date(currentDate); date <= endDate; date.setDate(date.getDate() + 1)) {
    const dayOfWeek = date.getDay();
    const dayConfig = daysConfig[dayOfWeek];
    
    // Skip if this day is not active or missing time settings
    if (!dayConfig.active || !dayConfig.startTime || !dayConfig.endTime) {
        continue;
    }
    
    // Parse start and end times
    const [startHour, startMinute] = dayConfig.startTime.split(':').map(Number);
    const [endHour, endMinute] = dayConfig.endTime.split(':').map(Number);
    
    // Create DateTime objects for this day's start and end times
    const startDateTime = new Date(date);
    startDateTime.setHours(startHour, startMinute, 0, 0);
    
    const endDateTime = new Date(date);
    endDateTime.setHours(endHour, endMinute, 0, 0);
    
    // Generate time slots for this day
    for (let slotTime = new Date(startDateTime); slotTime < endDateTime; slotTime.setSeconds(slotTime.getSeconds() + defaultDuration)) {
        timeSlots.push({
            fields: {
                'Time Slots': slotTime.toISOString(),
                'Dration⏳': defaultDuration
            }
        });
    }
}

// Create records in batches of 50 (Airtable limit)
const batchSize = 50;
let createdCount = 0;
for (let i = 0; i < timeSlots.length; i += batchSize) {
    const batch = timeSlots.slice(i, i + batchSize);
    const result = await meetingsAvailabilityTable.createRecordsAsync(batch);
    createdCount += result.length;
    logOutput(`Created batch ${Math.floor(i/batchSize) + 1} of ${Math.ceil(timeSlots.length/batchSize)} (${createdCount}/${timeSlots.length} slots)`);
}

// Reset the trigger field
await businessResearchTable.updateRecordAsync(recordId, {
    'Generate Time Slots': false
});

const resultMessage = `Created ${timeSlots.length} meeting availability slots from ${currentDate.toLocaleDateString()} to ${endDate.toLocaleDateString()}.`;
logOutput(resultMessage);



פותח ע"י יאיר חפץ עבור נקסאברס
