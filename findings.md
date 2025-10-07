# Accountability Database Findings

## File Overview
- File: `Accountability Database - Blank.accdb` (14,544,896 bytes)
- Format: Microsoft Access ACCDB (reported by `mdb-ver` as `ACE17`)
- Encoding: UTF-8; database appears to target an accountability/rostering workflow for training squadrons.

## Table Row Counts
| Table | Rows | Notes |
| --- | --- | --- |
| `tblCheckIns` | 0 | Empty staging table for individual check-ins (student/time/phase). |
| `tblColumnHeadings` | 11 | Stores UI column labels (e.g., "STUDENT", "RANK", "MTL NAME"). |
| `tblCurfews` | 144 | Curfew schedule by squadron (`334TRS`–`338TRS`), phase, shift (`AFI`, `IP`, `R-Shift`, `S-Shift`, `T-Shift`), and duty day type. |
| `tblDataPullSettings` | 5 | Config such as `cutoff_hour=4`, `default_phase=Phase I`, and `database_program_location=C:\MTO DB`. |
| `tblExcel_Accountability_Report` | 0 | Placeholder export table mirroring weekly tracker layout. |
| `tblHolidays` | 0 | Expected to hold holiday dates/names. |
| `tblLastWeekend` | 0 | Tracks last-weekend status per student. |
| `tblLastWeekendDates` | 0 | Stores the calendar labels used for last-weekend reporting. |
| `tblMTLs` | 6 | Master Training Leader roster (IDs 2–12) with names; includes mixed casing (e.g., "MOBLEYTP"). |
| `tblMiscSettings` | 1 | Single setting `currentPW=password` (plaintext default password). |
| `tblNumbers` | 51 | Utility number table containing integers 0–50. |
| `tblPermVals` | 3 | Tracks `currentTempID=1611` and pre-set weekend/weekly date ranges (strings dated July/August 2025). |
| `tblPhases` | 0 | Empty primary roster table for current students. |
| `tblPhasesTemp` | 0 | Empty staging table mirroring `tblPhases`. |
| `tblRemedialCheckIns` | 0 | Empty log of remedial check-ins. |
| `tblRemedialRoster` | 0 | Empty remedial roster (note: `DateTimeAdded` has Access type `Unknown_0014`, likely Extended Date/Time). |
| `tblRemovedStudents` | 0 | Empty archive of removed students. |
| `tblRequiredCheckInTimes` | 8 | Required check-in times (weekday/weekend) encoded as `12/30/99 HH:MM:59`. |
| `tblShowFormOptions` | 7 | Controls which buttons appear on named subforms (only `subfrmTabbedReports` enables reports, `subfrmCheckedIn` enables check-in). |
| `tblWeeklyTracker` | 0 | Weekly schedule by student. |
| `tblWeeklyTrackerDates` | 0 | Label store for weekly tracker calendar headings. |

## Notable Data Highlights
- `tblCurfews` covers four squadrons (`334TRS`, `335TRS`, `336TRS`, `338TRS`), four phases plus `Remedial`, five shifts, and six duty-day combinations (`ALL/ON DAY/OFF DAY` vs `DUTY/NON DUTY`). All times are stored as 24-hour strings (e.g., `2200`, `0000`).
- `tblRequiredCheckInTimes` defines three weekday and five weekend mandatory check-in timestamps; Access stores them as the sentinel date `12/30/99` plus a time.
- `tblMiscSettings` exposes a default password value in plaintext, which is a security risk if used in production.
- `tblPermVals` indicates the template expects future-dated weekly (`10 Aug 2025 to 16 Aug 2025`) and weekend (`25 Jul 2025 to 27 Jul 2025`) report ranges.
- `tblMTLs` provides six named MTL contacts; IDs are non-sequential (skips 1 and 7–11 except 12).

## Other Database Objects
- Saved queries: 99 (`mdb-queries -1`), covering data maintenance (e.g., `qryWipeCheckIns`, `qryAppendStudents`, `qryResetTempID`), reporting, and remedial tracking workflows.
- Forms: 42 objects including dashboards (`frmRemedialDashboard`, `frmMain`, `frmEditStudent`), modal dialogs (e.g., `ProgressBar`), and multiple subforms used by the UI shell.
- Reports: 23 report definitions such as `rptWeeklyTracker*`, `rptWeekendTracker*`, and remedial status snapshots.
- Macros: 2 (`MSysDb`, `~TMPCLPMacro`).
- VBA modules: 10 (e.g., `modCACfunctions`, `modRemedialScanner`, `modStartupProperties`).

## Observations & Potential Follow-up
- Lack of primary keys or enforced relationships in the dumped schema suggests integrity relies on Access UI/macros rather than relational constraints.
- Configuration tables contain operational defaults (paths, shift names, date ranges) that should be validated before deployment to a new environment.
- The presence of numerous `qryWipe*` queries implies built-in routines to clear data; ensure appropriate safeguards before running them on a populated copy.
- `Unknown_0014` data type may require Access 2019+ or ACE 16+ to interpret correctly; exporting to other systems might need type mapping.
- Consider rotating or removing the hard-coded password in `tblMiscSettings` before distributing the database.
