---
layout: post
category : javascript
tagline: ""
tags : [AngularJS, javascript]
---
{% include JB/setup %}

**Problem description**: I have a form that has values loaded from server. How do I initialize ng-model?

### Best Practice
Never pull data from server directly, use a service to do it:
    
    app.controller('myCtrl', function($scope, myService) {
        $scope.email = myService.getEmail();
    });
    <input type="text" ng-model="email" />

### Use ng-init
If you absolutely MUST render your values into your HTML from your server, you can use ng-init:

    <input type="text" ng-model="email" ng-init="email='test@test.com'" />

But if you want to use this email to pull the user data from server in controller:

    app.controller('myCtrl', function($scope,userService) {
        $scope.user = userService.getEmail($scope.email);
    });

You will find the email variable is undefined. This is because in the compile phase, the controller gets created first and then ng-init is set. So you should use $watch to observe changes in the email variable:

    app.controller('myCtrl', function($scope,userService) {
        $scope.$watch("email", function(){
            $scope.user = userService.getEmail($scope.email);
        });
    });