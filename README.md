### What is Log Rotation?

- When services like PostgreSQL or MySQL run, they write log(errors, queries, slow queries, etc.) to files. Over time, these log files can grow very large and eventually fill up your disk.

- Log rotation is the process of:

  - Archiving the current log file.
  - Starting a new empty log file for the service to keep writing to.
  - Optionally compressing the old logs (to save space).
  - Keeping logs only for a certain number of days (e.g., last 7 days).
  - This way, logs don’t grow forever and your VM doesn’t run out of space.

- On Linux, log rotation is usually handled by the tool logrotate.
  - Its main config file: /etc/logrotate.conf
  - Service-specific configs: /etc/logrotate.d/
- So for PostgreSQL and MySQL, you’ll create separate config files inside /etc/logrotate.d/.

- Each file tells logrotate which log files to rotate, how often, and how many to keep.

Step 1: Identify Log Locations

- PostgreSQL Logs: Check the PostgreSQL configuration for logging:

```
sudo -u postgres psql -c "SHOW log_directory;"
sudo -u postgres psql -c "SHOW log_filename;"
```

- MySQL Logs:Check MySQL log settings:

```
sudo grep -i "log_error" /etc/mysql/mysql.conf.d/mysqld.cnf
```

![alt text](https://github.com/Nishanc07/log-rotate/blob/main/public/logs_location.png)

Step 2: Create Logrotate Config Files

- MySQL Logrotate Config File: /etc/logrotate.d/mysql
- PostgreSQL Logrotate Config File: /etc/logrotate.d/postgresql

Step 3: Test Log Rotation
Run logrotate manually in debug mode:

```
sudo logrotate -d /etc/logrotate.d/postgresql
sudo logrotate -d /etc/logrotate.d/mysql
```

Force rotation:

```
sudo logrotate -f /etc/logrotate.d/postgresql
sudo logrotate -f /etc/logrotate.d/mysql

```

![alt text](https://github.com/Nishanc07/log-rotate/blob/main/public/poatgres%2Bmysql.png)

Step 4: Verify Logs

```
ls -lh /var/log/postgresql/
ls -lh /var/log/mysql/
```

![alt text](https://github.com/Nishanc07/log-rotate/blob/main/public/Screenshot%202025-08-22%20at%2011.47.58.png)
