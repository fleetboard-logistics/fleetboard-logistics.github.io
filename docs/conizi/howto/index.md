# Access the conizi HTTP input API

This describes how to connect a system to the conizi HTTP Input API using the HTTP protocol.

A detailed tutorial can be found under the following [link](howto-conizi-http-input-api.pdf){:target="_blank"}.

There are some requirements to be fulfilled in order to be able to connect to the conizi platform.

* A company, site and a user must be created and activated on conizi.
* An HTTP Edi connection must be created and configured.
* You must have an API key for the established connection.

A short overview of the conizi input HTTP API could be found [here online](https://preproduction.dev.conizi.io/api/input/swagger/index.html){:target="_blank"}.

Further information, prerequisites and endpoints can be found in the above mentioned tutorial.

Examples for the messages to be transmitted can be found in the [JSON examples](../semantic-models/examples/index.md).

Further documentation on the C# models and properties, can be obtained [here](../semantic-models/site/index.html).

# Access the conizi HTTP Poll API Endpoint

This describes how to connect a system to the conizi HTTP Poll API using the HTTP protocol.

There are some requirements to be fulfilled in order to be able to connect to the conizi platform.  

* A company, site and a user must be created and activated on conizi.
* An HTTP Edi Outbound connection must be created and configured.
* You must have an API key for the established connection.

<!-- A detailed tutorial can be found under the following [link](https://git.fleetboard-logistics.com/snippets/14){:target="_blank"}. -->

## conizi HTTP Poll Endpoint Description (v1)

The conizi HTTP Poll endpoint will listen to: 
* Production: https://conizi.io/api/output
* Preproduction: https://preproduction.dev.conizi.io/api/output
* Staging: https://staging.dev.conizi.io/api/output

## API Documentation
* Production: [https://conizi.io/api/output/swagger](https://conizi.io/api/output/swagger)
* Preproduction: [https://preproduction.dev.conizi.io/api/output/swagger](https://preproduction.dev.conizi.io/api/output/swagger)  
* Staging: [https://staging.dev.conizi.io/api/output/swagger](https://staging.dev.conizi.io/api/output/swagger)

## GET /api/output/v1/{continuationToken}

**Attention: We recommend to only request data which are newer than a few days, otherwise it can come to longer waiting periods or timeouts. conizi is not to be used as an archive system, this endpoint is not designed for this!**

Gets a list of available items/files for the EDI Connection determined by the Api Key.
The order of files can't be guaranteed but we try to maintain FIFO order.
* Client-Header
  * X-Api-Key: Configuration via conizi Edi Connection.
* Parameter:
  * continuationToken (required): Continuation Token that will get the next available messages
  * maxItems: Maximum number of events to return. (1-1000, default: 100)
* Response:

```JSONC
{
  "next": "string", // URL to request the next files
  "count": 0, // Number of files returned
  "continuationToken": "string", // Currently used continuation token
  "nextContinuationToken": "string", // Next continuation token
  "items": [
    {
      "name": "string", // Name of the file/item
      "model": "string", // Model of the file/item (e.g. transport/truck...)
      "file": "string", // URL of the file (valid for 5 minutes)
      "correlationId": "string", // Correlation Id of the item
      "messageId": "string", // Message Id of the item
      "dateTimeUtc": "2019-07-15T13:33:07.038Z" // Date of the item
    }
  ]
}
```

## GET /api/output/v1/requestToken
Requests a token for a specific timestamp
* Client-Header
  * X-Api-Key: Configuration via conizi Edi Connection.
* Parameter:
  * timestamp: Timestamp in milliseconds for which to create a token. If empty the token is created for the current time.
* Response: Token as string

## Possible implementations
1. Request a token for now or a specific timestamp
2. Request all items with the token and maxItem = 100
3. Download all items
4. Use nextContinuationToken as next token
5. If response count is 100 restart from 2. after 10 Seconds
6. Wait 10 Minutes and then restart from 2.

