# Employee Time Tracking System

A C# console application for tracking employee clock-in and clock-out times, validating entries, and generating weekly reports. Built for PRG271 to demonstrate **object-oriented design, interfaces, custom exceptions and multithreading**.

---

## Features

- **Admin login** before any data can be accessed
- **Clock in / clock out** for registered employees
- **Full-time and part-time employees** handled through a shared base class
- **Time entry validation**, with malformed or missing entries raising specific exceptions
- **Weekly reports** written out to `WeeklyReport.txt`
- **Automatic background reporting** on its own thread, without blocking the console
- **Directory watching** — a background task monitors a log folder and picks up new entries as they appear

---

## Design

The project is built around interfaces and inheritance rather than one large class:

```
User                    base class
 └─ Employee
     ├─ FullTimeEmployee
     └─ PartTimeEmployee

Admin                   login and employee registration
TimeLog                 a single clock-in/clock-out record
TimeLogManager          holds and queries the log entries
ReportGenerator       : IReportGenerator
FileChecker             watches the log directory on a background task
AutoReportThread        periodic report generation on its own thread
```

**Interfaces**

| Interface | Purpose |
| --- | --- |
| `IReportGenerator` | Any class that can produce a report, so the reporting strategy can be swapped |
| `ITimeValidator` | Any class that can validate a time entry |

**Custom exceptions**

| Exception | Raised when |
| --- | --- |
| `InvalidTimeFormatException` | A time entry cannot be parsed |
| `MissingTimeEntryException` | A clock-out has no matching clock-in |

Both derive from `Exception`, so callers can catch the specific failure rather than a generic error.

**Threading**

`FileChecker.WatchDirectory()` and `AutoReportThread` each run on their own task via `Task.Run`, so the console stays responsive while the application watches for new log files and regenerates reports in the background.

---

## Running it

Requires Visual Studio and .NET Framework 4.7.2.

1. Open `Project.sln`
2. Build and run
3. Log in with the demo credentials below
4. Clock employees in and out from the menu
5. `WeeklyReport.txt` is written to the application's working directory

**Demo login:** `admin` / `admin123` — hardcoded for demonstration only; a real system would authenticate against a store with hashed passwords.

**Note:** the log directory path is currently hardcoded to `C:\Logs`. Change it in `Program.cs` if that folder does not exist on your machine.

---

## Built with

C# · .NET Framework 4.7.2 · console application · file-based storage · `System.Threading` and `Task`

---

## Author

**Hanré Koen** — [@Hanrekoen](https://github.com/Hanrekoen)
