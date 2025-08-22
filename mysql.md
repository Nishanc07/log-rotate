/var/log/mysql/*.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    create 640 mysql adm
    sharedscripts
    postrotate
        test -x /usr/bin/mysqladmin || exit 0
        if mysqld_pid=$(pidof mysqld); then
            kill -HUP $mysqld_pid
        fi
    endscript
}
