# crIdentity
[![Build Status](https://api.travis-ci.org/ngutils/cr-identity.svg?branch=master)](https://travis-ci.org/ngutils/cr-identity)

Module for management of user's identity with crSession and crAcl

## Install
```bash
bower install cr-identity
```

## Init module
```javascript
angular.module(
        'ngtest',
        [
            'ui.router',
            'cr.acl',
            'cr.session',
            'cr.identity'
        ]
)
```

## Getting Started
This is a possibile implementation. crIdentity helps you to manage restore and purge of your user identity.

It triggers a few events:

* auth:identity:success is triggered after login event `auth:login:success`
* auth:restore:succes is triggered after success identity restore (example: F5 refresh)
* auth:purge:success is triggered after logout event `auth:logout:success`

To manage restore success you can use a promise $q.
```javascript
crIdentity.restore().then(function(identity) {

});
```

```javascript
.run(['$rootScope', 'crAcl', 'crSession', 'crRemoteHttp', 'crIdentity', '$state', '$log',
function run($rootScope, crAcl, crSession, crRemoteHttp, crIdentity, $state, $log) {

    //set default login state for unauth users
    crAcl.setRedirect("signin");

    //what append on user successful login
    $rootScope.$on('auth:identity:success', function(event, data) {
      $state.go("dashboard", {'area': 'test'});
    });

    // $rootScope.$on('auth:restore:success', function(event, data) {
    //
    // });

    //what append on user logout
    $rootScope.$on("auth:purge:success", function(event, data){
      $state.go("home");
    });
}])
```
