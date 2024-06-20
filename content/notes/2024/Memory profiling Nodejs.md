+++
title = 'Memory profiling'
date = 2024-06-16T09:52:33.3333+05:30
draft = true
tags =[]
+++ 


A core dump is generated when a Node.js process encounters a critical error that it cannot recover from, such as segmentation faults, accessing invalid memory, or other fatal errors.

- By default, Node.js does not generate core dumps automatically. Instead, you typically need to enable core dumps on the operating system level.
- On Linux, for example, you can use `ulimit -c unlimited` to enable core dumps of unlimited size.
- Once enabled, if a Node.js process crashes, a core dump file (`core.<pid>`) is generated in the current working directory or the directory specified by the `core_pattern` setting.


```js
const memwatch = require('memwatch-next');

memwatch.on('leak', function(info) {
    console.error('Memory leak detected:', info);
    
    memwatch.gc();
});

```

Linux 
`sudo gcore <pid>` 
`gdb /path/to/node /path/to/core.<pid>`

`strings heapdump-394083985.931455.heapsnapshot | grep "twilio"` -> will print all the char contain twilio

`kill -USR2 PID` -> by default nodejs will listen and create a heapdump