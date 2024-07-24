## Filter Nginx log to find uniq IP addresses 

Below command is for find uniq IP address in Nginx `access.log` in last 2 to 4 hours.
~~~~bash
sudo awk -vDate=`date -d'now-4 hours' +[%d/%b/%Y:%H:%M:%S` -vDate2=`date -d'now-2 hours' +[%d/%b/%Y:%H:%M:%S` '$5 > Date && $5 < Date2 {print Date,Date2,$1}' access_log | sort | uniq -c | wc -l
~~~~