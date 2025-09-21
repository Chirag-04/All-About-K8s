## Cron Job
A CronJob is a time-based Job in Kubernetes. It runs Jobs on a scheduled basis, similar to Linux cron.
Key Points

* Purpose: Automate recurring tasks like backups, report generation, or cleanup scripts.
* Uses Jobs: Each scheduled execution creates a Job, which then creates Pods.
* Scheduling: Uses standard cron syntax (min hour day month weekday) for timing.
* Concurrency Policy:
  * Allow → Run even if previous job is still running
  * Forbid → Skip next run if previous job is still running
  * Replace → Cancel the previous job and start new one

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello-cron
spec:
  schedule: "*/1 * * * *"
  concurrencyPolicy: Forbid # skip next one if previous one is still running
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            command: ["sh","-c","echo hello from cron!"]
          restartPolicy: OnFailure

```

<img width="801" height="559" alt="Screenshot 2025-09-21 at 7 28 44 PM" src="https://github.com/user-attachments/assets/c6b39646-41b6-45ad-96e8-a95ab45a79da" />
