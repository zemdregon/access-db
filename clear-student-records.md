# Clearing Student Records for a Fresh Import

This guide walks end users through removing existing student data from the Accountability Database (`Accountability Database - Blank.accdb`) so a new roster can be imported without conflicts. The steps assume you are working in Microsoft Access on Windows.

## 1. Before You Start
- **Notify others**: Make sure no one else has the database open while you clear data.
- **Back up the file**: Copy `Accountability Database - Blank.accdb` to a safe location before making changes.
- **Know your import files**: Have the new student roster and check-in files ready so you can import immediately after clearing the old records.
- **Enable macros/content**: When Access opens, choose *Enable Content* so all forms and queries remain available.

## 2. Tables Involved
Only clear data from the student-facing tables listed below. Leave configuration tables untouched so dropdown lists, passwords, and settings stay intact.

| Table name | Holds… | Action |
| --- | --- | --- |
| `tblWeeklyTracker` | Weekly accountability entries per student | Clear all rows |
| `tblWeeklyTrackerDates` | The date labels tied to `tblWeeklyTracker` | Clear all rows |
| `tblExcel_Accountability_Report` | Export-ready weekly accountability view | Clear all rows |
| `tblLastWeekend` | Last weekend accountability entries | Clear all rows |
| `tblLastWeekendDates` | Date labels for `tblLastWeekend` | Clear all rows |
| `tblRemedialRoster` | Students on remedial status | Clear all rows |
| `tblRemedialCheckIns` | Timestamped remedial check-ins | Clear all rows |
| `tblRemovedStudents` | Students recently removed from the roster | Clear all rows |

> Do **not** modify tables such as `tblColumnHeadings`, `tblDataPullSettings`, `tblHolidays`, `tblMiscSettings`, `tblPermVals`, `tblRequiredCheckInTimes`, `tblShowFormOptions`, `tblMTLs`, or `tblNumbers`. These store permanent configuration that should remain in place between imports.

## 3. Clearing the Records
1. Open the database in Access and close any forms that open automatically.
2. In the left navigation pane, switch to **Tables**.
3. For each table listed in Section 2:
   1. Double-click the table to open it in Datasheet View.
   2. Press `Ctrl+A` to highlight all rows (or use the row selector in the upper-left corner).
   3. Press the `Delete` key, confirm the delete prompt, and wait until all rows are removed.
   4. Close the table. If Access asks to save layout changes, choose **No**.
4. After finishing the list, press `F5` or close/reopen one of the tables to confirm it now contains zero records.

## 4. Reset Counters and Date Ranges (Optional but Recommended)
If your import process overwrites these automatically, you can skip this section. Otherwise, reset them manually so the interface shows blank values until the new data is loaded.

1. Open `tblPermVals`.
2. Locate the records where `permType` is:
   - `currentTempID` → set `permLongVal` to `0`.
   - `currentWeeklyDateRange` → clear `permStringVal` (leave it blank) or update it to match the upcoming week.
   - `currentWeekendDateRange` → clear `permStringVal` (leave it blank) or update it to the upcoming weekend.
3. Close the table when finished.

## 5. Verify Before Importing
- Use the *Navigation Pane Search* box (top of the table list) to quickly reopen each cleared table and confirm the status bar reads **0 records**.
- If any rows remain, repeat the delete steps for that table.
- From the **Database Tools** tab, select **Compact and Repair Database**. This keeps the file size small and removes any orphaned records.

Once everything shows zero records, proceed with your normal import process (for example, running the `Import Student Data` macro or using the import form provided in your workflow).

## 6. Troubleshooting
- **Delete prompts keep appearing**: Ensure the table is in Datasheet View (not in design) and that *Record Locks* are not enabled under *File ▸ Options ▸ Client Settings*.
- **Access says the record is in use**: Close any open forms, queries, or reports that might be referencing the table, then retry.
- **Tables instantly repopulate**: You may have auto-execution macros. Close the database without saving, reopen, hold `Shift` during launch to bypass auto macros, then repeat the clear-out steps.
- **Accidentally removed a configuration table**: Close without saving, restore your backup copy, and restart.

## 7. Next Steps
With the database cleared, follow your standard import routine immediately so daily operations are not disrupted. After importing, verify that new student IDs, weekly date ranges, and accountability entries appear correctly in the associated forms and reports.
