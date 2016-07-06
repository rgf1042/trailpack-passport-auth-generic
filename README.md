# trails-passport-auth

Trails-passport-auth for trails-authentication

## Install

```sh
$ npm install --save trailpack-passport-auth
```

## Configure

```js
// config/main.js
module.exports = {
  packs: [
    // ... other trailpacks
    require('trailpack-passport-auth-generic')
  ]
}
```

You need to add `passportInit` and optionally `passportSession` :
```js
// config/web.js
middlewares: {
        order: [
          'addMethods',
          'cookieParser',
          'session',
          'passportInit',
          'passportSession',
          'bodyParser',
          'methodOverride',
          'router',
          'www',
          '404',
          '500'
        ]
      }
```
And to configure sessions:
```js
// config/session.js
'use strict'

const JwtStrategy = require('passport-jwt').Strategy
const ExtractJwt = require('passport-jwt').ExtractJwt

const EXPIRES_IN_SECONDS = 60 * 60 * 24
const SECRET = process.env.tokenSecret || 'mysupersecuretoken';
const ALGORITHM = 'HS256'
const ISSUER = 'localhost'
const AUDIENCE = 'localhost'

module.exports = {
  secret: SECRET,//secret use by express for his sessions
  redirect: {
    login: '/',//Login successful
    logout: '/'//Logout successful
  },
  //Called when user is logged, before returning the json response
  onUserLogged: (app, user) => {
      return Promise.resolve(user)
  },
  strategies: {
    jwt: {
      strategy: JwtStrategy,
      tokenOptions: {
        expiresInSeconds: EXPIRES_IN_SECONDS,
        secret: SECRET,
        algorithm: ALGORITHM,
        issuer: ISSUER,
        audience: AUDIENCE
      },
      options: {
        secretOrKey: SECRET,
        issuer: ISSUER,
        audience: AUDIENCE,
        jwtFromRequest: ExtractJwt.fromAuthHeader()
      }
    },

    local: {
      strategy: require('passport-local').Strategy,
      options: {
        usernameField: 'username'
      }
    }
}
```
## Usage

Now you have your strategy ready for enjoy with passport.

## Support and Help
Full support by [Jaumard](https://github.com/jaumard) and this all is possible only and only help by him.
All Code by [Ycpatel813](https://github.com/ycpatel813/trailpack-passport-auth) , I only changed one line of code.
