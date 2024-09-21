# Code Duplication
---

Original Program:

```go
func getUser(response, request) {
  userID = request.url["user"]

  cacheMu.Lock()
  user, found = cache[userID]
  cacheMu.Unlock()

  if !found {
    query = ...
    userDb = db.ExecQuery(query)["user"]

    cacheMu.Lock()
    cache[userID] = userDb
    cacheMu.Unlock()

    user = userDb
  }

  response.encode(user)
}

func getUsers(response, request) {
  userIDs = request.UserIDs
  var users

  for userID in userIDs {
    cacheMu.Lock()
    user, found = cache[userID]
    cacheMu.Unlock()

    if !found {
      query = ...
      userDb = db.ExecQuery(query)["user"]
  
      cacheMu.Lock()
      cache[userID] = userDb
      cacheMu.Unlock()
  
      user = userDb
    }

    users.append(user)
  }

  response.encode(users)
}
```

If the developer needs to modify / remove the cache lock logic, they will have to update all the blocks manually.

There is a chance they might miss few spots / introduce bugs.

This is because of existence of duplicte code.

Updated Program:

```go
func getSingleUser(userID) {
  cacheMu.Lock()
  user, found = cache[userID]
  cacheMu.Unlock()

  if !found {
    query = ...
    userDb = db.ExecQuery(query)["user"]

    cacheMu.Lock()
    cache[userID] = userDb
    cacheMu.Unlock()

    user = userDb
  }

  return user
}

func getUser(response, request) {
  userID = request.url["user"]
  user = getSingleUser(userID)
  response.encode(user)
}

func getUsers(response, request) {
  userIDs = request.UserIDs
  for userID in userIDs {
    user = getSingleUser(userID)
    users.append(user)
  }
  response.encode(users)
}
```

Now the developer has to update the logic in a single place - `getSingleUser(userID)`

Now the code is less convoluted and more readable
