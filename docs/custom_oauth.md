# Custom OAuth2

## Adding Your Custom OAuth2 Strategy

Custom OAuth2 strategies may be placed in the following directory:
```valueset-workbench-docker/valueset-workbench/auth```

!!! warning
    Files must be prefixed with ```custom-``` - for example, ```custom-myauth.js```

OAuth2 implementations are handled by [Passport](http://passportjs.org/). An example implementation is shown below.

``` javascript
'use strict';

var passport = require('passport'),
    jwt = require('jsonwebtoken'),
    request = require('request'),
    config = require('../config'),
    OAuth2Strategy = require('passport-oauth2');

// Give your custom OAuth2 strategy a unique name
var providerName = "mycompany";

module.exports = function () {

    // Register your strategy. The logo will be shown on the login page
    passport.register({
        name: providerName,
        logo: "https://your/server/logo.jpg"
    });

    // Custom OAuth2 strategy. You can hardcode your setup here,
    // but it is usually preferred to set these parameters via 'process.env' variables.
    // Note: Generally, 'callbackURL' shouldn't change from what is shown.
    passport.use(providerName, new OAuth2Strategy({
            authorizationURL: process.env.MYCOMPANY_AUTH_URL || 'none',
            tokenURL: process.env.MYCOMPANY_TOKEN_URL || 'none',
            clientID: process.env.MYCOMPANY_CLIENT_ID || 'none',
            callbackURL: config.externalUrl + "/auth/" + providerName + "/callback" // don't change this
        },
        function (accessToken, refreshToken, profile, cb) {
            var decoded = jwt.decode(accessToken);

            // Adjust these based on your specific JWT claims
            var user = {
                firstName: decoded.givenName,
                lastName: decoded.familyName,
                username: decoded.myCompanyID,
                displayName: decoded.displayName,
                email: decoded.email,
                provider: providerName
            };

            return cb(null, user);
        }
    ));

};
```