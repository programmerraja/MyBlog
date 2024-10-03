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


Event loop monitor
```js
import { createHook } from "node:async_hooks";
import { tracer } from "/blog/event-loop-lag/v3/tracer.server";

const THRESHOLD_NS = 1e8; // 100ms

const cache = new Map<number, { type: string; start?: [number, number] }>();

function init(
  asyncId: number,
  type: string,
  triggerAsyncId: number,
  resource: any
) {
  cache.set(asyncId, {
    type,
  });
}

function destroy(asyncId: number) {
  cache.delete(asyncId);
}

function before(asyncId: number) {
  const cached = cache.get(asyncId);

  if (!cached) {
    return;
  }

  cache.set(asyncId, {
    ...cached,
    start: process.hrtime(),
  });
}

function after(asyncId: number) {
  const cached = cache.get(asyncId);

  if (!cached) {
    return;
  }

  cache.delete(asyncId);

  if (!cached.start) {
    return;
  }

  const diff = process.hrtime(cached.start);
  const diffNs = diff[0] * 1e9 + diff[1];
  if (diffNs > THRESHOLD_NS) {
    const time = diffNs / 1e6; // in ms

    const newSpan = tracer.startSpan("event-loop-blocked", {
      startTime: new Date(new Date().getTime() - time),
      attributes: {
        asyncType: cached.type,
        label: "EventLoopMonitor",
      },
    });

    newSpan.end();
  }
}

export const eventLoopMonitor = singleton("eventLoopMonitor", () => {
  const hook = createHook({ init, before, after, destroy });

  return {
    enable: () => {
      console.log("🥸  Initializing event loop monitor");

      hook.enable();
    },
    disable: () => {
      console.log("🥸  Disabling event loop monitor");

      hook.disable();
    },
  };
});
```


I used the basic "collectDefaultMetrics" of the prom-client package. We use Prometheus to gather metrics from instances, and Grafana to display Graphs.

About the ELU, I'm not aware of any "standard" prometheus ways to compute and report it. We do it manually via a small snippet of code like this

```

let lastELU = performance.eventLoopUtilization();

this._intervalRef = setInterval(() => {

// Store the current ELU so it can be assigned later.

const tmpELU = performance.eventLoopUtilization();

// Calculate the diff between the current and last before sending.

const report = performance.eventLoopUtilization(tmpELU, lastELU);

this._idleGauge.set(report.idle);

this._activeGauge.set(report.active);

this._utilizationGauge.set(report.utilization);

// Assign over the last value to report the next interval.

lastELU = tmpELU;

}, this._interval);

```

[https://www.npmjs.com/package/prom-client](https://www.npmjs.com/package/prom-client)

[https://prometheus.io/](https://prometheus.io/)

[https://grafana.com/](https://grafana.com/)