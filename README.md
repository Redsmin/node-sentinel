# node-sentinel
=============

Redis Sentinel wrapper for Node.js.
Provides methods and events for the Redis Sentinel API.

![Sentinel](https://raw.github.com/JvrBaena/node-sentinel/master/stuff/sentinel.gif)


## Installation
```
  npm install node-sentinel
```  
## Usage

```javascript

var Sentinel = require('node-sentinel'),
    //init with ip and port for the sentinel server
    s = new Sentinel('127.0.0.1','26379');
    /******* Sentinel API examples *******/
    s.masters(function(err, masters) {
    
      /* masters contains
      [ { name: 'mymaster',
        ip: '127.0.0.1',
        port: '6379',
        runid: '',
        flags: 'master',
        'pending-commands': '0',
        'last-ok-ping-reply': '613',
        'last-ping-reply': '613',
        'info-refresh': '9101',
        'num-slaves': '1',
        'num-other-sentinels': '2',
        quorum: '2' }]    
        */
    });
    
    s.getMasterAddress('mymaster', function(err, masterInfo) {
      /* masterInfo contains
      { ip: '127.0.0.1', port: '6379' }
      */
    });
    
    s.isMasterDown('127.0.0.1', '6379', function(err, isMasterDown) {
      /* isMasterDown contains
      { isDown: false,  leaderSentinel: '7ca87187f80adc20979fb61efec296f965bee515' }
      */
    });
    /******* Sentinel pub/sub messages events *******/
    
    s.on('failover-triggered', function(data) {
      ...
    });
    
    s.on('failover-status', function(data) {
      switch(data.status) {
        case 'wait-start':
          ...
        break;
        case 'selecting-slave':
          ...
        break;
        case 'slave-selected':
          ...
        break;
        case 'sent-slaveof-noone':
          ...
        break;
        case 'wait-promotion':
          ...
        break;
        case 'reconfigure-slaves':
          ...
        break;
        case 'succeeded':
          ...
        break;
        case 'abort-no-good-slave-found':
          ...
        break;
        case 'abort-master-is-back':
          ...
        break;
        case 'abort-x-sdown':
          ...
        break;
      }
    });
    
    s.on('switch-master', function(event) {
      ...
    });

```
**TODO: Add description for each API method and each event**



## License

**MIT License**

Copyright (c) 2013 Javier Baena

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
