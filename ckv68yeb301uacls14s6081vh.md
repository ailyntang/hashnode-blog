## Authentication and security 101

Authentication token 
* OAuth is a type of auth token
* An auth token is specific to a user, and in some instances (maybe always?) specific to a user's login. So, if a user logs out, a new auth token is required on the next login
* In the server, there is a mapping of auth tokens to users
* Best practice is that companies should set their auth tokens to expire frequently. E.g. every month. That means that if a user's auth token is compromised, it will expire anyway, so at least there is a time limit to the compromised activity

OAuth2
* When a client requests an auth token, the server also sends a refresh token which is valid for one use only
* Both the auth token and refresh token are stored on the server, and on the user's device
* If the auth token is compromised, the server can invalidate the auth token but keep the refresh token valid
* It's highly unlikely that both the auth token and the refresh token are compromised. For that to happen, the attacker must be listening to the original exchange where the server sent the refresh token. 
* The user does not need to login again with a username and password combination, unless both the auth token and refresh token are invalid. So this is a really nice user experience. This was an update with OAuth2.

Secret
* This is specific to a mobile platform and a company.
* So each app on the iOS store will have its own secret, that is stored inside the codebase as a string
* There are ways to hide this string, so that if someone gets access to the repo, they can't just search "Secret" and find the string
* On Android, it is more important to hide the string as it is much easier to reverse engineer an app on Android than on iOS
* To hide the secret, some common practices are:
  * Transform the original secret (e.g. replace certain characters)
  * Split the secret across multiple files

HTTPS
* This encrypts the actual endpoint being used

HTTPS + Auth Token + Secret = access to the end point
* So a hacker needs to know the actual endpoint, i.e. the URL used. This is protected by HTTPS
* Then a hacker needs the server to honour the request as a legitimate request. This is protected by the secret
* Then if a hacker wants user specific information, they need the user's auth token


Public tokens
* These are mapped to a user's account, however the public token contains no user information
* If someone steals the public token, there's no way they can do anything meaningful with that token alone
* A hacker would need to also break into the database and steal the mapping between the public token and the user id
* Public tokens can be used in things such as analytics. So that we can see that an issue happened to this particular user multiple times, but we can't trace which user it was



User token
* Same as a public token. It's a token that is mapped to a user's id.
* Might want to use one as a private user token, that is used to talk to other APIs internally, so pass user data around


UUID - Universally unique id
* Tokens can all be UUIDs
* Ideally you pass in a set of inputs, and then an algorithm generates a UUID which should be always be unique. Inputs are things like:
  * Current date and time (I imagine this is why it's always unique)
  * device's mac address
  * data about the silicon chip in the device
  * etc.


Hashing - seed
* When writing a random number generator, it’s usually based on an initial number that the algorithm works off
* That initial number is called the SEED
* It's because it's tough to create a truly random number. So you use one faux randomly generated seed number to help you generate another faux randomly generated number


Hashing - salt
* It's possible to use a rainbow table to figure out the hashing algorithm used
  * A table that has all the combinations, so that you can find and match the algorithm and then decrypt all the hashes
* To make it more secure, companies salt user's passwords, by adding something to the password - e.g. at the start, end or middle of the password
* Then the rainbow table doesn't work anymore
* If a hacker figures out the salt, then they have to create a rainbow table with that salt
  * It's possible to do, but it's just another layer of protection and effort for the hacker than if the passwords weren't salted

