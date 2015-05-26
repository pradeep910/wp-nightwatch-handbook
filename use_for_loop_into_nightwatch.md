# Use For loop into nightwatch

When you have multiple data (i.e Json format) and want to test by iterating it then you can use `for` loop for that.

You can not perform assertions directly into `for` loop.

For that you can use custom commands to perform your assertions. And custom commands called whitin `for` loop and passing approprite data.

Let's see by one example.

I have user details in json format. and want to test it by iterating it.

**Steps 1: Define global varibale for user details in before function. ** 

```
module.exports = {
    before: function(client) {
        client.globals.user_info = {"1":{"name":"User 1", "email":"user1@info.com"}, "2":{"name":"User 2", "email":"user2@info.com"}}
    },
```

**Steps 2: Create test case for testing user details. **

```
    '1. Check User Details' : function (client) {
        client.maximizeWindow()
        url( 'http://testurl.com/user_details' )
    	.waitForElementVisible( 'body', 1000 )
    	.click('#user_tab', function() {
            for( user_id in client.globals.user_info ) {
                client.checkUserInfo(client.globals.user_info, user_id); /* Call our custom command. */
            }
        })
        .end();
    }
} /* End of module.exports */
```

**Steps 3: Create custom command to test user details. **

**Link:** http://nightwatchjs.org/guide#writing-custom-commands

```
File name: checkUserInfo.js (Note: The command name is the name of the file itself)

exports.command = function(users, user_id, callback) {
    var self = this;
    /* Perform your assertions here. */
    return this;
 };
```