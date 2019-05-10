# Changes with new conizi models (Version 1)

* The $schema tag is now mandatory:
  * For example: "\$schema": "https://model.conizi.io/v1/transport/truck/groupage/forwarding/consignment.json"
* The primary Github repo is now [https://github.com/fleetboard-logistics/semantic-model]()
* The branch master, preproduction, production depends on the system is used for dev, test or produciton purposes. Please keep that in mind.
* The schema definitions could now also be found under [https://model.conizi.io/]()
    * For example [https://model.conizi.io/v1/transport/truck/groupage/forwarding/tour.json]()
  * The version is mandatory!
  * When a message is sent, please use this address [https://model.conizi.io/]() + the model id as $schema tag, please have a look at [simple-tour.json]({{ site.github.owner_url }}/semantic-model/tree/{{ site.branch }}/examples/transport/truck/groupage/forwarding/tour/simple-tour.json){:target="_blank"}
* The schema tag must be included for any child's like tour=>consignment
    * For example [simple-tour.json]({{ site.github.owner_url }}/semantic-model/tree/{{ site.branch }}/examples/transport/truck/groupage/forwarding/tour/simple-tour.json){:target="_blank"}
* The Address element contains now a "reference" property for all models, "referenceNumber" is not longer supported.
    * For example [simple-tour.json]({{ site.github.owner_url }}/semantic-model/tree/{{ site.branch }}/examples/transport/truck/groupage/forwarding/tour/simple-tour.json#L16){:target="_blank"}
    * Furhter examples of new models, could be found [here]({{ site.github.owner_url }}/semantic-model/tree/{{ site.branch }}/examples/transport/truck/groupage/forwarding){:target="_blank"} or under [JSON Examples](examples/index.html).

* The sender, receiver for routing information has changed, so no address information should be provided any longer. Please use the ediId, partnerId, or coniziId see [EdiMessageRouting](https://fleetboard-logistics.github.io/docs/conizi/semantic-models/site/classes/Conizi.Model.Shared.Entities.EdiMessageRouting.html){:target="_blank"}.
```json
"sender": {
    "ediId": "FLELO"
},
"receiver": {
    "ediId": "EIKONA"
}
```

* A format "time" was added => time relevant values must be formated as "HH:mm:ss" => "14:30:00"

```json
"timeUntil": {
    "title": "Time Until",
    "type": "string",
    "format": "time"
}
```

* A format "date" was added => date relevant values must be formated as "yyyy-MM-dd" => "2019-02-14"

```json
  "notBefore": {
    "$id": "notBefore",
    "title": "Not Before",
    "description": "The consignment must not be delivered before the given date",
    "type": "object",
    "additionalProperties": false,
    "properties": {
        "date": {
            "type": "string",
            "format": "date"
        }
    },
    "patternProperties": {
        "x-.*": {},
        "\\$.*": {}
    }
}
```
* A format "date-time" was added => date-time relevant values must be formated as [ISO-8601](https://de.wikipedia.org/wiki/ISO_8601) DateTime => "2019-02-09T16:47:00"
  
```json
 "shippingDate": {
    "title": "Shipping date",
    "description": "The date of the manifest which included the consignment",
    "type": "string",
    "format": "date-time"
}
```

* The "message" tag for events was renamed in "remarks" for purposes of unification

```json
  "remarks": {
    "title": "Remarks (free form)",
    "description": "Additional remarks",
    "type": "string"
}
```
e.g. "changeRequest.message" is now "changeRequest.remarks"
