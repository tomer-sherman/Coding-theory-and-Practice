
Auth- Authentication and authorization.

Authentication:
The action of checking wether or not a user or something exist within the data base.

When you enter a site and you are not logged in:
You are considered as an anynamous user.
When you log in you are considered as a logged in user.

Autherization:
When you log into a site , you can have different  roles such as admin, or paid user etc..

Different kinds of user type will make you be able too acces different kinds of pages.
For example a paid user will be able too acces to paid content page.
And a Admin could acces the user Manegment section.

Now keep in mind, Its usually a 3 layer deffence, 
1.The first one is preventing from the component of being rendered or not, depending on your autherization.
2.Code within the component that checks you autherization, and if you do not have it it kicks you too the main page.
3.Server/data base side:
The server / data base that authenticates you the depending on your autherizations you will not be able too acces specific pages, that require specific data too be fetched.

So if one layer failed for some reason, then the second or third will defend.

For example:
using just the first method. You will deflect 99 precent of users, but the ones who know basic code,
Could acces different pages, using idor attack, Which you basically change the url path hoping too reach an existing page.
for example writing "https://my-site/admin" etc too try too enter a specific page.
if you did not have layer 2 a random user could have accesed your page.

JWT:
dont remember the specific meaning, 
Its just the most common way of handing out autherization , too different pages.

how it works:
1. You send a post request too a server using its register or sign in url, and sending the formated data.
2. If succesfull the server returns you back a JWT string. Its a simple string that contains all your user info.
3. Step 3, decoding it, Now this string contains a user Object only after you decode it. 
That is why it is incredibaly common too use decoders inside your code.
npm install jwt-decoder for example.
4. Using this JWT string and storing it in local storage or session storage. Gives you the ability too refresh the page, but the user will still be logged in.
5. Every time You want too acces restricted pages, This JWT sting will be your key of entry.

So how all OF THIS WORKS!!!!
The overview:

1. User registers- 
A post request is sent too the register url, if succesfull, then the server sends back this JWT string.
2. User is stored in the global state i am not sure how but it seems, it is stored as the UserModel you need too create, Not as the JWT token that you need too encrpt.
3. 





