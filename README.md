# Cloud Jenkins Lab & FestPass CI Assessment

[![Java](https://img.shields.io/badge/Java-21%20%7C%2027-orange)](https://adoptium.net/)
[![Jenkins](https://img.shields.io/badge/Jenkins-2.580.x%20LTS-blue)](https://www.jenkins.io/)
[![Pipeline](https://img.shields.io/badge/Pipeline-Declarative%20SCM-brightgreen)](https://www.jenkins.io/doc/book/pipeline/)
[![Tests](https://img.shields.io/badge/Tests-28%20Passed-success)](https://junit.org/)
[![Academic](https://img.shields.io/badge/Academic-Assignment%20Submission-informational)]()

This repository contains the complete implementation and verified build evidence for the **Cloud Automation & Jenkins (CSE4141)** lab assessment.

The project implements an automated Continuous Integration pipeline using a Jenkins Declarative Pipeline with SCM polling, dedicated build agent execution, strict quality gating, and diagnostic build evidence recording for **FestPass**, an event registration and ticketing application.

---

## Repository Structure

```text
CloudJenkinsLab/
├── README.md                          # Main repository overview and guide
├── .gitignore                         # Strict ignore rules protecting runtime and secrets
├── festpass-java/                     # Application source, tests, and CI definitions
│   ├── Jenkinsfile                    # Declarative Pipeline with SCM polling & artifact gating
│   ├── submission.properties          # Class code and student challenge configuration
│   ├── src/main/java/                 # FestPass application and booking logic
│   └── src/test/java/                 # 28 JUnit test cases
├── jenkins-lab/                       # Automation scripts for local controller, Git server & agent
│   ├── start-git-server.sh            # Git daemon publishing festpass-java on port 9418
│   ├── start-jenkins.sh               # Local Jenkins LTS 2.580.1 controller
│   └── start-agent.sh                 # Dedicated student-laptop inbound agent launcher
└── submission/                        # Complete assessment deliverables and verified evidence
    ├── SUBMISSION.md                  # Detailed submission report and rubric alignment
    ├── jar-run-evidence.txt           # Executable test and verification run output
    ├── build-red/                     # Evidence from failing build (defect present)
    │   └── submission-evidence.csv    # Initial build failure diagnostics
    └── build-green/                   # Evidence from successful build (repaired)
        ├── submission-evidence.csv    # Final build success diagnostics (28/28 passed)
        ├── festpass.jar               # Gated runnable JAR package
        ├── smoke-test.txt             # Real-JAR 7-command smoke test verification
        └── build-info.txt             # Build metadata
```

---

## Lab Objectives & Quality Gating

1. **SCM Polling Trigger**: Automatically polls the local Git repository (`git://127.0.0.1:9418/festpass-java`) once per minute via `pollSCM('* * * * *')`.
2. **Quality Gating**: Failed test runs skip packaging and publishing stages, but record diagnostic evidence.
3. **Defect Diagnosis & Repair**: Identified that `BookingService.java` failed to remove students on cancellation (`cancellationFreesSeat`). Repaired with minimal code changes without altering any of the 28 tests.
4. **Automated Green Build**: Upon committing the fix, Jenkins detected changes via SCM polling and achieved a clean build (28 tests passing, 0 failures), generating and verifying `festpass.jar`.

For full build traces, commit IDs, and diagnostic tables, see [**`submission/SUBMISSION.md`**](submission/SUBMISSION.md).
