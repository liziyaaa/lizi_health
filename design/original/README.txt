lizi Health Daily - Windows+iPhone project configuration

This package is NOT an importable .shortcut file. It describes the workflow for implementation and validation.

Workflow:
1. Compute report date (previous local calendar day).
2. Query HealthKit sleep samples in 18:00 yesterday to 12:00 today, retain start/end/stage/source.
3. Query heart-rate samples for yesterday, retain timestamp/bpm/source.
4. Query step samples for yesterday, retain start/end/count/source. Avoid double-counting sources.
5. Construct JSON with date, sleep[], heart_rate[], steps[], schema_version.
6. Save health_YYYY-MM-DD.json in iCloud Drive/Shortcuts/lizi-health.
7. Send via Apple Mail using the configured Outlook account; recipient must be chosen on-device.
8. Create a daily 09:00 personal automation on iPhone and test background permissions.

Signing and importing requires a valid Apple-compatible signed .shortcut file. Third-party signing services should not be sent health records, credentials or API keys.
