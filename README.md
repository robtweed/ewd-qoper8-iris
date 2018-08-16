# ewd-qoper8-iris: Integrates ewd-qoper8 worker modules with InterSystems IRIS Data Platform
 
Rob Tweed <rtweed@mgateway.com>  
16 August 2018, M/Gateway Developments Ltd [http://www.mgateway.com](http://www.mgateway.com)  

Twitter: @rtweed

Google Group for discussions, support, advice etc: [http://groups.google.co.uk/group/enterprise-web-developer-community](http://groups.google.co.uk/group/enterprise-web-developer-community)


## ewd-qoper8-iris

This module may be used to simplifiy the integration of the InterSystems IRIS Data Platform with ewd-qoper8 worker process modules. 
It additionally loads the ewd-document-store module to provide a very powerful and natural JavaScript interface to the underlying
Hierachical Storage database engine within IRIS.

## Installing

       npm install ewd-qoper8-iris
	   
## Using ewd-qoper8-iris

This module should be used with the start event handler of your ewd-qoper8 worker module, eg:

    this.on('start', function(isFirst) {
      var connectIrisTo = require('ewd-qoper8-iris');
      connectIrisTo(this);
    });

This will open a connection to an IRIS database using the following default parameters:

    {
      path: '/opt/iris/mgr',
      username: '_SYSTEM',
      password: 'SYS',
      namespace: 'USER',
      charset: 'UTF-8'
    }

If you want to modify any of these parameters, simply specify any differences and pass the params object as the second
argument, eg to change just the namespace:

    this.on('start', function(isFirst) {
      var connectIrisTo = require('ewd-qoper8-iris');
      var params = {
        namespace: 'GOLD'
      };
      connectIrisTo(this, params);
    });

ewd-qoper8-iris will load and initialise the ewd-document-store module, creating a DocumentStore object within your worker.

ewd-qoper8-iris takes responsibility for handling the 'stop' event, but provides you with 3 new events that you may handle:

- dbOpened: fires after the connection to IRIS is opened within a worker process
- dbClosed: fires after the connection to IRIS is closed within a worker process.  The worker exits immediately after this event
- DocumentStoreStarted: fires after the DocumentStore object has been instantiated.  This is a good place to handle DocumentStore events, 
 for example to maintain Document indices

The dbOpened event provides you with a single status object argument, allowing you to determine the success (or not) of
opening the connection to IRIS, so you could add the following handler in your worker module, for example:

    this.on('dbOpened', function(status) {
      console.log('IRIS was opened by worker ' + process.pid + ': status = ' + JSON.stringify(status));
    });


The dbClosed and DocumentStoreStarted events provide no arguments.


## License

 Copyright (c) 2018 M/Gateway Developments Ltd,                           
 Redhill, Surrey UK.                                                      
 All rights reserved.                                                     
                                                                           
  http://www.mgateway.com                                                  
  Email: rtweed@mgateway.com                                               
                                                                           
                                                                           
  Licensed under the Apache License, Version 2.0 (the "License");          
  you may not use this file except in compliance with the License.         
  You may obtain a copy of the License at                                  
                                                                           
      http://www.apache.org/licenses/LICENSE-2.0                           
                                                                           
  Unless required by applicable law or agreed to in writing, software      
  distributed under the License is distributed on an "AS IS" BASIS,        
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. 
  See the License for the specific language governing permissions and      
   limitations under the License.      
