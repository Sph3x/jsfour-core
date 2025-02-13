# [BETA] jsfour-core

### Features
* Use fetch() to fetch data directly from the server to the NUI.
* Adds a gamemaster that lets you trigger client events from the browser (http:/localhost:30120/jsfour-core/gamemaster?token=password). It's disabled by default in the config.js.
* Adds a shared folder for images, fonts, javascripts and css files that can be used by other NUIs.

### LICENSE
Please don't sell or reupload this resource. Feel free to make forks and post any updates in the original forum thread. Do not create your own thread since it's not a new release.

### INSTALLATION
* Install dependency: <a href="https://github.com/brouznouf/fivem-mysql-async">mysql-async</a>
* <a href="https://github.com/jonassvensson4/jsfour-core/releases">Download the resource</a>
* Add your server IP + port to the <a href="https://github.com/jonassvensson4/jsfour-core/blob/master/config.js">config</a>.
* Add `start jsfour-core` to your server.cfg, remember to start it before my other scripts.
* It's recommended to run the resource once and then restart the server since it's generating some modules.

### SHARED FILES
Notice: Everything in the shared folder will be publicly available, don't put any sensitive information in there.

To use a file in your NUI from the shared folder simply point the href to `nui://jsfour-core/shared/FOLDER/NAME.extension`:

### FETCH USAGE
To be able to use the fetch the client needs a session token and the endpoint that is generated by the server. You need to add the following event to your client file if you want to pass the data to your NUI.

```javascript
onNet('jsfour-core:session', ( data ) => {
    SendNuiMessage(JSON.stringify({
        action: 'token',
        token: data.token,
        endpoint: data.endpoint,
        esx: data.esx,
        steam: data.steam
    }));
});
```

You'll then get the data by using the window eventlistener.
```javascript
window.addEventListener('message', ( event ) => {
    let action = event.data.action;
    
    switch( action ) {
        case 'token':
            sessionToken = event.data.token;
            endpoint = event.data.endpoint;
            esxEnabled = event.data.esx;
            steam = event.data.steam;
            break;
    }
});
```
The NUI can now use the fetch() to grab data from the database using a correct query type. It's using predefined SQL queries found in the config.js file and in some of my other resources server.js file.


To change the query type you simple change the database/login to something else like this: 

`http://${endpoint}/jsfour-core/${sessionToken}/database/fetchUsernames`

Here's an example. It's using the **login** query with the specified parameters:
```javascript
fetch(`http://${endpoint}/jsfour-core/${sessionToken}/database/login`, {
    method: 'POST',
    mode: 'cors',
    body: JSON.stringify({
        '@username': 'username',
        '@password': 'password'
    })
})
.then(response => response.json())
.then(data => {
    console.log( data );
});
```

### IS THIS SECURE?
Since you can send a post request from all different kinds of programs this is a bit risky to use. 

To make this secure as possible you need to meet the following requirements to be able to send a post request:
* Send the request from a FiveM client.
* Be signed in on the server you're sending the request to.

Regarding the gamemaster you need to have a password, it's recommended to use something good.

If you have any <a href="https://github.com/jonassvensson4/jsfour-core/issues">suggestions</a> on how to make this more secure, feel free to add it to the suggestions section.

