# Week 3 Assessment

## Setup
* Fork and Clone this repository
* Run `update-database` - let the instructors know if you run into any issues!

## Exercise (10 Points)
### Securing the Application (4 points)
* Use Secrets Managemnt to remove the database connection string from the application.
* So that we can see the full implmentation, include a screenshot below to show your `secrets.json` file

[< Screen shot here >](https://gyazo.com/b44b4c049f76063f225337266a9be952)

### Maintaining Current-User (6 points)

This application includes some starter code so that we could maintain a current user.  Review the Login and Logout code in the Home Controller, along with the Login and Logout buttons in the Home/Index.cshtml file.  Building on this pre-existing structure:
* Create a cookie that holds a key for "currentUser" when a user logs in.
* Delete that cookie when a user logs out.
* Add the value of "current user" to all views

## Questions (10 Points)

For the following questions, answer as if you are in an interview!
1. Describe one strategy you have used to maintain state in a web application. (2 points)
   * One of my most recent strategies was using Cookies to maintain state, for instance in the program I was writing I needed to display the currently signed in user on all view pages in an MVC application. After grounding myself in the codebase I located the LogIn() and LogOut() actions, and inside those is where I appended a cookie, and deleted it. I knew that when a user logged in I needed to grab their screen name, and when a user logs out - I no longer need it so I can delete it. Since I needed to display it, I used Request.Cookies to call the cookie from all actions that returns a view and assigned its value to a ViewData. Now, when a user logs in, I grab their screen name and pass it to all views in the application, the last thing was to implement the ViewData in my views. After creating a local variable to hold the screen name, I was able to display it in an appropriate space, and upon log out the screen name would no longer display.

3. How would you protect sensitive data that we need to store in our database - for example, passwords? (2 points)
   * Thats a great question, one method that I have been using is encrypting, or digesting the string through SHA 256. This method will take in a string with an arbitrary amount of characters and return an encrypted string with a set number of characters, often in my case 32. With 32 characters even if someone were able to gain access to my database it would be virtually impossible for them to decrypt the password. Let's say we are using a program that has shoppers, and merchants, the shoppers need accounts so they can have their own cart, so we can start there. As soon as a shopper is created, before saving to the database the password (and other sensitive data) is ran through the encryption process. This process takes in the password, and turns it into an array of bytes, as our next piece which is the encryption itself needs a value of an array of bytes. After our password is encrypted we need to turn it back into a string so it can be saved in the database, since the raw password is never saved; there is no trace of it other than the encrypted password, the data is safe.
     
5. Describe 3 additional common security risks in web development. (make sure you discuss what you know about the risk, and if you know how to minimize the risk) (6 points)
   * One very common, and very dangerous security risk is SQLi or SQL Injection, in short this is when an attacker inputs raw SQL into an input field that gets ran in the database. For instance, if there was a table of users and the webiste I was on was insecure, I could enter something like *User Id:* myUserId OR 1=1. If this were sent directly to the database, it could be ran as: SELECT * FROM users WHERE userid = 52 OR 1=1. Since 1=1 is always true, this will return ALL users with ALL information. This vulnurability is very lethal, and has been the cause of countless major cyber attacks. An easy way to limit or eliminate this risk is by sanitizing the input before sending it to the database, for instance if I had a method to check a string (input) for characters such as =, '', or keywords such as, OR, AND, WHERE; I could stop that input from being sent, thus stopping the SQL injection before any threat arose.
   * Another common security risk is not limiting API requests. This can affect many things but one of the most common is affecting the speed of your program for all users. So if I had an API that was open-source with unlimited API requests, this opens me up to DDoS attacks, extreme maintenence costs for the server, and poor speeds for users. A common solution to this is to use API keys, to limit who has access to your API, but if you wanted to keep it open-source you can deploy other methods such as request queues, or limiting the amount of requests a user can make in x amount of time.
   * One other example of security threats is XSS, or Cross-Site Scripting. This occurs when an attacker stores a script on a website, either in the database, or a profile and many other places, so that when a user triggers the script it gives the attacker access to that user's session - this includes cookies, location, and everything stored client-side. Not only can the attacker take information, but they can also take control and use the website as if they are the user. So now the attacker has taken the sensitive information from the user's cookies, and can log in using those creditials to do damage on the user's behalf. A common way to fix this, just like for SQL Injection is to sanatize all user inputs so that the scripts can't be stored in places like comments, user profiles, or anywhere else a user can input. Another posibility is to encrypt all cookie data - this would mean that even if the attacker got control of the cookies, they would have no idea what they did or what information they held.
