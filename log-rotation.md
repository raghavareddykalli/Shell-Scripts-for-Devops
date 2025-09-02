# Shell script that compresses logs older than 7 days and deletes logs older than 30 days. Also, run it daily via cron.

```
#!/bin/bash

# Directory where logs are stored
LOG_DIR="/var/log/myapp"
LOG_FILE="/var/log/myapp/log_rotation.log"

# Ensure the log directory exists
if [ ! -d "$LOG_DIR" ]; then
    echo "[$(date)] ERROR: Log directory $LOG_DIR does not exist!" >> "$LOG_FILE"
    exit 1
fi

# Compress logs older than 7 days (but newer than 30)
find "$LOG_DIR" -type f -name "*.log" -mtime +7 -mtime -30 ! -name "*.gz" -exec gzip {} \; -exec echo "[$(date)] Compressed: {}" >> "$LOG_FILE" \;

# Delete compressed logs older than 30 days
find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -exec rm -f {} \; -exec echo "[$(date)] Deleted: {}" >> "$LOG_FILE" \;

# Optional: Delete uncompressed logs older than 30 days
find "$LOG_DIR" -type f -name "*.log" -mtime +30 -exec rm -f {} \; -exec echo "[$(date)] Deleted (uncompressed): {}" >> "$LOG_FILE" \;

# Done
echo "[$(date)] Log rotation completed successfully." >> "$LOG_FILE"
```

## Detailed Explanation

1. **Shebang and Variables**

```
#!/bin/bash
LOG_DIR="/var/log/myapp"
LOG_FILE="/var/log/myapp/log_rotation.log"
```

- The script uses Bash.

- LOG_DIR is the directory where your application logs are stored.

- LOG_FILE is the file where this script writes its own activity logs.

2. **Check if Log Directory Exists**

```
if [ ! -d "$LOG_DIR" ]; then
    echo "[$(date)] ERROR: Log directory $LOG_DIR does not exist!" >> "$LOG_FILE"
    exit 1
fi
```

- If the log directory does not exist, it logs an error and exits.

3. **Compress Old Logs**

```
find "$LOG_DIR" -type f -name "*.log" -mtime +7 -mtime -30 ! -name "*.gz" -exec gzip {} \; -exec echo "[$(date)] Compressed: {}" >> "$LOG_FILE" \;
```

- Finds .log files older than 7 days but newer than 30 days (not already compressed).

- Compresses them with gzip.

- Logs each compression action.

4. **Delete Old Compressed Logs**

```
find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -exec rm -f {} \; -exec echo "[$(date)] Deleted: {}" >> "$LOG_FILE" \;
```

- Finds compressed log files (.gz) older than 30 days.

- Deletes them and logs the deletion.

5. **Delete Old Uncompressed Logs**

```
find "$LOG_DIR" -type f -name "*.log" -mtime +30 -exec rm -f {} \; -exec echo "[$(date)] Deleted (uncompressed): {}" >> "$LOG_FILE" \;
```

- Finds uncompressed .log files older than 30 days.

- Deletes them and logs the deletion.

6. **Completion Message**

```
echo "[$(date)] Log rotation completed successfully." >> "$LOG_FILE"
```

- Logs that the script finished successfully.

## Summary:

This script manages log files by compressing old logs, deleting very old compressed and uncompressed logs, and logging all its actions for auditing.




