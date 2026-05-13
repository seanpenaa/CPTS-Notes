scripted nmap scan
```
nmap -Pn -sV -p 3306 --script="banner,(mysql* or ssl*) and not (brute or broadcast or dos or external or fuzzer)" -oA "${TARGET}_mysql_nmap" $TARGET
```

mysql command
```
mysql -u root -p<password> -h $TARGET #NO SPACE BETWEEN -p AND ACTUAL PASSWORD
mysql -h 10.129.2.99 -u robin -probin --ssl-mode=DISABLED
mysql -h $TARGET -u robin -p --ssl=0 robin
```

| Command | Desctiption |
|---------|-------------|
|    mysql -u <user> -p<password> -h <IP address>     |      Connect to the MySQL server. There should not be a space between the '-p' flag, and the password.       |
|    show databases;     |      Show all databases.       |
|    use <database>;     |      Select one of the existing databases.       |
|    show tables;     |      Show all available tables in the selected database.       |
|    show columns from <table>;     |       Show all columns in the selected table.      |
|    select * from <table>;     |      Show everything in the desired table.       |
|    select * from <table> where <column> = "<string>";     |      Search for needed string in the desired table.       |

