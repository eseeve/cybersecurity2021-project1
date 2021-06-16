# MOOC Cybersecurity 2021 - Project 1

## Flaw 1: Injection  

[Link](https://github.com/eseeve/cybersecurity2021-project1/blob/f2dbb028f3b69b3478820451eabdbc1283caedb1/polls/views.py#L32)  
The application contains an implementation of the detail view that uses raw SQL queries, which are vulnerable to SQL injection. In this example, the data in the query is not validated or sanitized before performing the query, which can lead to SQL injection attack.

This flaw can be simply fixed by not using raw SQL or sanitizing the data before use. One can use Django's own functions or shortcuts, which are safe from SQL injection attacks, to fetch the needed data from the database. This is the easiest way to fix most simple SQL queries, since Django's library methods will validate all input data before perfroming the SQL query in the database. However, sometimes more complex queries need to be performed and the only way to do them is to use raw SQL queries. The way to fix this issue in this case is to ensure the data entered in the SQL query is sanitized properly in the code and does not contain additional query injections. Also, the SQL queries should be made safe against possibly dangerous data. For example, string formatting and quote placeholders in SQL strings are vulnerable to injections, so they must not be used. Instead, passing the data to the query as parameters ensures SQL injection protection.

## Flaw 2: Cross-Site Scripting XSS 

[Link](https://github.com/eseeve/cybersecurity2021-project1/blob/f2dbb028f3b69b3478820451eabdbc1283caedb1/polls/templates/polls/index.html#L31)  
The application is not safe against XSS attacks. In the source code, the comment data is not validated and passed on as safe to the HTML rendering the comments. Attackers can write comments on the site that are scripts that redirect to other sites and expose cookies and tokens. This can be extremely harmful since all cookies and tokens allow attackers to steal user accounts and get access to authorized parts of the website.

To fix this flaw, any data rendered must not be deemed as safe before validation. In this case, Django can prevent the scripts from working if we omit the tag 'safe' from the comments. Django uses HTML escaping, where special characters are encoded before they are used as an input for the HMTL document. This way each comment in our application is escaped and encoded so they do not execute any scripts. 

## Flaw 3: Using Components with Known Vulnerabilities  

[Link](https://github.com/eseeve/cybersecurity2021-project1/blob/f2dbb028f3b69b3478820451eabdbc1283caedb1/project1/settings.py#L4)  
The project components can be outdated and contain vulnerabilities. This project was created with Django 3.2.3, which contains vulnerabilities, which are fixed in a later patch 3.2.4. One vulnerability is Directory Traversal, where the attacker can access the sensitive private ssh key from the URL of the server. 

To fix these problems, all the vulnerable packages need to be updated to safe versions. All the packages used by the project need to be audited for vulnerabilities and then upgraded if needed. Using the newest versions with security patches is essential for projects. Also, it is necessary to remove all unused packages from the project, since they might also be used for attacks if they contain any vulnerabilities.

## Flaw 4: Sensitive Data Exposure  

[Link](https://github.com/eseeve/cybersecurity2021-project1/blob/f2dbb028f3b69b3478820451eabdbc1283caedb1/polls/views.py#L44)  
The application polls can be set to future date. These polls with future dates cannot be seen from the index page, but they can be accessed from the URL if the user guesses the id of the poll. These polls might contain unpublished information that can be seen as sensitive, and the information is accessible to attackers. 

All sensitive information should not be accessed without authorization. This flaw could be fixed by filtering all future polls out from the queryset of the polls, so that they cannot be accessed from the URL. Also, all sensitive data needs to be encrypted. This way if the encrypted data gets leaked, if cannot be accessed without the right decrypting algorithm and key. This adds an additional layer of security for sensitive data. 

## Flaw 5: Security Misconfiguration  

[Link](https://github.com/eseeve/cybersecurity2021-project1/blob/f2dbb028f3b69b3478820451eabdbc1283caedb1/project1/settings.py#L23)  
The configuration of the Django application contains variables that need to be secret, such as the SECRET_KEY variable and database names. It should not be seen in the source code, since it can be seen by everybody and used maliciously to sign tokens and hashes by attackers. Also, the admin password and username of this site are very generic and need to be more safe. Generic password are easy to crack by writing a script that brute forces the most common passwords into the login form

This flaw can be fixed by making the SECRET_KEY and other sensitive variables environmental variables with the django-environ package. The values are added to .env file which is hidden, and contains all the secret variables. Also, the admin login credentials need to be changed into more safe ones. 

## Flaw 6: Broken Authentication  

[Link](https://github.com/eseeve/cybersecurity2021-project1/blob/f2dbb028f3b69b3478820451eabdbc1283caedb1/polls/templates/polls/index.html#L31)  
Attackers can access the cookies and tokens of the application through XSS scripting. This allows for session hijacking and submitting unauthorized data to the database. If the website contained user accounts, the passwords of the accounts could also be compromised through this flaw.

This flaw can be fixed by preventing XSS scripting and securing all tokens in the code. Also, the tokens and cookies can be encrypted to enhance the security of the authentication methods.
