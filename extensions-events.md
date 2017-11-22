# whale.events

| Description: | The `whale.events` namespace contains common types used by APIs dispatching events to notify you when something interesting happens. |
|---|---|
| Availability: | Since Chrome 21. |

<section>

An `Event` is an object that allows you to be notified when something interesting happens. Here's an example of using the `whale.alarms.onAlarm` event to be notified whenever an alarm has elapsed:

<pre>      whale.alarms.onAlarm.**addListener(function(**alarm**) {**
        appendToLog('alarms.onAlarm --'
                    + ' name: '          + alarm.name
                    + ' scheduledTime: ' + alarm.scheduledTime);
      **});**
      </pre>

As the example shows, you register for notification using `addListener()`. The argument to `addListener()` is always a function that you define to handle the event, but the parameters to the function depend on which event you're handling. Checking the documentation for [alarms.onAlarm](/extensions/alarms#event-onAlarm), you can see that the function has a single parameter: an [alarms.Alarm](/extensions/alarms#type-Alarm) object that has details about the elapsed alarm.

Example APIs using Events: [alarms](/extensions/alarms), [i18n](/extensions/i18n), [identity](/extensions/identity), [runtime](/extensions/runtime). Most [chrome APIs](api_index) do.

<div class="doc-family extensions">

## Declarative Event Handlers

The declarative event handlers provide a means to define rules consisting of declarative conditions and actions. Conditions are evaluated in the browser rather than the JavaScript engine which reduces roundtrip latencies and allows for very high efficiency.

Declarative event handlers are used for example in the [Declarative Web Request API](declarativeWebRequest) and [Declarative Content API](declarativeContent). This page describes the underlying concepts of all declarative event handlers.

### Rules

The simplest possible rule consists of one or more conditions and one or more actions:

<pre>      var rule = {
        conditions: [ /* my conditions */ ],
        actions: [ /* my actions */ ]
      };
      </pre>

If any of the conditions is fulfilled, all actions are executed.

In addition to conditions and actions you may give each rule an identifier, which simplifies unregistering previously registered rules, and a priority to define precedences among rules. Priorities are only considered if rules conflict each other or need to be executed in a specific order. Actions are executed in descending order of the priority of their rules.

<pre>      var rule = {
        id: "my rule",  // optional, will be generated if not set.
        priority: 100,  // optional, defaults to 100.
        conditions: [ /* my conditions */ ],
        actions: [ /* my actions */ ]
      };
      </pre>

### Event objects

[Event objects](events) may support rules. These event objects don't call a callback function when events happen but test whether any registered rule has at least one fulfilled condition and execute the actions associated with this rule. Event objects supporting the declarative API have three relevant methods: [events.Event.addRules](/extensions/events#method-Event-addRules), [events.Event.removeRules](/extensions/events#method-Event-removeRules), and [events.Event.getRules](/extensions/events#method-Event-getRules).

### Adding rules

To add rules call the `addRules()` function of the event object. It takes an array of rule instances as its first parameter and a callback function that is called on completion.

<pre>      var rule_list = [rule1, rule2, ...];
      function addRules(rule_list, function callback(details) {...});
      </pre>

If the rules were inserted successfully, the `details` parameter contains an array of inserted rules appearing in the same order as in the passed `rule_list` where the optional parameters `id` and `priority` were filled with the generated values. If any rule is invalid, e.g., because it contained an invalid condition or action, none of the rules are added and the [runtime.lastError](/extensions/runtime#property-lastError) variable is set when the callback function is called. Each rule in `rule_list` must contain a unique identifier that is not currently used by another rule or an empty identifier.

**Note:** Rules are persistent across browsing sessions. Therefore, you should install rules during extension installation time using the `[runtime.onInstalled](/extensions/runtime#event-onInstalled)` event. Note that this event is also triggered when an extension is updated. Therefore, you should first clear previously installed rules and then register new rules.

### Removing rules

To remove rules call the `removeRules()` function. It accepts an optional array of rule identifiers as its first parameter and a callback function as its second parameter.

<pre>      var rule_ids = ["id1", "id2", ...];
      function removeRules(rule_ids, function callback() {...});
      </pre>

If `rule_ids` is an array of identifiers, all rules having identifiers listed in the array are removed. If `rule_ids` lists an identifier, that is unknown, this identifier is silently ignored. If `rule_ids` is `undefined`, all registered rules of this extension are removed. The `callback()` function is called when the rules were removed.

### Retrieving rules

To retrieve a list of currently registered rules, call the `getRules()` function. It accepts an optional array of rule identifiers with the same semantics as `removeRules` and a callback function.

<pre>      var rule_ids = ["id1", "id2", ...];
      function getRules(rule_ids, function callback(details) {...});
      </pre>

The `details` parameter passed to the `callback()` function refers to an array of rules including filled optional parameters.

### Performance

To achieve maximum performance, you should keep the following guidelines in mind:

*   Register and unregister rules in bulk. After each registration or unregistration, Chrome needs to update internal data structures. This update is an expensive operation.

    Instead of

    <pre>      var rule1 = {...};
          var rule2 = {...};
          whale.declarativeWebRequest.onRequest.addRules([rule1]);
          whale.declarativeWebRequest.onRequest.addRules([rule2]);</pre>

    prefer to write

    <pre>      var rule1 = {...};
          var rule2 = {...};
          whale.declarativeWebRequest.onRequest.addRules([rule1, rule2]);</pre>

*   Prefer substring matching over matching using regular expressions in a [events.UrlFilter](/extensions/events#type-UrlFilter). Substring based matching is extremely fast.

    Instead of

    <pre>      var match = new whale.declarativeWebRequest.RequestMatcher({
              url: {urlMatches: "example.com/[^?]*foo" } });</pre>

    prefer to write

    <pre>      var match = new whale.declarativeWebRequest.RequestMatcher({
              url: {hostSuffix: "example.com", pathContains: "foo"} });</pre>

*   If you have many rules that all share the same actions, you may merge the rules into one because rules trigger their actions as soon as a single condition is fulfilled. This speeds up the matching and reduces memory consumption for duplicate action sets.

    Instead of

    <pre>      var condition1 = new whale.declarativeWebRequest.RequestMatcher({
              url: { hostSuffix: 'example.com' } });
          var condition2 = new whale.declarativeWebRequest.RequestMatcher({
              url: { hostSuffix: 'foobar.com' } });
          var rule1 = { conditions: [condition1],
                        actions: [new whale.declarativeWebRequest.CancelRequest()]};
          var rule2 = { conditions: [condition2],
                        actions: [new whale.declarativeWebRequest.CancelRequest()]};
          whale.declarativeWebRequest.onRequest.addRules([rule1, rule2]);</pre>

    prefer to write

    <pre>      var rule = { conditions: [condition1, condition2],
                       actions: [new whale.declarativeWebRequest.CancelRequest()]};
          whale.declarativeWebRequest.onRequest.addRules([rule]);</pre>

</div>

<div class="doc-family extensions">

## Filtered events

Filtered events are a mechanism that allows listeners to specify a subset of events that they are interested in. A listener that makes use of a filter won't be invoked for events that don't pass the filter, which makes the listening code more declarative and efficient - an [event page](event_pages) page need not be woken up to handle events it doesn't care about.

Filtered events are intended to allow a transition from manual filtering code like this:

<pre>      whale.webNavigation.onCommitted.addListener(function(e) {
        if (hasHostSuffix(e.url, 'google.com') ||
            hasHostSuffix(e.url, 'google.com.au')) {
          // ...
        }
      });
      </pre>

into this:

<pre>      whale.webNavigation.onCommitted.addListener(function(e) {
        // ...
      }, {url: [{hostSuffix: 'google.com'},
                {hostSuffix: 'google.com.au'}]});
      </pre>

Events support specific filters that are meaningful to that event. The list of filters that an event supports will be listed in the documentation for that event in the "filters" section.

When matching URLs (as in the example above), event filters support the same URL matching capabilities as expressible with a [events.UrlFilter](/extensions/events#type-UrlFilter), except for scheme and port matching.

</div>

</section>

<section id="toc">

## Summary

| Types |
|---|
| [Rule](#type-Rule) |
| [Event](#type-Event) |
| [UrlFilter](#type-UrlFilter) |

</section>

<section>

<div class="api-reference">

## Types

<div>

### Rule

<dd>Description of a declarative rule for handling events.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> id | 

Optional identifier that allows referencing this rule.
 |
| array of string | <span class="optional">(optional)</span> tags | 

Since Chrome 28.

Tags can be used to annotate rules and perform operations on sets of rules.
 |
| array of any | conditions | 

List of conditions that can trigger the actions.
 |
| array of any | actions | 

List of actions that are triggered if one of the conditions is fulfilled.
 |
| integer | <span class="optional">(optional)</span> priority | 

Optional priority of this rule. Defaults to 100.
 |

</div>

<div>

### Event

<dd>An object which allows the addition and removal of listeners for a Chrome event.</dd>

| methods |
|---|
| 

<div>

#### addListener

<div class="summary">`Event.addListener(<span>function callback</span>)`</div>

<div class="description">

Registers an event listener _callback_ to an event.

| Parameters |
|---|
| function | callback | 

Called when an event occurs. The parameters of this function depend on the type of event.

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### removeListener

<div class="summary">`Event.removeListener(<span>function callback</span>)`</div>

<div class="description">

Deregisters an event listener _callback_ from an event.

| Parameters |
|---|
| function | callback | 

Listener that shall be unregistered.

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### hasListener

<div class="summary">`boolean Event.hasListener(<span>function callback</span>)`</div>

<div class="description">

| Parameters |
|---|
| function | callback | 

Listener whose registration status shall be tested.

The _callback_ parameter should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |
| 

<div>

#### hasListeners

<div class="summary">`boolean Event.hasListeners()`</div>

</div>
 |
| 

<div>

#### addRules

<div class="summary">`Event.addRules(<span>array of [Rule](/extensions/events#type-Rule) rules</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Registers rules to handle events.

| Parameters |
|---|
| array of [Rule](/extensions/events#type-Rule) | rules | 

Rules to be registered. These do not replace previously registered rules.
 |
| function | <span class="optional">(optional)</span> callback | 

Called with registered rules.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function(array of [Rule](/extensions/events#type-Rule) rules) <span class="subdued">{...}</span>;`

| array of [Rule](/extensions/events#type-Rule) | rules | 
|---|---|

Rules that were registered, the optional parameters are filled with values.
 |
 |

</div>

</div>
 |
| 

<div>

#### getRules

<div class="summary">`Event.getRules(<span class="optional">array of string ruleIdentifiers</span>, <span>function callback</span>)`</div>

<div class="description">

Returns currently registered rules.

| Parameters |
|---|
| array of string | <span class="optional">(optional)</span> ruleIdentifiers | 

If an array is passed, only rules with identifiers contained in this array are returned.
 |
| function | callback | 

Called with registered rules.

The _callback_ parameter should be a function that looks like this:

`function(array of [Rule](/extensions/events#type-Rule) rules) <span class="subdued">{...}</span>;`

| array of [Rule](/extensions/events#type-Rule) | rules | 
|---|---|

Rules that were registered, the optional parameters are filled with values.
 |
 |

</div>

</div>
 |
| 

<div>

#### removeRules

<div class="summary">`Event.removeRules(<span class="optional">array of string ruleIdentifiers</span>, <span class="optional">function callback</span>)`</div>

<div class="description">

Unregisters currently registered rules.

| Parameters |
|---|
| array of string | <span class="optional">(optional)</span> ruleIdentifiers | 

If an array is passed, only rules with identifiers contained in this array are unregistered.
 |
| function | <span class="optional">(optional)</span> callback | 

Called when rules were unregistered.

If you specify the _callback_ parameter, it should be a function that looks like this:

`function() <span class="subdued">{...}</span>;` |

</div>

</div>
 |

</div>

<div>

### UrlFilter

<dd>Filters URLs for various criteria. See [event filtering](events#filtered). All criteria are case sensitive.</dd>

| properties |
|---|
| string | <span class="optional">(optional)</span> hostContains | 

Matches if the host name of the URL contains a specified string. To test whether a host name component has a prefix 'foo', use hostContains: '.foo'. This matches 'www.foobar.com' and 'foo.com', because an implicit dot is added at the beginning of the host name. Similarly, hostContains can be used to match against component suffix ('foo.') and to exactly match against components ('.foo.'). Suffix- and exact-matching for the last components need to be done separately using hostSuffix, because no implicit dot is added at the end of the host name.
 |
| string | <span class="optional">(optional)</span> hostEquals | 

Matches if the host name of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> hostPrefix | 

Matches if the host name of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> hostSuffix | 

Matches if the host name of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> pathContains | 

Matches if the path segment of the URL contains a specified string.
 |
| string | <span class="optional">(optional)</span> pathEquals | 

Matches if the path segment of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> pathPrefix | 

Matches if the path segment of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> pathSuffix | 

Matches if the path segment of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> queryContains | 

Matches if the query segment of the URL contains a specified string.
 |
| string | <span class="optional">(optional)</span> queryEquals | 

Matches if the query segment of the URL is equal to a specified string.
 |
| string | <span class="optional">(optional)</span> queryPrefix | 

Matches if the query segment of the URL starts with a specified string.
 |
| string | <span class="optional">(optional)</span> querySuffix | 

Matches if the query segment of the URL ends with a specified string.
 |
| string | <span class="optional">(optional)</span> urlContains | 

Matches if the URL (without fragment identifier) contains a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlEquals | 

Matches if the URL (without fragment identifier) is equal to a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlMatches | 

Since Chrome 23.

Matches if the URL (without fragment identifier) matches a specified regular expression. Port numbers are stripped from the URL if they match the default port number. The regular expressions use the [RE2 syntax](https://github.com/google/re2/blob/master/doc/syntax.txt).
 |
| string | <span class="optional">(optional)</span> originAndPathMatches | 

Since Chrome 28.

Matches if the URL without query segment and fragment identifier matches a specified regular expression. Port numbers are stripped from the URL if they match the default port number. The regular expressions use the [RE2 syntax](https://github.com/google/re2/blob/master/doc/syntax.txt).
 |
| string | <span class="optional">(optional)</span> urlPrefix | 

Matches if the URL (without fragment identifier) starts with a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| string | <span class="optional">(optional)</span> urlSuffix | 

Matches if the URL (without fragment identifier) ends with a specified string. Port numbers are stripped from the URL if they match the default port number.
 |
| array of string | <span class="optional">(optional)</span> schemes | 

Matches if the scheme of the URL is equal to any of the schemes specified in the array.
 |
| array of integer or array of integer | <span class="optional">(optional)</span> ports | 

Matches if the port of the URL is contained in any of the specified port lists. For example `[80, 443, [1000, 1200]]` matches all requests on port 80, 443 and in the range 1000-1200.
 |

</div>

</div>

</section>