# Resilient specification

> Draft version under active discussion

## Client

Client documentation will be soon. Please check the first [client implementation](https://github.com/resilient-http/resilient.js) 
for further information

### Request flow algorithm

<img src="http://rawgit.com/resilient-http/resilient-http.github.io/master/images/algorithm.svg" />

## Discovery server API

The following API must be implemented by any of the discovery server consumed by Resilient

### Request

Resilient will perform a HTTP request to the configured discovery servers in order to retrieve
the service servers URLs.

It could perform a request to a custom path, transport any HTTP header and use any HTTP verb.
Resilient it's really flexible and configurable to define 
how the discovery request should be performed from the client

Discovery servers must be always a valid URL, including the protocol schema

##### Example

Using the JavaScript Resilient client:

```js
var client = Resilient({
  discovery: {
    // define the discovery servers list
    servers: ['http://discover1.server.me', 'http://discover2.server.me']
    // transport a custom headers, for example for auhtentication
    headers: {
      'Accept': 'application/json',
      'API-Token': 'd1964f12ff0388cc897d22b717d0fc3861df8063' 
    },
    // use POST to retrieve the servers list (default GET)
    method: 'POST'
  }
})
```

### Response

When Resilient ask to a discovery server, it should return a valid JSON array os string
with valid URLs.

URLs could contains paths, but not query params

##### Mandatory headers
 
```yaml
Content-Type: application/json
```

##### Response body

It should return an array of strings which implements the following JSONSchema
```json
{
  "type": "array",
  "items": {
    "type": "string",
    "require": true,
    "patternProperties": "^(?:([^:\/?#]+):\/\/)?((?:([^\/?#@]*)@)?([^\/?#:]*)(?:\:(\d*))?)?"
  },
  "minItems": 0,
  "uniqueItems": true
}
```

###### Example

```json
[
  "http://api1.server.me",
  "http://api2.server.me",
  "http://api3.server.me"
]
```

###### Valid URLs

```
http://server.me
http://server.me:8080
https://server.me/api/1.0
https://192.168.1.100:8000
```

###### Invalid URLs

```
server.me
http://server.me/?version=1.0
```

### Supported servers

- [Hydra](innotech.github.io/hydra/) - multi-cloud application discovery, management and balancing service 

## Discussion

Please, the opened [issue](https://github.com/resilient-http/spec/issues/new) to contribute and join to the discussion

## License

[MIT](http://opensource.org/licenses/MIT) - Resilient project contributos
