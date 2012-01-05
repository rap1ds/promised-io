Promised-IO is a cross-platform package for asynchronous promise-based IO. Promises
provide a simple robust mechanism asynchronicity with separation of concerns by encapsulating
eventual completion of an operation with side effect free callback registration
separate from call invocation. Promised-IO provides cross-platform 
file, HTTP, and system interaction with promises for asynchronous operations.

Promised-IO also utilizes "lazy arrays" for progressively completed 
actions or for streaming of data. Lazy arrays provide all the standard iterative Array methods for
receiving callbacks as actions are completed. Lazy arrays are utilized
for progressive loading of files and HTTP responses.

# Modules

## promise

The promise module provides the primary tools for creating new promises and interacting
with promises. The promise API used by promised-io is the [Promises/A](http://wiki.commonjs.org/wiki/Promises/A)
proposal used by Dojo, jQuery, and other toolkits. Within promised-io, a promise is 
defined as any object that implements the Promises/A API, that is they provide a
then() method that can take a callback. The then() methods definition is:

    promise.then(fulfilledHandler, errorHandler);

Promises can originate from a variety of sources, and promised-io provides a constructor, Deferred,
to create promises.

### Deferred

	deferred = require("promised-io/promise").Deferred(canceler);

The Deferred constructor is the primary mechanism for creating new promises. The Deferred
object is a form of a promise that with an interface for fulfilling or rejecting the promise.
A Deferred object is a means for a producer to resolve a promise and it also provides
a promise for consumers that are listening for the resolution of the promise. The basic
usage pattern looks like:

	var Deferred = require("promised-io/promise").Deferred;
    function delay(ms, value){
    	// create a new Deferred
    	var deferred = new Deferred();
    	setTimeout(function(){
    		// fulfill the deferred/promise, all listeners to the promise will be notified, and 
    		// provided the value as the value of the promise 
    		deferred.fulfill(value);
    	}, ms);
    	// return the promise that is associated with the Deferred object
    	return deferred.promise;
    }

The Deferred can optional take a canceler function. This function will cause resulting
promises to have a cancel() method, and if the cancel() method is called, the 
Deferred will be canceled and the canceler function will be called.

The Deferred object has the following methods and properties:

#### fulfill

    deferred.fulfill(value);

This will fulfill the Deferred's promise with the provided value. The fulfillment listeners to the promise
will be notified.

#### reject

    deferred.reject(error);

This will reject the Deferred's promise with the provided error. The error listeners to the promise
will be notified.

#### promise

This is the promise object associated with the Deferred instance. The promise object 
will not have any of the Deferred's fulfill or reject methods, and only provides an interface
for listening. This can be safely provided to consumers without any chance of being modified.

#### cancel

    deferred.cancel();

This will cancel the Deferred.
 
### currentContext

One of the challenges with working asynchronous code is that there can be times when
you wish for some contextual state information to be preserved across multiple
asynchronous actions, without having to actually pass the state to each function in
the asynchronous chain. A common examples of such contextual state would be tracking
the current transaction, or the currently logged in user. Such state information could be 
stored in a singleton (a module property or a global variable), but with asynchronous
actions being interleaved, this is unsuitable for tracking state across asynchronous continuations
of an action. 

The promised-io package's promise module provides a facility for tracking state across
asynchronous operations. The promise module tracks the "currentContext" global variable,
and whatever value that was in the variable at the time a promise was created
will be restored when that promise is fulfilled (or rejected). 

### 

Promised-IO is part of the Persevere project, and therefore is licensed under the
AFL or BSD license. The Persevere project is administered under the Dojo foundation,
and all contributions require a Dojo CLA.