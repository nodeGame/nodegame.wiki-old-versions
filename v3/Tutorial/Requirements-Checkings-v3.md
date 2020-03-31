- status: complete
- version: 3.x
- follows from: [Waiting Room](Waiting-Room-v3)
    
The requirements room is the component responsible for checking the
technical suitability of an incoming client to play the game.

The following files are available:

* requirements.settings.js
* requirements.js

## File: requirements.settings.js 

Firstly, you need to enable the


* **maxExecTime**: If set, limits the maximum execution time for all
    requirement tests. Default: 6000 ms.

* **speedTest**: If set, client must exchange messages with server
    "quickly enough", as defined by the settings. For example:
        
    ```javascript 
    speedTest = {
        messages: 10,
        time: 1000
    };
    ```

* **viewportSize**: If set, client must have a resolution between the
    min and max specified. For example:
     
    ```javascript
    viewportSize = {
        minX: 1366,
        minY: 768,
        maxX: 13660,
        maxY: 7680
    };
    ```
    
* **browserDetect**: Checks the browser and device of incoming
    connection. Available options:
    
     - parser: (optional) a function that will parse the userAgent.
         Default:
         [ua-parser-js](https://github.com/faisalman/ua-parser-js)
     - cb: a callback that takes an object containing the parsed userAgent
        and must return an object of the type:
        { success: true/false, errors: undefined|array of strings }

     Example:
     
     ```javascript
     browserDetect = {
         cb: function(ua, params) {
             if (ua.device.model || ua.device.type) {
                 return {
                     success: false,
                     errors: [ 'It seems you are using a mobile or tablet ' +
                               'device. You can participate to this game ' +
                               'only from a desktop or laptop computer ' +
                               'with a keyboard and a mouse. If you ' +
                               'can, try with another browser or device.' ]
                 };
             }
             return { success: true };
         }
     };
     ```

* **cookieSupport**: If set, client must support setting
     cookies. Accepted values:
        - 'persistent': cookies must persist across session
        - 'session': cookies must be set within same session
     
* **nextRoom**: If set, clients that pass the requirements are moved
     to this room. Default: the waiting room

* **doChecking**: If TRUE, start testing the requirements
     *immediately. Default: TRUE

* **logicPath**: Path to a custom requirements room logic.


## File requirements.js 

This file is optional and it allows the game developer to add custom
requirements. This file must export a function that takes as input two
parameters:

 - the `requirements` object, and
 - the requirements settings, as defined in requirements.settings.js 

The `requirements` object exposes the following methods:

  - `.add(function)`: adds a requirements callback which will be
    evaluated on the connecting client. The function must return an
    object of the type:

         { success: false|false, errors: ['err 1', 'err2', ... ] };

    The requirement callback can also return asynchronously, by
    calling the _result_ function passed as first input parameter
    
  - `.onFailure(function)`: adds a callback function which will be
    executed on the client machine if at least one test fails.
    
  - `.onSuccess(function)`: adds a callback function which will be
    executed on the client machine if at least no test fails.
 
 - `.onComplete(function)`: adds a callback function which will be
    executed after all tests have been executed, regardless of the
    result.
  
  - `setMaxExecutionTime(number)`: sets the maximum execution time for
    all tests.
    
## Next

* [Authorization Rules](Authorization-Rules-v3)

## Go Back to 

* [Home](Home)
