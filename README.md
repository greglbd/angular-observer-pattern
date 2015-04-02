# angular-observer-pattern
Observer Pattern as a service for angularJS
**Examples to follow**

## About
This is an angular factory which reflects the Observer Pattern it works well with the ControllerAs method of working as it can be much more efficient that $scope.$watch and more specific to a unique scope or object than $emit and $broadcast when used correctly. 

**Use Case:** You would use this pattern to communicate between 2 controllers that use the same model but are not connected in anyway

## How to use

**Require, angularJS!**

### Methods

**_observerService.attach = function(callback, event, id)**

function adds a listener to an event with a callback which is stored against the event with it's corresponding id.

**_observerService.detachById = function(id)**

function removes all occurences of one id from all events in the observer object

**_observerService.detachByEvent = function(event)** __most commonly used and least expensive__

function removes all occurences of the event from the observer Object

**_observerService.detachByEventAndId = function(event, id)**

removes all callbacks for an id in a specific event from the observer object

**_observerService.notify = function(event, parameters)**

notifies all observers of a specific event, can pass a params variable of any type

### Controller Example
Below example shows how to attach, notify and detach an event.
```
angular.module('app.controllers')
  .controller('ObserverExample',ObserverExample);
ObserverExample.$inject= ['ObserverService','$timeout'];
function ObserverExample(ObserverService, $timeout) {
    var vm = this;
    var id = 'vm1';
    ObserverService.attach(callbackFunction, 'let_me_know', id)
    
    function callbackFunction(params){
        console.log('now i know');
        ObserverService.detachByEvent('let_me_know')
    }
    
    $timeout(function(){
        ObserverService.notify('let_me_know');
    },5000);
}
```
Alternative way to remove event

```
angular.module('app.controllers')
  .controller('ObserverExample',ObserverExample);
ObserverExample.$inject= ['ObserverService','$timeout', '$scope'];
function ObserverExample(ObserverService, $timeout, $scope) {
    var vm = this;
    var id = 'vm1';
    ObserverService.attach(callbackFunction, 'let_me_know', id)
    
    function callbackFunction(params){
        console.log('now i know');
    }
    
    $timeout(function(){
        ObserverService.notify('let_me_know');
    },5000);
    
    // Cleanup listeners when this controller is destroyed
    $scope.$on('$destroy', function handler() {
        ObserverService.detachByEvent('let_me_know')
    });
}
```

