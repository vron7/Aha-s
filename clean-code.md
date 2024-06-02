### Avoid unneccesary nesting
```js
function proccessPlayer(player) {
    if (player != null) {
        if (player.hasSubscription) {
            if (player.age >= 18) {
                showFullVersion();
            } else {
                showChildrenVersion();
            }
        } else {
            throw new Error("Player not subscribed");
        }
    } else {
        throw new Error("Player not found");
    }
}
```
This code above is not easy to follow, lets make adjustments.
```js
function proccessPlayer(player) {
    if (player == null) {
        // here error code goes with the if check, its not on the other side of the code
        throw new Error("Player not found"); 
    }

    if (!player.hasSubscription) {
        // same as above
        throw new Error("Player not subscribed");
    }

    if (player.age < 18) {
        // we can use return here, so it will not pass
        return showChildrenVersion();
    }
 
    showFullVersion();       
}
```
---
### Avoid ambiguity
```js
const MIN_PASSWORD = 6;

function checkPasswordLength(password) {
    return password.length >= MIN_PASSWORD;
}
```
Problems?
- constant name is not clear enough for what it should represent
- method name is not clear enough, what it does? what it returns? does it throw error? does it return boolean? what returned boolean means?

Let's make it better:
```js
// renamed the constant to be more clear what it represents
const MIN_PASSWORD_LENGTH = 6;

// renamed funtion to indicate that it returns a boolean value by adding *is* as a prefix
// now it's clear what it means when it returns true (password is long enough) and false (password is not long enough)
function isPasswordLongEnough(password) {
    return password.length >= MIN_PASSWORD_LENGTH;
}
```
---

