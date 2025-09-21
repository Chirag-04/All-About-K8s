## Cron Job
A CronJob is a time-based Job in Kubernetes. It runs Jobs on a scheduled basis, similar to Linux cron.
Key Points

* Purpose: Automate recurring tasks like backups, report generation, or cleanup scripts.
Uses Jobs: Each scheduled execution creates a Job, which then creates Pods.

Scheduling: Uses standard cron syntax (min hour day month weekday) for timing.

Concurrency Policy:

Allow → Run even if previous job is still running

Forbid → Skip next run if previous job is still running

Replace → Cancel the previous job and start new one
