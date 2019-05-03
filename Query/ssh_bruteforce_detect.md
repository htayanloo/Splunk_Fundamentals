## Honestly, ANY failed password for ROOT seems like it would suggest an issue. More than 3 in a second, worth alerting on.


 index=test  "Failed password for" 
 | bin _time span=1s 
 | rex "Failed password for (?<userid>.*?) from (?<fromIP)>[\d\.]+) port (?<port>\d+)" 
 | stats  count as trycount, values(login_IP) as login_IP, values(port) as port  by  login_name _time
 | streamstats window=2 sum(trycount) as eventcount values(port) as eventports by login_name 
 | where eventcount>=4 OR (eventcount>=3 AND userid="root")
 | table login_name,login_IP, eventports
