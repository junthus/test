# whale.processes

| Description: | Use the `whale.processes` API to interact with the browser's processes. |
|---|---|
| Availability: | **Dev** channel only. [Learn more](api_index#dev_apis). |
| Permissions: | <span class="code">"processes"</span> |

<section id="toc">

## Summary

| Types |
|---|
| [Cache](#type-Cache) |
| [Process](#type-Process) |
| Methods |
| [getProcessIdForTab](#method-getProcessIdForTab) − `whale.processes.getProcessIdForTab(<span>integer tabId</span>, <span>function callback</span>)` |
| [terminate](#method-terminate) − `whale.processes.terminate(<span>integer processId</span>, <span class="optional">function callback</span>)` |
| [getProcessInfo](#method-getProcessInfo) − `whale.processes.getProcessInfo(<span>integer or array of integer processIds</span>, <span>boolean includeMemory</span>, <span>function callback</span>)` |
| Events |
| [onUpdated](#event-onUpdated) |
| [onUpdatedWithMemory](#event-onUpdatedWithMemory) |
| [onCreated](#event-onCreated) |
| [onUnresponsive](#event-onUnresponsive) |
| [onExited](#event-onExited) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Cache

| properties |
|---|
| double | size | 

The size of the cache, in bytes.
 |
| double | liveSize | 

The part of the cache that is utilized, in bytes.
 |

</div>

<div>

### Process

| properties |
|---|
| integer | id | 

Unique ID of the process provided by the browser.
 |
| integer | osProcessId | 

The ID of the process, as provided by the OS.
 |
| enum of `"browser"`, `"renderer"`, `"extension"`, `"notification"`, `"plugin"`, `"worker"`, `"nacl"`, `"utility"`, `"gpu"`, or `"other"` | type | 

The type of process.
 |
| string | profile | 

The profile which the process is associated with.
 |
| integer | naclDebugPort | 

The debugging port for Native Client processes. Zero for other process types and for NaCl processes that do not have debugging enabled.
 |
| array of object | tasks | 

Array of TaskInfos representing the tasks running on this process.

#### Properties of each object

<dl>

<div>

| string | title | 
|---|---|

The title of the task.
 |
| integer | <span class="optional">(optional)</span> tabId | 

Optional tab ID, if this task represents a tab running on a renderer process.
 |

</div>

</dl>
 |
| double | <span class="optional">(optional)</span> cpu | 

The most recent measurement of the process’s CPU usage, expressed as the percentage of a single CPU core used in total, by all of the process’s threads. This gives a value from zero to CpuInfo.numOfProcessors*100, which can exceed 100% in multi-threaded processes. Only available when receiving the object as part of a callback from onUpdated or onUpdatedWithMemory.
 |
| double | <span class="optional">(optional)</span> network | 

The most recent measurement of the process network usage, in bytes per second. Only available when receiving the object as part of a callback from onUpdated or onUpdatedWithMemory.
 |
| double | <span class="optional">(optional)</span> privateMemory | 

The most recent measurement of the process private memory usage, in bytes. Only available when receiving the object as part of a callback from onUpdatedWithMemory or getProcessInfo with the includeMemory flag.
 |
| double | <span class="optional">(optional)</span> jsMemoryAllocated | 

The most recent measurement of the process JavaScript allocated memory, in bytes. Only available when receiving the object as part of a callback from onUpdated or onUpdatedWithMemory.
 |
| double | <span class="optional">(optional)</span> jsMemoryUsed | 

The most recent measurement of the process JavaScript memory used, in bytes. Only available when receiving the object as part of a callback from onUpdated or onUpdatedWithMemory.
 |
| double | <span class="optional">(optional)</span> sqliteMemory | 

The most recent measurement of the process’s SQLite memory usage, in bytes. Only available when receiving the object as part of a callback from onUpdated or onUpdatedWithMemory.
 |
| [Cache](/extensions/processes#type-Cache) | <span class="optional">(optional)</span> imageCache | 

The most recent information about the image cache for the process. Only available when receiving the object as part of a callback from onUpdated or onUpdatedWithMemory.
 |
| [Cache](/extensions/processes#type-Cache) | <span class="optional">(optional)</span> scriptCache | 

The most recent information about the script cache for the process. Only available when receiving the object as part of a callback from onUpdated or onUpdatedWithMemory.
 |
| [Cache](/extensions/processes#type-Cache) | <span class="optional">(optional)</span> cssCache | 

The most recent information about the CSS cache for the process. Only available when receiving the object as part of a callback from onUpdated or onUpdatedWithMemory.
 |

</div>

## Methods

<div>

### getProcessIdForTab

<div class="summary">`whale.processes.getProcessIdForTab(<span>integer tabId</span>, <span>function callback</span>)`</div>

<div class="description">

Returns the ID of the renderer process for the specified tab.

| Parameters |
|---|
| integer | tabId | 

The ID of the tab for which the renderer process ID is to be returned.
 |
| function | callback | 

A callback to return the ID of the renderer process of a tab.

The _callback_ parameter should be a function that looks like this:

`function(integer processId) <span class="subdued">{...}</span>;`

| integer | processId | 
|---|---|

Process ID of the tab's renderer process.
 |
 |

</div>

</div>

<div>

### terminate

<div class="summary">`whale.processes.terminate(<span>integer processId</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Terminates the specified renderer process. Equivalent to visiting about:crash, but without changing the tab's URL.

| Parameters |
|---|
| integer | processId | 

The ID of the process to be terminated.
 |
| function | <span class="optional">(optional)</span> callback | 

A callback to report the status of the termination.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(boolean didTerminate) <span class="subdued">{...}</span>;`

| boolean | didTerminate | 
|---|---|

True if terminating the process was successful, and false otherwise.
 |
 |

</div>

</div>

<div>

### getProcessInfo

<div class="summary">`whale.processes.getProcessInfo(<span>integer or array of integer processIds</span>, <span>boolean includeMemory</span>, <span>function callback</span>)`</div>

<div class="description">

Retrieves the process information for each process ID specified.

| Parameters |
|---|
| integer or array of integer | processIds | 

The list of process IDs or single process ID for which to return the process information. An empty list indicates all processes are requested.
 |
| boolean | includeMemory | 

True if detailed memory usage is required. Note, collecting memory usage information incurs extra CPU usage and should only be queried for when needed.
 |
| function | callback | 

A callback called when the processes information is collected.

The _callback_ parameter should be a function that looks like this:

`function(object processes) <span class="subdued">{...}</span>;`

| object | processes | 
|---|---|

A dictionary of [Process](/extensions/processes#type-Process) objects for each requested process that is a live child process of the current browser process, indexed by process ID. Metrics requiring aggregation over time will not be populated in each Process object.
 |
 |

</div>

</div>

## Events

<div>

### onUpdated

<div class="description">

Fired each time the Task Manager updates its process statistics, providing the dictionary of updated Process objects, indexed by process ID.

<div>

#### addListener

<div class="summary">`whale.processes.onUpdated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object processes) <span class="subdued">{...}</span>;`

| object | processes | 
|---|---|

A dictionary of updated [Process](/extensions/processes#type-Process) objects for each live process in the browser, indexed by process ID. Metrics requiring aggregation over time will be populated in each Process object.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onUpdatedWithMemory

<div class="description">

Fired each time the Task Manager updates its process statistics, providing the dictionary of updated Process objects, indexed by process ID. Identical to onUpdate, with the addition of memory usage details included in each Process object. Note, collecting memory usage information incurs extra CPU usage and should only be listened for when needed.

<div>

#### addListener

<div class="summary">`whale.processes.onUpdatedWithMemory.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(object processes) <span class="subdued">{...}</span>;`

| object | processes | 
|---|---|

A dictionary of updated [Process](/extensions/processes#type-Process) objects for each live process in the browser, indexed by process ID. Memory usage details will be included in each Process object.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onCreated

<div class="description">

Fired each time a process is created, providing the corrseponding Process object.

<div>

#### addListener

<div class="summary">`whale.processes.onCreated.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Process](/extensions/processes#type-Process) process) <span class="subdued">{...}</span>;`

| [Process](/extensions/processes#type-Process) | process | 
|---|---|

Details of the process that was created. Metrics requiring aggregation over time will not be populated in the object.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onUnresponsive

<div class="description">

Fired each time a process becomes unresponsive, providing the corrseponding Process object.

<div>

#### addListener

<div class="summary">`whale.processes.onUnresponsive.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function( [Process](/extensions/processes#type-Process) process) <span class="subdued">{...}</span>;`

| [Process](/extensions/processes#type-Process) | process | 
|---|---|

Details of the unresponsive process. Metrics requiring aggregation over time will not be populated in the object. Only available for renderer processes.
 |
 |

</div>

</div>

</div>

</div>

<div>

### onExited

<div class="description">

Fired each time a process is terminated, providing the type of exit.

<div>

#### addListener

<div class="summary">`whale.processes.onExited.addListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

The _callback_ parameter should be a function that looks like this:

`function(integer processId, integer exitType, integer exitCode) <span class="subdued">{...}</span>;`

| integer | processId | 
|---|---|

The ID of the process that exited.
 |
| integer | exitType | 

The type of exit that occurred for the process - normal, abnormal, killed, crashed. Only available for renderer processes.
 |
| integer | exitCode | 

The exit code if the process exited abnormally. Only available for renderer processes.
 |
 |

</div>

</div>

</div>

</div>

</div>

</section>