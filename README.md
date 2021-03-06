## Socket.IO API

### Client Side

<hr><br>

#### Top-level

These are exposed by `require('socket.io-client')` or is defined as `io` in a browser:

##### Properties

- `version` _(String)_ defined the socket.io version
- `protocol` _(Number)_: defined the protocol supported
- `transports` _(Object)_: map of available transports
- `j` _(Array)_: internal array used to track jsonp callbacks
- `sockets` _(Function)_: internal function defined in _socket.js_
- `util` _(Object)_: internal utility functions for inheritance and more. Defined in _util.js_
- `JSON` _(Object)_: internal json utilities based on JSON2.js. Defined in _json.js_
- `parser` _(Object)_: internal object for encoding data. Defined in _parser.js_
- `EventEmitter` _(Object)_: EventEmitter Constructor. Defined in _events.js_
- `SocketNamespace`: _(Function)_: Internal function for namespacing different sockets. Defined in _namespace.js_
- `Transport` _(Function)_: Internal function for shared features among transports. Defined in _namespace.js_

##### Methods

- `connect`
    - Connects and returns a Client object.
    - **Parameters**
      - `Host`: String containing the full URI (e.g., "http://www.some_server.com")
      - `Options`: Map of connection options. View socket.options property to get a full list of options.
    - **Returns** `Client`

<hr><br>

#### Client

Object returned from from connect. This object is where you setup socket.io events and listeners. _Inherits from EventEmitter_.

##### Events

- `connecting`
    - Called when a connection is being attempted
    - **Arguments**
      - `Transport Name`: Name name of the transport the connection was established with (e.g., 'websocket')
- `connect`
    - Called when a socket connection is established
- `disconnect`
    - Called when a the established connection has been disconnected
	- **Arguments**
      - `Reason`: The reason the disconnect occurred (if known)


##### Properties

- `Socket`: Socket class instance
- `name` _(Function)_: internal namespace identifier
- `flags` _(Object)_:internal namespace property
- `json` _(Object)_: internal namespace Flag instance
- `ackPackets` _(Object)_: internal namespace property
- `acks` _(Object)_: internal namespace property
- `$events` _(Object)_: internal mapping of all registered events, included events registered with the "on" method. Never use directly. _Inherited from EventEmitter_

##### Methods

- `on`:
    - Registers a new event. _Inherited from EventEmitter_
    - **Parameters**
      - `Name`: _(String)_ Name of event 
      - `Function`: _(Function)_ The function to be executed when event is triggered
    - **Returns** `Client` for chaining
- `once`
    - Registers a new event. Listened for only a single time. _Inherited from EventEmitter_
    - **Parameters**
      - `Name`: _(String)_ Name of event 
      - `Function`: _(Function)_ The function to be executed when event is triggered
    - **Returns** `Client` for chaining
- `removeListener`
    - Removes a registered event. _Inherited from EventEmitter_
    - **Parameters**
      - `Name`: _(String)_ Name of event to remove
      - `Function`: _(Function)_ The function signature to remove
    - **Returns** `Client` for chaining
- `removeAllListeners`
    - Removes all registered listeners for a registered event. _Inherited from EventEmitter_
    - **Parameters**
      - `Name`: _(String)_ Name of event to remove
    - **Returns** `Client` for chaining
- `listeners`
    - Gets all listeners for a registered event. _Inherited from EventEmitter_
    - **Parameters**
      - `Name`: _(String)_ Name of event to get to registered listeners for
    - **Returns** `Events` _(Array)_ All events for a given name
- `emit`
    - Fire an event. _Inherited from EventEmitter_
    - **Parameters**
      - `Name`: _(String)_ Name of event to fire
      - `Args`: Remaining arguments passed to the receiving function
    - **Returns** `Success` _(Boolean)_ 

<hr><br>

#### Socket

Socket the the client uses to communicate to the socket.io server instance. Rarely will you want to work directly with this object but instead use the client instance. Exceptions to this are forcing a disconnect (client.socket.disconnect()), etc . _Inherits from EventEmitter_.

##### Properties

- `options` _(Object)_: All socket options
	- `secure`: _(boolean)_ whether this is a secure (HTTPS or WSS) connection
	- `port` : _(number)_ port number for connection. Will use 443 or 80 by default (depending on secure options)
	- `query` : _(string)_ Get like parameters to pass with the connection (e.g., "param1=foo&param2=bar")
	- `document`: Pointer to the dom level document, used insert the client side script in dom
	- `resource`: The url resource that it connects to. Default is 'socket.io'. You should not change this unless you really know what you are doing.
	- `transports`: Array of strings for the avaialable transports
	- `connect timeout`: How long in ms until it gives up connecting with a transport. Default: 10000 (ms)
	- `try multiple transports`: Boolean for whether it will attempt to try more than one 	
	- `reconnect`: Whether it will attempt to reconnect after being disconnected. Default: true,
	- `reconnection delay`: The starting delay for reconnecting. Socket.IO will exponentially increase the day for reconnecting after each failed attempt. Default: 500 (ms)
	- `reconnection limit`: How many times it will exponentially adjust the reconnection delay. Default: Infinity (ecmascript static value).
	- `max reconnection attempts`: How many times it will try to reconnect. Default: 10
	- `sync disconnect on unload`: Will attempt to synchronize the disconnect before disconnecting. Default: false
	- `auto connect`: Whether the socket should automatically connect as part of this Connect call. Default: true
	- `flash policy port`: Default: 10843
	- `manualFlush`: Whether the transport buffer should be flushed manually or not. Default: false
- `connected` _(Boolean)_: request that originated the Socket
- `open` _(Boolean)_: whether the transport has been upgraded
- `connecting` _(Boolean)_: opening|open|closing|closed
- `reconnecting` _(Boolean)_: transport reference
- `namespaces` _(Object)_:  internal object containing all transport namespaces

##### Methods

- `connect`:
    - Connects using the available transports. This is called for you using the top level connect.
    - **Parameters**
      - `fn`: _(Function)_ A callback upon a successful network connections
    - **Returns** `Socket` for chaining
- `packet`
    - Sends a message. Emit takes care of this on behalf of you.
    - **Returns** `Socket` for chaining
- `flushBuffer`
    - Manually flushes the buffer
- `disconnect`
    - Disconnects the current connected transport
    - **Returns** `Socket` for chaining
    
### Server Side

	var server = http.createServer(app); // Create server (app assumes express instance)
	
	// Top Level Object
	var topLevel = require('socket.io'); 
	
	// Manager instance, usually combines with the call above
	var io = io.listen(server, {
   		origins: '*:*'
	}); 

<hr><br>

#### Top-Level

These are exposed by `require('socket.io')`

##### Properties

- `version` _(String)_ defined the socket.io version
- `protocol` _(Number)_: defined the protocol supported
- `clientVersion` _(String)_: the client version served
- `Manager` _(Object)_: manager constructor (which is returned by listen)
- `Transport` _(Object)_: transport constructor
- `Socket` _(Object)_: socket constructor
- `Static` _(Object)_: static constructor
- `Store` _(Object)_: generic store constructor
- `MemoryStore` _(Object)_: memory store constructor (single isntance only)
- `RedisStore`: _(Object)_: redis store constructor (used for horizontally scaling)
- `parser` _(Function)_: internal object for encoding data. Defined in parser.js

##### Methods

- `listen`
    - Creates or piggy backs an an existing http server to handle socket.io events
    - **Parameters**
      - `Server`: An HTTPServer instance or a port to create a new instance using
      - `Options`: A JSON object of options for the manager (see manager docs for list and description)
      - `Callback`: A function that is called after the server instance is started (only valid if you passed a number for Server param)
    - **Returns** `Manager Instance`

<hr><br>

#### Manager

Created and returned by the listen method of the top level object.

It accepts the following options (pass to the connect method of the top level object):
- origins: '*:*'
- log: true
- store: new MemoryStore
- logger: new Logger
- static: new Static(this)
- heartbeats: true
- resource: '/socket.io'
- transports: defaultTransports
- authorization: false
- blacklist: ['disconnect']
- 'log level': 3
- 'log colors': tty.isatty(process.stdout.fd)
- 'close timeout': 60
- 'heartbeat interval': 25
- 'heartbeat timeout': 60
- 'polling duration': 20
- 'flash policy server': true
- 'flash policy port': 10843
- 'destroy upgrade': true
- 'destroy buffer size': 10E7
- 'browser client': true
- 'browser client cache': true
- 'browser client minification': false
- 'browser client etag': false
- 'browser client expires': 315360000
- 'browser client gzip': false
- 'browser client handler': false
- 'client store expiration': 15
- 'match origin protocol': false

##### Properties

- `store` _(Store Instance)_ Returns the store in use (use to get only)
- `log` _(Logger Instance)_ Instance of logger (use to get only)
- `static` _(Static Instance)_ Instance of static (use to get only)


##### Methods
- `get`
	- Used to get a setting/option using the provided key
	- **Parameters**
		- `key`: _(String)_ 
	- **Returns** `value for options using provided key
- `set`
	- Used to set a setting/option
	- **Parameters**
		- `key`: _(String)_ 
		- `value`: _(Any)_
	- **Returns** Manger Instance for chaining
- `enable`
	- Enables (sets to true) the settings/option using the provided key
	- **Parameters**
		- `key`: _(String)_ 
	- **Returns** Manger Instance for chaining
- `disable`
	- Disables (sets to false) the settings/option using the provided key
	- **Parameters**
		- `key`: _(String)_ 
	- **Returns** Manger Instance for chaining
- `enabled`
	- Checks whether the setting/option is enabled (equals true) using the provided key
	- **Parameters**
		- `key`: _(String)_ 
	- **Returns** _(Boolean)_
- `disable`
	- Checks whether the setting/option is disabled (equals false) using the provided key
	- **Parameters**
		- `key`: _(String)_ 
	- **Returns** _(Boolean)_
- `configure`
	- Used to configure settings based on the environment (NODE_ENV). If it provided environment is true then it calls the provided callback.
	- **Parameters**
		- `env`: _(String)_ Name of environment
		- `fn`: _(Function)_ Callback if the environment is considered true
	- **Returns** Manger Instance for chaining

##### Events
- `connect`
    - Called when a socket is connected

<hr><br>

#### Socket

Socket is the objects that allows you to setup events and emit events back to the client. You get a socket instance from the Managers connection event.

##### Properties
- `json` : Special 
- `broadcast`: Special property that when used prior to an emit will broadcast event to everyone except the socket that originates the event.
- `volatile`: Special property that when included prior to an emit will cause the emit to be volatile. Volatile messages are not assured to get to the client as a normal emit is.


##### Methods
- `in`
    - Puts socket.io into a specific room namespace
    - **Parameters**
      - `Room`: A strong for the user name you would like to use
    - **Returns** `Socket Instance in specified namespace`
- `emit`
	- Fires off a new event message
	- **Parameters**
		- `Name`: _(String)_ 
		- `args`: An arbritrary number of arguments can be added that will be given on the received end.
		- `acknowledgement`: _(function)_ A function at the end of the parameters will be treated an acknowledgement function. It will be called if the receiving end responds 
	- **Returns** `packet`
- `send`
	- Fires off a simple message that can be heard on the other end by listening the the _message_ event.
	- **Parameters**
		- `data`: _(anything)_ data you want to send
		- `acknowledgement` _(function)_ An acknowledgment function that will be called if the receiving end responds 
	- **Returns** `packet`
		
##### Events

<hr><br>
