
This are fundementals, that help coders secure their code,
If you do not code using this fundementals then your code will be easier too hack.

1. TMI: Too Much Information, Do not expose, any info, that can be used for a hacker too understand how too hack ealisy.
2. CORS: if you want the server too work, only for one front, then you have too define it too the Specific Domain. specific front.
3. Logs- Always log errors, and general traffic, This can be done in the catch all.
4. .env- senetive data is saved in .env, and you put this file in gitignore.
5. Auth - for a rest Api, that we use Auth, USE JWT. like we learned in class.
6. Crash Errors: Do not return messages of crashes, too the client end in production,
You can send 4xx errors, Do not send in production any 5XX errors.
In development, 5XX errors are good.
	, You do not show that there are 5xx errors, errors reveal a bunch of info.

7. Every DB has too be protected by a password
8. never return passwords too the front, use jwt token and set the password too undefiend
9. Role in Register, when you do a regsister, You do give the possiblity too add role.id, REMOVE role .id in the get SQL quries. in the user -service.
10.  Never Save Plain Test Passwords in DB. no one Needs too see the users password. Passwords, are not plain text in the DB. You do not chipen the password.,
You hash passwords:

Hash it is an action that when you disorgenize the string.

So what happens when user wants too get in, he inputs his pass, it gets hashed then gets equated, in the DB with all the other hashed passwords. Then if the hash equated too some hash in the DB , then the that is how the user can log in.

hash of abcd will always be abcd, Meaning if i type abcd in a hundred years  abcd will still be the same hash.
abcd : "asdiaojsdouiahu12h3123a987(*&#(@!*&391987))"
abcd in 100 years: "asdiaojsdouiahu12h3123a987(*&#(@!*&391987))"

There are several algorithems too create hashes.
MD5, SHA1, SHA256, SHA512.

11. Rainbow Table:
It is a table, that has millions, of passwords, of hashed passwords., and it saved in a big db.

A. require the customer too create a strong password.
min 8 chars.
and 3 different char groups (big letters, small letter, signs and and more.)

B. adding a special string, that is added ontop on the password the user gave us, and then the salt, is added too The string, and only the SALT GETS hashed.

for example:

adding "4T72w", too every passwords:
if use gave "abcd",

Then the server will add salt on it "4aTb7c2dw";
and this string gets hashed!!!!

12. YOU HAVE TOO PROTECT YOUR CODE FROM SQL INJECTION
    use values and execute the dal using this feater.





13. IDOR , insecure direct object reference. this is where hackers try, too modify the params, too acces restricted pages.
    or a file selecting, that lets hackker too inject a file too the DB system.
	a file that you can push too the system, and it runs some kind of script. within the DB,

for numbered params, its better too show UUID in the params, instead of the actuall id that is in the Database. UUID is saved in the DB the DB maneges both int numbers and the uuid.

14. DoS attack:
Denier of service, overloading the server, with a bunch of requests, too basically too make the server too crash. , it is a scipt that runs in a loop, that all it does, is running milion of times too sending requests. Simple install npm install express-rate-limit, You use this package in the security middle were too prevent attacks. like this.

15. Disributed Denial Of service:
This is when you, request, from a bunch of different ip's at the same time using the same Dos attack but in a bunch of IP's.

usually hackers use ip spoofing. for every request you send a spoofed Ip addres, then what happens is that the server sends back the info for a non existent ip addres. And this overloads the server. so the rate limit, Prevention doesn't work.

The solution: is too use a net infustructure, that prevents it. For example cloud flair, google cloud or whatever, A bunch of big servers have this solutions.


16. XSS:
    Cross Site Scipting.
	 If a web page, gets html it shows it, If html has, a <p> it is a <p> , if the html has script, Then it runs, imideatly, And the code that is whithin it runs.
	In frontEnd like react, the framework, prevents scripts injecting, running, in Native, scripts work.
	If i feed an item name like this 
	<script>alert("Hi")<script/> Then this script would actually work.  Too prevent this attack npm install striptags




