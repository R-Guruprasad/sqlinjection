![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/6a3fd893-3161-4baa-9965-2eea37635770)# sqlinjection
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

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/1a2dc5cd-b561-4e03-8efc-166c51cc0241)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/46af73f6-9e6c-47ee-9767-11977fd25c2d)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/5d9651b4-8dcc-4774-930d-cc4dccc78985)

Click on the menu Login/Register and register for an account

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/2b493e6c-ac60-4647-88ce-7a1ece9e3835)

Click on the menu Login/Register and register for an account

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/a65e4eb7-d4f0-4744-b8cc-2155e49d9323)

Click on the link “Please register here”

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/7e942bf2-02d0-417c-820d-2a4b94752fa3)

Click on “Create Account” to display the following page:

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/d7717025-8b28-4dd6-9a78-77fa3d6a23f7)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/4db4b186-e724-46aa-b474-7a02089063ad)

Click “Login”. The logged in page will show as below:

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/d6edee3a-bdca-46e7-a42a-3557f67fcc0f)

## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/d3043947-3ed4-46bc-969b-a09438507a50)

## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/c1d306f6-957a-4813-8361-a018eaf9e276)

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/686c3dc2-28a2-456f-949b-ffce56aeed8e)

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/0d554b2e-80cb-4dd4-addf-9466f9009599)

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/3886d2b8-127b-48e5-b2d3-48040d368afa)

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/7fbe1950-a655-469d-9314-2a56cceb5fc8)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/f92f6924-28d4-42dd-836d-627aa163cb89)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/07d8e617-f5a8-4526-b83c-3b5218633fda)

After adding the order by 6 into the existing url , the following error statement will be obtained:

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/0aa54229-369d-4d23-820a-b27b48730a63)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/8262ffd4-7868-4892-b57d-e7d42c81b076)

As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/ff677c76-0fd2-4526-8e31-77df3bbde0b5)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/90efe321-e489-4067-bf9f-ca4f1f749d8b)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/985d9ecb-656d-4c84-bf6d-1b5395473c8c)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/635b9c2c-512a-40cb-82b3-07b2b7e261ff)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/579f95a4-0168-42e8-8c5e-fb62bd54a333)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/3d9e1a02-b4fd-4350-8e86-37cddf14e283)

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/1f6e5ee8-adf6-409f-9397-a563ae6ed8dc)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/566bb471-e349-4de8-8b74-b19251e83a25)

## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

![image](https://github.com/R-Guruprasad/sqlinjection/assets/119390308/238f1cfd-6c15-4844-8d7a-e336d1739e70)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
