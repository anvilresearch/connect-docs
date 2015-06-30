<!-- WIP Guide to Installing on Modulus - Broken on Modulus's end right now. -->

1. Go to Modulus
2. Sign in
3. New project
3.1. Input Project Name
3.2. Node Project
3.3. 193mb
3.4. Default server
4. Create a new Anvil Connect project
4.1. npm install -g anvil-connect locally
4.2. nv init
5. npm install -g modulus
5.1. modulus login
// Other steps that are important
6. Navigate to Administration tab !important
6.1. Enable SSL Redirect
8 Go to redislabs.com
7.1. Create account or Login
7.2. New Redis Subscription
7.2.1. 30MB Free
7.2.2 Input Information
7.2.3 Create a password and REMEMBER IT: c0nn3ct
7.3 Add redis endpoint to production.json
8. modulus deploy
8.1. Select connect project
9. Run `NODE_ENV=production nv migrate` against local repo
