# Privacy and Data Handling

- Collect, transmit, duplicate and retain only the data a feature needs; process locally
  by default.
- No telemetry, analytics, advertising or tracking, and no background network traffic,
  unless it is a documented, user-controllable feature.
- Never put personal, patient, participant or confidential research data into
  repositories, test fixtures, logs, issues or CI output; use synthetic or de-identified
  data.
- Do not upload source code, logs or user files to external services without the
  maintainer's explicit request.
- Treat user files conservatively: no overwrites or deletions without confirmation;
  back up before migrations.
- Logs and crash reports stay local and exclude document content and personal data by
  default.
