# HttpStateProvider
This is an implemenation of an Ext JS state provider (since Ext JS 4 located at _Ext.state.Provider_) providing a remote storage of states via HTTP. See Sencha Ext JS documentation at http://docs.sencha.com/ext-js/4-1/#!/api/Ext.state.Provider

In general, the _hlx.base.state.HttpProvider_ has following options:

* `storeSyncOnLoadEnabled` (default `false`) if `true` the store will load in a blocking process. This prevents from a situation where other components will already read states but they aren't loaded yet.
* `buffered` (default `true`) if `true` the store will write state changes with a buffer avoiding multiple write requests at the same time
* `writeBuffer` (default `2000`) defines the buffer size used when `buffered` is enabled

Currently, the Ajax-Endpoint is `cookies.json` but will be configurable soon. Each record is a _<key,value>_ pair.

## Example
```javascript
Ext.state.Manager.setProvider(new hlx.base.state.HttpProvider());
```
This examples defines the remote http provider. Because `storeSyncOnLoadEnabled` is not enabled, the store will not be synced and therefor no states are available (even if the server could provide some).
Because of that you should call an `Ext.state.Manager.getProvider().store.sync()` explicitly. In a situation of user scope changes (let's say user login or logout without a page refresh) this must be done manually.

## Remote Server
The current default configuration uses the endpoint `cookies.json`.

* When fetching cookies from the server, a `GET cookies.json` request will be made. A list of records with `name` and `value` will be expected (see the model).
* When writing cookies to the server, a `POST cookies.json` request will be made. In that cases, one or more records with `name` and `value` will be expected.