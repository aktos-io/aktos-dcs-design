# Authentication

### Login

1. Send `username` + `password` to server, like so: 

    ```ls 
    TEMPLATE <<<< {
        auth: 
            username: 'your-username'
            password: 'your-password'
    }
    ```
    or 
    ```ls 
    TEMPLATE <<<< {
        auth: 
            token: 'your-existing-token'
    }
    ```
2. Check if user exists in the database. 
2. If exists, generate `a-random-token` in the server 
3. Add the following object to the `session-cache` store:
    ```ls
    session-cache[a-random-token] = {
      user: the-user-id 
      date: some-timestamp
    }
    ``` 
    
4. Send the token to the client: 
    ```ls 
    TEMPLATE <<<< {
        auth: 
            token: 'a-random-token'
            user: the-user-id 
    }
    ```

### Logout 

Send following message to logout: 

```ls 
TEMPLATE <<<< {
    auth: logout: yes 
}
```

Server responds with the following message: 

```ls 
TEMPLATE <<<< {
    auth: logout: 'ok' 
}
```


### Authenticating messages 

1. Any messages should be dropped if it doesn't exist in the temporary authentication table. 

### Authorization 

1. At the destination, all messages will be checked if sender is authorized to perform that action. 
2. If unauthorized, send an exception message back. 


# Security Design 

1. An attacker might get whole password database. 
2. Attacker might (and probably will) know the hash algorithm we are using. 
3. With these informations, he/she shouldn't be able to 
   1. Retrieve original password.
   2. Retrieve any sensitive user data
