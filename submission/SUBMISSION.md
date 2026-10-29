# Jenkins CI Challenge - Student Evidence & Submission

**Student Alias (Git Author):** `myaumyauverse`  
**Class Code:** `CLASS_2026`  
**Challenge Code:** `CHALLENGE_001`  
**Pipeline Job:** `festpass-ci`  
**Agent:** `student-laptop`  

---

### 1. Build Trace & Commits

| Metric | Red Build | Green Build |
| :--- | :--- | :--- |
| **Build Number** | `#3` | `#4` |
| **Commit ID** | `7e94dfe2e87efbc68995706339568c2ccc918dd0` | `5bc899822cfd8842bbc350e3fdc1091ec6416f62` |
| **Commit Message** | `Personalize my help introduction` | `Fix booking cancellation defect` |
| **Build Cause** | Started by an SCM change (or user rebuild) | `Started by an SCM change` (automatic polling) |
| **Pipeline Result** | `FAILURE` | `SUCCESS` |
| **Test Counts** | 28 tests (25 passed, 3 failed, 0 errors, 0 skipped) | 28 tests (28 passed, 0 failed, 0 errors, 0 skipped) |
| **Skipped Stages** | `Build JAR`, `Smoke test JAR`, `Save artifact` | None (all stages executed successfully) |
| **JAR Status** | `NOT_PUBLISHED` | Published (`festpass.jar`) |
| **JAR SHA-256** | N/A | `56eac51ac3738e70dd26c9b39440c70e05c49ed6b221b6191536ff47e2e64e95` |

---

### 2. Diagnosis and Defect Repair

- **Failing Assertion:**  
  `FAIL cancellationFreesSeat: SOLD OUT. Even the developer needs a ticket.` (`FestPassTest.java:147`)  
  Additional failures: `cancellationsSurviveReload` and `cliWorkflowPersistsAcrossCommands`.
- **Violated Business Rule:**  
  When a student cancels their registration for an event, that seat must be freed so another student can book it.
- **Root Cause:**  
  In `src/main/java/BookingService.java`, the `cancel(String eventId, String studentId)` method validated that the booking existed, but failed to remove the student from the internal registration list (`event.students`).
- **Code Fix Made:**  
  Added `event.students.remove(studentId);` directly inside `cancel()` in `BookingService.java`:
  ```java
  public void cancel(String eventId, String studentId) {
      validateId(studentId);
      Registration event = requireEvent(eventId);
      if (!event.students.contains(studentId)) {
          throw new IllegalArgumentException("No booking for student: " + studentId);
      }
      event.students.remove(studentId);
  }
  ```
  All 28 tests remained untouched and passed cleanly.

---

### 3. Runnable JAR Verification

Executing `java -jar festpass.jar help`:
```text
FestPass - campus event registration (myaumyauverse)
Usage: java -jar build/festpass.jar [--data FILE] COMMAND
  add-event ID "TITLE" CAPACITY
  list
  book EVENT_ID STUDENT_ID
  cancel EVENT_ID STUDENT_ID
  roster EVENT_ID
  summary
  help
Default data file: data/festpass.tsv
IDs are case-sensitive. Use synthetic student IDs, not personal data.
Exit codes: 0 success, 2 invalid request, 3 storage failure.
```

---

### 4. Submitted Artifacts

1. `red-submission-evidence.csv` (Archived diagnostic evidence from the failing build)
2. `green-submission-evidence.csv` (Archived evidence from the successful build)
3. `festpass.jar` (Executable runnable JAR built by Jenkins Build #4)
4. `festpass-java/` (Git repository with complete 4-commit history)
