---
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
This code is not easy to follow, lets make adjustments.

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

