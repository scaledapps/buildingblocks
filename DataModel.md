
# Data Structures

The proposed resource model showing the relationship between data objects that are used by this Building Block is illustrated in the diagram below.

## 1 Standards

- Ignore this section

## 2 E-Marketplace Building Block Data Model

- **TODO**: Create a Class Diagram based on the JSON

## 3 Data Elements

### 3.1 Ack

Describes the acknowledgement sent in response to an API call. If the implementation uses HTTP/S, then Ack must be returned in the same session. Every API call to a Provider Platform must be responded to with an Ack whether the Provider Platform intends to respond with a callback or not. This has one property called `status` that indicates the status of the Acknowledgement.

| Property | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Format | Enum      |
| -------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | --------- |
| status   | string | The status of the acknowledgement. If the request passes the validation criteria of the Provider Platform, then this is set to ACK. If a Provider Platform responds with status = `ACK` to a request, it is required to respond with a callback. If the request fails the validation criteria, then this is set to NACK. Additionally, if a Provider Platform does not intend to respond with a callback even after the request meets the validation criteria, it should set this value to `NACK`. |        | ACK, NACK |
| tags     | array  | A list of [tags](#7353-taggroup) containing any additional information sent along with the Acknowledgement.                                                                                                                                                                                                                                                                                                                                                                                        |

### 3.2 AddOn

Describes an additional item offered as a value-addition to a product or service. This does not exist independently in a catalog and is always associated with an item.

| Property   | Type   | Description                       |
| ---------- | ------ | --------------------------------- |
| id         | string | Provider-defined ID of the add-on |
| descriptor | object | [Descriptor](#7319-descriptor)    |
| price      | object | [Price](#7340-price)              |

### 3.3 Address

Describes a postal address that is stored as a string value.

### 3.4 Agent

Describes the direct performer, driver or executor that fulfills an order. It is usually a person. But in some rare cases, it could be a non-living entity like a drone, or a bot. Some examples of agents are Doctor in the healthcare sector, a driver in the mobility sector, or a delivery person in the logistics sector. This object can be set at any stage of the order lifecycle. This can be set at the discovery stage when the Provider Platform wants to provide details on the agent fulfilling the order, like in healthcare, where the doctor's name appears during search. This object can also used to search for a particular person that the customer wants fulfilling an order. Sometimes, this object gets instantiated after the order is confirmed, like in the case of on-demand taxis, where the driver is assigned after the user confirms the ride.

| Property     | Type   | Description                        |
| ------------ | ------ | ---------------------------------- |
| person       | object | [Person](#7339-person              |
| contact      | object | [Contact](#7313-contact)           |
| organization | object | [Organization](#7337-organization) |
| rating       | object | [Rating](#7343-rating)             |

### 3.5 Authorization

Describes an authorization mechanism used to start or end the fulfillment of an order. For example, in the mobility sector, the driver may require a one-time password to initiate the ride. In the healthcare sector, a patient may need to provide a password to open a video conference link during a teleconsultation.

| Property   | Type   | Description                                                                                                                                                                                                                                                 | Format    | Enum |
| ---------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ---- |
| type       | string | Type of authorization mechanism used. The allowed values for this field can be published as part of the network policy.                                                                                                                                     |
| token      | string | Token used for authorization. This is typically generated at the Provider Platform. The Application Platform can send this value to the user via any channel that it uses to authenticate the user like SMS, Email, Push notification, or in-app rendering. |
| valid_from | string | Timestamp in RFC3339 format from which token is valid                                                                                                                                                                                                       | date-time |      |
| valid_to   | string | Timestamp in RFC3339 format until which token is valid                                                                                                                                                                                                      | date-time |      |
| status     | string | Status of the token                                                                                                                                                                                                                                         |

### 3.6 Billing

Describes the billing details of an entity.<br>This has properties like name,organization,address,email,phone,time,tax_number, created_at,updated_at

| Property     | Type   | Description                                                                                                   | Format | Enum |
| ------------ | ------ | ------------------------------------------------------------------------------------------------------------- | ------ | ---- |
| name         | string | Name of the billable entity                                                                                   |
| organization | array  | Details of the [organization](#7337-organization) being billed.                                               |
| address      | array  | The [address](#733-address) of the billable entity                                                            |
| state        | array  | The [state](#7349-state) where the billable entity resides. This is important for state-level tax calculation |
| city         | array  | The [city](#7312-city) where the billable entity resides.                                                     |
| email        | string | Email address where the bill is sent to                                                                       | email  |      |
| phone        | string | Phone number of the billable entity                                                                           |
| time         | array  | Details regarding the billing period. ([Time](#7354-time))                                                    |
| tax_id       | string | ID of the billable entity as recognized by the taxation authority                                             |

### 3.7 Cancellation

Describes a cancellation event

| Property               | Type   | Description                                                                                       | Format    | Enum               |
| ---------------------- | ------ | ------------------------------------------------------------------------------------------------- | --------- | ------------------ |
| time                   | string | Date-time when the order was cancelled by the buyer                                               | date-time |                    |
| cancelled_by           | string |                                                                                                   |           | CONSUMER, PROVIDER |
| reason                 | array  | The reason for cancellation. ([Option](#7335-option))                                             |
| additional_description | array  | Any additional information regarding the nature of cancellation. ([Descriptor](#7319-descriptor)) |

### 3.8 CancellationTerm

Describes the cancellation terms of an item or an order. This can be referenced at an item or order level. Item-level cancellation terms can override the terms at the order level.

| Property          | Type    | Description                                                                                                  |
| ----------------- | ------- | ------------------------------------------------------------------------------------------------------------ |
| fulfillment_state | array   | The state of fulfillment during which this term is applicable. ([Fulfillment State](#7326-fulfillmentstate)) |
| reason_required   | boolean | Indicates whether a reason is required to cancel the order                                                   |
| cancel_by         | array   | Information related to the [time](#7354-time) of cancellation.                                               |
| cancellation_fee  | object  | [Fee](#7323-fee)                                                                                             |
| xinput            | object  | [XInput](#7357-xinput)                                                                                       |
| external_ref      | object  | [MediaFile](#7333-mediafile)                                                                                 |

### 3.9 Catalog

Describes the products or services offered by a Provider Platform. This is typically sent as the response to a search intent from a Application Platform. The payment terms, offers and terms of fulfillment supported by the Provider Platform can also be included here. The Provider Platform can show hierarchical nature of products/services in its catalog using the parent_category_id in categories. The Provider Platform can also send a ttl (time to live) in the context which is the duration for which a Application Platform can cache the catalog and use the cached catalog. <br>This has properties like bbp/descriptor,bbp/categories,bbp/fulfillments,bbp/payments,bbp/offers,bbp/providers and exp<br>This is used in the following situations.<br><ul><li>This is typically used in the discovery stage when the Provider Platform sends the details of the products and services it offers as response to a search intent from the Application Platform. </li></ul>

| Property     | Type   | Description                                                                                                                                                                                  | Format    | Enum |
| ------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ---- |
| descriptor   | object | [Descriptor](#7319-descriptor)                                                                                                                                                               |
| fulfillments | array  | [Fulfillment](#7325-fulfillment) modes offered at the Provider Platform level. This is used when a Provider Platform itself offers fulfillments on behalf of the providers it has onboarded. |
| payments     | array  | [Payment](#7338-payment) terms offered by the Provider Platform for all transactions. This can be overriden at the provider level.                                                           |
| offers       | array  | [Offers](#7334-offer) at the Provider Platform-level. This is common across all providers onboarded by the Provider Platform.                                                                |
| providers    | array  | [Provider](#7341-provider)                                                                                                                                                                   |
| exp          | string | Timestamp after which catalog will expire                                                                                                                                                    | date-time |      |
| ttl          | string | Duration in seconds after which this catalog will expire                                                                                                                                     |

### 3.10 Category

A label under which a collection of items can be grouped.

| Property           | Type   | Description                                 |
| ------------------ | ------ | ------------------------------------------- |
| id                 | string | ID of the category                          |
| parent_category_id | object | [Category](#7310-category)                  |
| descriptor         | object | [Descriptor](#7319-descriptor)              |
| time               | object | [Time](#7354-time)                          |
| ttl                | object | Time to live for an instance of this schema |
| tags               | array  | [tags](#7353-taggroup)                      |

### 3.11 Circle

Describes a circular region of a specified radius centered at a specified GPS coordinate.

| Property | Type   | Description            |
| -------- | ------ | ---------------------- |
| gps      | object | [GPS](#7327-gps)       |
| radius   | object | [Scalar](#7347-scalar) |

### 3.12 City

Describes a city

| Property | Type   | Description      |
| -------- | ------ | ---------------- |
| name     | string | Name of the city |
| code     | string | City code        |

### 3.13 Contact

Describes the contact information of an entity

| Property | Type   | Description                                                      |
| -------- | ------ | ---------------------------------------------------------------- |
| phone    | string |                                                                  |
| email    | string |                                                                  |
| jcard    | object | A Jcard object as per draft-ietf-jcardcal-jcard-03 specification |

### 3.14 Context

Every API call in beckn protocol has a context. It provides a high-level overview to the receiver about the nature of the intended transaction. Typically, it is the Application Platform that sets the transaction context based on the consumer's location and action on their UI. But sometimes, during unsolicited callbacks, the Provider Platform also sets the transaction context but it is usually the same as the context of a previous full-cycle, request-callback interaction between the Application Platform and the Provider Platform. The context object contains four types of fields. <ol><li>Demographic information about the transaction using fields like `domain`, `country`, and `region`.</li><li>Addressing details like the sending and receiving platform's ID and API URL.</li><li>Interoperability information like the protocol version that implemented by the sender and,</li><li>Transaction details like the method being called at the receiver's endpoint, the transaction_id that represents an end-to-end user session at the Application Platform, a message ID to pair requests with callbacks, a timestamp to capture sending times, a ttl to specifiy the validity of the request, and a key to encrypt information if necessary.</li></ol> This object must be passed in every interaction between a Application Platform and a Provider Platform. In HTTP/S implementations, it is not necessary to send the context during the synchronous response. However, in asynchronous protocols, the context must be sent during all interactions,

| Property       | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Format    | Enum |
| -------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ---- |
| domain         | array  | [Domain](#7320-domain) code that is relevant to this transaction context                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| location       | array  | The [location](#7332-location) where the transaction is intended to be fulfilled.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| action         | string | The Beckn protocol method being called by the sender and executed at the receiver.                                                                                                                                                                                                                                                                                                                                                                                                                       |
| version        | string | Version of transaction protocol being used by the sender.                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| bap_id         |        | Subscriber ID of the Application Platform                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| bap_uri        |        | Subscriber URL of the Application Platform for accepting callbacks from Provider Platforms.                                                                                                                                                                                                                                                                                                                                                                                                              |
| bpp_id         |        | Subscriber ID of the Provider Platform                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| bpp_uri        |        | Subscriber URL of the Provider Platform for accepting calls from Application Platforms.                                                                                                                                                                                                                                                                                                                                                                                                                  |
| transaction_id | string | This is a unique value which persists across all API calls from `search` through `confirm`. This is done to indicate an active user session across multiple requests. The Provider Platforms can use this value to push personalized recommendations, and dynamic offerings related to an ongoing transaction despite being unaware of the user active on the Application Platform.                                                                                                                      | uuid      |      |
| message_id     | string | This is a unique value which persists during a request / callback cycle. Since beckn protocol APIs are asynchronous, Application Platforms need a common value to match an incoming callback from a Provider Platform to an earlier call. This value can also be used to ignore duplicate messages coming from the Provider Platform. It is recommended to generate a fresh message_id for every new interaction. When sending unsolicited callbacks, Provider Platforms must generate a new message_id. | uuid      |      |
| timestamp      | string | Time of request generation in RFC3339 format                                                                                                                                                                                                                                                                                                                                                                                                                                                             | date-time |      |
| key            | string | The encryption public key of the sender                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ttl            | string | The duration in ISO8601 format after timestamp for which this message holds valid                                                                                                                                                                                                                                                                                                                                                                                                                        |

### 3.15 Country

Describes a country

| Property | Type   | Description                                          |
| -------- | ------ | ---------------------------------------------------- |
| name     | string | Name of the country                                  |
| code     | string | Country code as per ISO 3166-1 and ISO 3166-2 format |

### 3.16 Credential

Describes a credential of an entity - Person or Organization

| Property | Type   | Description                                     | Format | Enum |
| -------- | ------ | ----------------------------------------------- | ------ | ---- |
| id       | string |                                                 |
| type     | string | Takes a default value of "VerifiableCredential" |
| url      | string | URL of the credential                           | uri    |      |

### 3.17 Customer

Describes a customer buying/availing a product or a service

| Property | Type   | Description              |
| -------- | ------ | ------------------------ |
| person   | object | [Person](#7339-person)   |
| contact  | object | [Contact](#7313-contact) |

### 3.18 DecimalValue

Describes a numerical value in decimal form. The string value should match the pattern "[+-]?([0-9]\*[.])?[0-9]+"

### 3.19 Descriptor

Physical description of something.

| Property        | Type   | Description                  |
| --------------- | ------ | ---------------------------- |
| name            | string |                              |
| code            | string |                              |
| short_desc      | string |                              |
| long_desc       | string |                              |
| additional_desc | object |                              |
| media           | array  | [MediaFile](#7333-mediafile) |
| images          | array  | [Image](#7328-image)         |

### 3.20 Domain

Described the industry sector or sub-sector. The network policy should contain codes for all the industry sectors supported by the network. Domains can be created in varying levels of granularity. The granularity of a domain can be decided by the participants of the network. Too broad domains will result in irrelevant search broadcast calls to Provider Platforms that don't have services supporting the domain. Too narrow domains will result in a large number of registry entries for each Provider Platform. It is recommended that network facilitators actively collaborate with various working groups and network participants to carefully choose domain codes keeping in mind relevance, performance, and opportunity cost. It is recommended that networks choose broad domains like mobility, logistics, healthcare etc, and progressively granularize them as and when the number of network participants for each domain grows large.

| Property        | Type   | Description                                                                                                                                                                                                                 |
| --------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name            | string | Name of the domain                                                                                                                                                                                                          |
| code            |        | Standard code representing the domain. The standard is usually published as part of the network policy. Furthermore, the network facilitator should also provide a mechanism to provide the supported domains of a network. |
| additional_info | array  | A url that contains [addtional information](#7333-mediafile) about that domain.                                                                                                                                             |

### 3.21 Duration

Describes duration as per ISO8601 format

### 3.22 Error

Describes an error object that is returned by a Application Platform, Provider Platform or BG as a response or callback to an action by another network participant. This object is sent when any request received by a network participant is unacceptable. This object can be sent either during Ack or with the callback.

| Property | Type   | Description                                                                                                                      |
| -------- | ------ | -------------------------------------------------------------------------------------------------------------------------------- |
| code     | string | Standard error code. For full list of error codes, refer to docs/protocol-drafts/BECKN-005-ERROR-CODES-DRAFT-01.md of this repo" |
| paths    | string | Path to json schema generating the error. Used only during json schema validation errors                                         |
| message  | string | Human readable message describing the error. Used mainly for logging. Not recommended to be shown to the user.                   |

### 3.23 Fee

A fee applied on a particular entity

| Property   | Type  | Description                                                  |
| ---------- | ----- | ------------------------------------------------------------ |
| percentage | array | Percentage of a value. ([Decimal Value](#7318-decimalvalue)) |
| amount     | array | A fixed value ([Price](#7340-price))                         |

### 3.24 Form

Describes a form

| Property      | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Format | Enum                       |
| ------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------------------------- |
| url           | string | The URL from where the form can be fetched. The content fetched from the url must be processed as per the mime_type specified in this object. Once fetched, the rendering platform can choosed to render the form as-is as an embeddable element; or process it further to blend with the theme of the application. In case the interface is non-visual, the the render can process the form data and reproduce it as per the standard specified in the form. | uri    |                            |
| data          | object | The form submission data                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| mime_type     | string | This field indicates the nature and format of the form received by querying the url. MIME types are defined and standardized in IETF's RFC 6838.                                                                                                                                                                                                                                                                                                              |        | text/html, application/xml |
| submission_id | string |                                                                                                                                                                                                                                                                                                                                                                                                                                                               | uuid   |                            |

### 3.25 Fulfillment

Describes how a an order will be rendered/fulfilled to the end-customer

| Property | Type    | Description                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id       | string  | Unique reference ID to the fulfillment of an order                                                                                                                                                                                                                                                                                                                                                                         |
| type     | string  | A code that describes the mode of fulfillment. This is typically set when there are multiple ways an order can be fulfilled. For example, a retail order can be fulfilled either via store pickup or a home delivery. Similarly, a medical consultation can be provided either in-person or via tele-consultation. The network policy must publish standard fulfillment type codes for the different modes of fulfillment. |
| rateable | boolean | Whether the fulfillment can be rated or not                                                                                                                                                                                                                                                                                                                                                                                |
| rating   | array   | The [rating](#7343-rating) value of the fulfullment service.                                                                                                                                                                                                                                                                                                                                                               |
| state    | arrat   | The current state of fulfillment. The Provider Platform must set this value whenever the state of the order fulfillment changes and fire an unsolicited `on_status` call. ([Fulfillment State](#7326-fulfillmentstate))                                                                                                                                                                                                    |
| tracking | boolean | Indicates whether the fulfillment allows tracking. Has a default value of false.                                                                                                                                                                                                                                                                                                                                           |
| customer | array   | The person that will ultimately receive the order. ([Customer](#7317-customer))                                                                                                                                                                                                                                                                                                                                            |
| agent    | array   | The [agent](#734-agent) that is currently handling the fulfillment of the order                                                                                                                                                                                                                                                                                                                                            |
| contact  | object  | [Contact](#7313-contact)                                                                                                                                                                                                                                                                                                                                                                                                   |
| vehicle  | object  | [Vehicle](#7356-vehicle)                                                                                                                                                                                                                                                                                                                                                                                                   |
| stops    | array   | The list of logical [stops](#7350-stop) encountered during the fulfillment of an order.                                                                                                                                                                                                                                                                                                                                    |
| path     | string  | The physical path taken by the agent that can be rendered on a map. The allowed format of this property can be set by the network.                                                                                                                                                                                                                                                                                         |
| tags     | array   | [Tags](#7353-taggroup)                                                                                                                                                                                                                                                                                                                                                                                                     |

### 3.26 FulfillmentState

Describes the state of fulfillment

| Property   | Type   | Description                          | Format    | Enum |
| ---------- | ------ | ------------------------------------ | --------- | ---- |
| descriptor | object | [Descriptor](#7319-descriptor)       |
| updated_at | string |                                      | date-time |      |
| updated_by | string | ID of entity which changed the state |

### 3.27 Gps

Describes a GPS coordinate. The string value should be of the pattern "^[-+]?([1-8]?\\d(\\.\\d+)?|90(\\.0+)?),\\s\*[-+]?(180(\\.0+)?|((1[0-7]\\d)|([1-9]?\\d))(\\.\\d+)?)$".

| Property | Type | Description |
| -------- | ---- | ----------- |

### 3.28 Image

Describes an image

| Property  | Type   | Description                                                                              | Format | Enum                       |
| --------- | ------ | ---------------------------------------------------------------------------------------- | ------ | -------------------------- |
| url       | string | URL to the image. This can be a data url or an remote url                                | uri    |                            |
| size_type | string | The size of the image. The network policy can define the default dimensions of each type |        | xs, sm, md, lg, xl, custom |
| width     | string | Width of the image in pixels                                                             |
| height    | string | Height of the image in pixels                                                            |

### 3.29 Intent

The intent to buy or avail a product or a service. The Application Platform can declare the intent of the consumer containing <ul><li>What they want (A product, service, offer)</li><li>Who they want (A seller, service provider, agent etc)</li><li>Where they want it and where they want it from</li><li>When they want it (start and end time of fulfillment</li><li>How they want to pay for it</li></ul><br>This has properties like descriptor,provider,fulfillment,payment,category,offer,item,tags<br>This is typically used by the Application Platform to send the purpose of the user's search to the Provider Platform. This will be used by the Provider Platform to find products or services it offers that may match the user's intent.<br>For example, in Mobility, the mobility consumer declares a mobility intent. In this case, the mobility consumer declares information that describes various aspects of their journey like,<ul><li>Where would they like to begin their journey (intent.fulfillment.start.location)</li><li>Where would they like to end their journey (intent.fulfillment.end.location)</li><li>When would they like to begin their journey (intent.fulfillment.start.time)</li><li>When would they like to end their journey (intent.fulfillment.end.time)</li><li>Who is the transport service provider they would like to avail services from (intent.provider)</li><li>Who is traveling (This is not recommended in public networks) (intent.fulfillment.customer)</li><li>What kind of fare product would they like to purchase (intent.item)</li><li>What add-on services would they like to avail</li><li>What offers would they like to apply on their booking (intent.offer)</li><li>What category of services would they like to avail (intent.category)</li><li>What additional luggage are they carrying</li><li>How would they like to pay for their journey (intent.payment)</li></ul><br>For example, in health domain, a consumer declares the intent for a lab booking the describes various aspects of their booking like,<ul><li>Where would they like to get their scan/test done (intent.fulfillment.start.location)</li><li>When would they like to get their scan/test done (intent.fulfillment.start.time)</li><li>When would they like to get the results of their test/scan (intent.fulfillment.end.time)</li><li>Who is the service provider they would like to avail services from (intent.provider)</li><li>Who is getting the test/scan (intent.fulfillment.customer)</li><li>What kind of test/scan would they like to purchase (intent.item)</li><li>What category of services would they like to avail (intent.category)</li><li>How would they like to pay for their journey (intent.payment)</li></ul>

| Property    | Type  | Description                                                                                                  |
| ----------- | ----- | ------------------------------------------------------------------------------------------------------------ |
| descriptor  | array | A raw description of the search intent. Free text search strings, raw audio, etc can be sent in this object. |
| provider    | array | The [provider](#7341-provider) from which the customer wants to place to the order from                      |
| fulfillment | array | Details on how the customer wants their order fulfilled [Fulfillment](#7325-fulfillment)                     |
| payment     | array | Details on how the customer wants to pay for the order. [Payment](#7338-payment)                             |
| category    | array | Details on the item [category](#7310-category)                                                               |
| offer       | array | details on the [offer](#7334-offer) the customer wants to avail                                              |
| item        | array | Details of the [item](#7331-item) that the consumer wants to order                                           |
| tags        | array | [Tags](#7353-taggroup)                                                                                       |

### 3.30 ItemQuantity

Describes the count or amount of an item

| Property  | Type   | Description                                                                                                          |
| --------- | ------ | -------------------------------------------------------------------------------------------------------------------- |
| allocated | object | This represents the exact quantity allocated for purchase of the item.                                               |
| available | object | This represents the exact quantity available for purchase of the item. The buyer can only purchase multiples of this |
| maximum   | object | This represents the maximum quantity allowed for purchase of the item                                                |
| minimum   | object | This represents the minimum quantity allowed for purchase of the item                                                |
| selected  | object | This represents the quantity selected for purchase of the item                                                       |
| unitized  | object | This represents the quantity available in a single unit of the item                                                  |

### 3.31 Item

Describes a product or a service offered to the end consumer by the provider. In the mobility sector, it can represent a fare product like one way journey. In the logistics sector, it can represent the delivery service offering. In the retail domain it can represent a product like a grocery item.

| Property             | Type    | Description                                                                                                                                  |
| -------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| id                   | string  | ID of the item.                                                                                                                              |
| parent_item_id       | array   | ID of the [item](#7331-item), this item is a variant of                                                                                      |
| parent_item_quantity | array   | The number of units of the parent item this item is a multiple of [Item Quantity](#7330-itemquantity)                                        |
| descriptor           | array   | Physical description of the item [Descriptor](#7319-descriptor)                                                                              |
| creator              | array   | The creator of this item [Organization](#7337-organization)                                                                                  |
| price                | array   | The [price](#7340-price) of this item, if it has intrinsic value                                                                             |
| quantity             | array   | The selling quantity of the item [Item Quantity](#7330-itemquantity)                                                                         |
| category_ids         | array   | Categories this item can be listed under [Category](#7310-category)                                                                          |
| fulfillment_ids      | array   | Modes through which this item can be fulfilled [Fulfillment](#7325-fulfillment)                                                              |
| location_ids         | array   | Provider [Locations](#7332-location) this item is available in                                                                               |
| payment_ids          | array   | [Payment](#7338-payment) modalities through which this item can be ordered                                                                   |
| add_ons              | array   | [AddOn](#732-addon)                                                                                                                          |
| cancellation_terms   | array   | [Cancellation terms](#738-cancellationterm) of this item                                                                                     |
| refund_terms         | array   | Refund terms of this item                                                                                                                    |
| replacement_terms    | array   | Terms that are applicable be met when this item is replaced [Replacement Terms](#7345-replacementterm)                                       |
| return_terms         | array   | Terms that are applicable when this item is returned [Return Terms](#7346-returnterm)                                                        |
| xinput               | array   | Additional input required from the customer to purchase / avail this item [XInput](#7357-xinput)                                             |
| time                 | array   | Temporal attributes of this item. This property is used when the item exists on the catalog only for a limited period of [time](#7354-time). |
| rateable             | boolean | Whether this item can be rated                                                                                                               |
| rating               |         | The [rating](#7343-rating) of the item                                                                                                       |
| matched              | boolean | Whether this item is an exact match of the request                                                                                           |
| related              | boolean | Whether this item is a related item to the exactly matched item                                                                              |
| recommended          | boolean | Whether this item is a recommended item to a response                                                                                        |
| ttl                  | string  | Time to live in seconds for an instance of this schema                                                                                       |
| tags                 | array   | [Tags](#7353-taggroup)                                                                                                                       |

### 3.32 Location

The physical location of something

| Property   | Type   | Description                                                                                                               | Format | Enum |
| ---------- | ------ | ------------------------------------------------------------------------------------------------------------------------- | ------ | ---- |
| id         | string |                                                                                                                           |
| descriptor |        | [Descriptor](#7319-descriptor)                                                                                            |
| map_url    | string | The url to the map of the location. This can be a globally recognized map url or the one specified by the network policy. | uri    |      |
| gps        |        | The [GPS](#7327-gps) co-ordinates of this location.                                                                       |
| address    |        | The [address](#733-address) of this location.                                                                             |
| city       |        | The [city](#7312-city) this location is, or is located within                                                             |
| district   | string | The state this location is, or is located within                                                                          |
| state      |        | The [state](#7349-state) this location is, or is located within                                                           |
| country    |        | The [country](#7315-country) this location is, or is located within                                                       |
| area_code  | string |                                                                                                                           |
| circle     |        | [Circle](#7311-circle)                                                                                                    |
| polygon    | string | The boundary polygon of this location                                                                                     |
| 3dspace    | string | The three dimensional region describing this location                                                                     |
| rating     |        | The [rating](#7343-rating) of this location                                                                               |

### 3.33 MediaFile

This object contains a url to a media file.

| Property  | Type   | Description                                                                                                                               | Format | Enum |
| --------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---- |
| mimetype  | string | indicates the nature and format of the document, file, or assortment of bytes. MIME types are defined and standardized in IETF's RFC 6838 |
| url       | string | The URL of the file                                                                                                                       | uri    |      |
| signature | string | The digital signature of the file signed by the sender                                                                                    |
| dsa       | string | The signing algorithm used by the sender                                                                                                  |

### 3.34 Offer

An offer associated with a catalog. This is typically used to promote a particular product and enable more purchases.

| Property     | Type   | Description                    |
| ------------ | ------ | ------------------------------ |
| id           | string |                                |
| descriptor   | object | [Descriptor](#7319-descriptor) |
| location_ids | array  | [Location](#7332-location)     |
| category_ids | array  | [Category](#7310-category)     |
| item_ids     | array  | [Item](#7331-item)             |
| time         | object | [Time](#7354-time)             |
| tags         | array  | [Tags](#7353-taggroup)         |

### 3.35 Option

Describes a selectable option

| Property   | Type   | Description                    |
| ---------- | ------ | ------------------------------ |
| id         | string |                                |
| descriptor | object | [Descriptor](#7319-descriptor) |

### 3.36 Order

Describes a legal purchase order. It contains the complete details of the legal contract created between the buyer and the seller.

| Property           | Type   | Description                                                                                                                                                                                                                                                                                                                                                                    | Format    | Enum                        |
| ------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- | --------------------------- |
| id                 | string | Human-readable ID of the order. This is generated at the Provider Platform layer. The Provider Platform can either generate order id within its system or forward the order ID created at the provider level.                                                                                                                                                                  |
| ref_order_ids      | array  | A list of order IDs to link this order to previous orders.                                                                                                                                                                                                                                                                                                                     |
| status             | string | Status of the order. Allowed values can be defined by the network policy                                                                                                                                                                                                                                                                                                       |           | ACTIVE, COMPLETE, CANCELLED |
| type               | string | This is used to indicate the type of order being created to Provider Platforms. Sometimes orders can be linked to previous orders, like a replacement order in a retail domain. A follow-up consultation in healthcare domain. A single order part of a subscription order. The list of order types can be standardized at the network level. Has a default value of "DEFAULT" |           | DRAFT, DEFAULT              |
| provider           | array  | Details of the [provider](#7341-provider) whose catalog items have been selected.                                                                                                                                                                                                                                                                                              |
| items              | array  | The [items](#7331-item) purchased / availed in this order                                                                                                                                                                                                                                                                                                                      |
| add_ons            | array  | The [add-ons](#732-addon) purchased / availed in this order                                                                                                                                                                                                                                                                                                                    |
| offers             | array  | The [offers](#7334-offer) applied in this order                                                                                                                                                                                                                                                                                                                                |
| billing            | array  | The [billing](#736-billing) details of this order                                                                                                                                                                                                                                                                                                                              |
| fulfillments       | array  | The [fulfillments](#7325-fulfillment) involved in completing this order                                                                                                                                                                                                                                                                                                        |
| cancellation       | array  | The [cancellation](#737-cancellation) details of this order                                                                                                                                                                                                                                                                                                                    |
| cancellation_terms | array  | [Cancellation terms](#738-cancellationterm) of this item                                                                                                                                                                                                                                                                                                                       |
| refund_terms       | array  | Refund terms of this item                                                                                                                                                                                                                                                                                                                                                      |
| replacement_terms  | array  | [Replacement terms](#7345-replacementterm) of this item                                                                                                                                                                                                                                                                                                                        |
| return_terms       | array  | [Return terms](#7346-returnterm) of this item                                                                                                                                                                                                                                                                                                                                  |
| quote              | array  | The mutually agreed upon [quotation](#7342-quotation) for this order.                                                                                                                                                                                                                                                                                                          |
| payments           | array  | The terms of settlement for this order [Payment](#7338-payment)                                                                                                                                                                                                                                                                                                                |
| created_at         | string | The date-time of creation of this order                                                                                                                                                                                                                                                                                                                                        | date-time |                             |
| updated_at         | string | The date-time of updated of this order                                                                                                                                                                                                                                                                                                                                         | date-time |                             |
| xinput             | array  | Additional input required from the customer to confirm this order [XInput](#7357-xinput)                                                                                                                                                                                                                                                                                       |
| tags               | array  | [Tags](#7353-taggroup)                                                                                                                                                                                                                                                                                                                                                         |

### 3.37 Organization

An organization. Usually a recognized business entity.

| Property   | Type   | Description                                                                 |
| ---------- | ------ | --------------------------------------------------------------------------- |
| descriptor | object | [Descriptor](#7319-descriptor)                                              |
| address    | array  | The postal [address](#733-address) of the organization                      |
| state      | array  | The [state](#7349-state) where the the organization's address is registered |
| city       | array  | The [city](#7312-city) where the the organization's address is registered   |
| contact    | object | [Contact](#7313-contact)                                                    |

### 3.38 Payment

Describes the terms of settlement between the Application Platform and the Provider Platform for a single transaction. When instantiated, this object contains <ol><li>the amount that has to be settled,</li><li>The payment destination destination details</li><li>When the settlement should happen, and</li><li>A transaction reference ID</li></ol>. During a transaction, the Provider Platform reserves the right to decide the terms of payment. However, the Application Platform can send its terms to the Provider Platform first. If the Provider Platform does not agree to those terms, it must overwrite the terms and return them to the Application Platform. If overridden, the Application Platform must either agree to the terms sent by the Provider Platform in order to preserve the provider's autonomy, or abort the transaction. In case of such disagreements, the Application Platform and the Provider Platform can perform offline negotiations on the payment terms. Once an agreement is reached, the Application Platform and Provider Platform can resume transactions.

| Property     | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Format | Enum                                                         |
| ------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------ |
| id           | string | ID of the payment term that can be referred at an item or an order level in a catalog                                                                                                                                                                                                                                                                                                                                                                       |
| collected_by |        | This field indicates who is the collector of payment. The Application Platform can set this value to 'Application Platform' if it wants to collect the payment first and settle it to the Provider Platform. If the Provider Platform agrees to those terms, the Provider Platform should not send the payment url. Alternatively, the Provider Platform can set this field with the value 'Provider Platform' if it wants the payment to be made directly. |
| url          | string | A payment url to be called by the Application Platform. If empty, then the payment is to be done offline. The details of payment should be present in the params object. If tl_method = http/get, then the payment details will be sent as url params. Two url param values, `$transaction_id` and `$amount` are mandatory.                                                                                                                                 | uri    |                                                              |
| params       | object |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| type         | string |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |        | PRE-ORDER, PRE-FULFILLMENT, ON-FULFILLMENT, POST-FULFILLMENT |
| status       | string |                                                                                                                                                                                                                                                                                                                                                                                                                                                             |        | PAID, NOT-PAID                                               |
| time         | object | [Time](#7354-time)                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| tags         | array  | [Tags](#7353-taggroup)                                                                                                                                                                                                                                                                                                                                                                                                                                      |

### 3.39 Person

Describes a person as any individual

| Property  | Type   | Description                                                                                                                                                                                                                                                                               | Format | Enum |
| --------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---- |
| id        | string | Describes the identity of the person                                                                                                                                                                                                                                                      |
| url       | string | Profile url of the person                                                                                                                                                                                                                                                                 | uri    |      |
| name      | string | the name of the person                                                                                                                                                                                                                                                                    |
| image     | object | [Image](#7328-image)                                                                                                                                                                                                                                                                      |
| age       | array  | Age of the person [Duration](#7321-duration)                                                                                                                                                                                                                                              |
| dob       | string | Date of birth of the person                                                                                                                                                                                                                                                               | date   |      |
| gender    | string | Gender of something, typically a Person, but possibly also fictional characters, animals, etc. While Male and Female may be used, text strings are also acceptable for people who do not identify as a binary gender.Allowed values for this field can be published in the network policy |
| creds     | array  | [Credential](#7316-credential)                                                                                                                                                                                                                                                            |
| languages | array  |                                                                                                                                                                                                                                                                                           |
| skills    | array  |                                                                                                                                                                                                                                                                                           |
| tags      | array  | [Tags](#7353-taggroup)                                                                                                                                                                                                                                                                    |

### 3.40 Price

Describes the price of a product or service

| Property        | Type   | Description                         |
| --------------- | ------ | ----------------------------------- |
| currency        | string |                                     |
| value           | object | [Decimal Value](#7318-decimalvalue) |
| estimated_value | object | [Decimal Value](#7318-decimalvalue) |
| computed_value  | object | [Decimal Value](#7318-decimalvalue) |
| listed_value    | object | [Decimal Value](#7318-decimalvalue) |
| offered_value   | object | [Decimal Value](#7318-decimalvalue) |
| minimum_value   | object | [Decimal Value](#7318-decimalvalue) |
| maximum_value   | object | [Decimal Value](#7318-decimalvalue) |

### 3.41 Provider

Describes the catalog of a business.

| Property     | Type    | Description                                                                                                                                      | Format    | Enum |
| ------------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | --------- | ---- |
| id           | string  | Id of the provider                                                                                                                               |
| descriptor   | object  | [Descriptor](#7319-descriptor)                                                                                                                   |
| category_id  | string  | Category Id of the provider at the Provider Platform-level catalog                                                                               |
| rating       | object  | [Rating](#7343-rating)                                                                                                                           |
| time         | object  | [Time](#7354-time)                                                                                                                               |
| categories   | array   | [Category](#7310-category)                                                                                                                       |
| fulfillments | array   | [Fulfillments](#7325-fulfillment)                                                                                                                |
| payments     | array   | [Payment](#7338-payment)                                                                                                                         |
| locations    | array   | [Location](#7332-location)                                                                                                                       |
| offers       | array   | [Offer](#7334-offer)                                                                                                                             |
| items        | array   | [Item](#7331-item)                                                                                                                               |
| exp          | string  | Time after which catalog has to be refreshed                                                                                                     | date-time |      |
| rateable     | boolean | Whether this provider can be rated or not                                                                                                        |
| ttl          | integer | The time-to-live in seconds, for this object. This can be overriden at deeper levels. A value of -1 indicates that this object is not cacheable. |
| tags         | array   | [Tags](#7353-taggroup)]                                                                                                                          |

### 3.42 Quotation

Describes a quote. It is the estimated price of products or services from the Provider Platform.<br>This has properties like price, breakup, ttl

| Property | Type   | Description                           | Format | Enum |
| -------- | ------ | ------------------------------------- | ------ | ---- |
| id       | string | ID of the quote.                      | uuid   |      |
| price    | array  | The total quoted [price](#price)      |
| breakup  | array  | the breakup of the total quoted price |
| ttl      | object | [Duration](#7321-duration)            |

### 3.43 Rating

Describes the rating of an entity

| Property        | Type   | Description                                                                                                                                                                                                            | Format | Enum                                               |
| --------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | -------------------------------------------------- |
| rating_category | string | Category of the entity being rated                                                                                                                                                                                     |        | Item, Order, Fulfillment, Provider, Agent, Support |
| id              | string | Id of the object being rated                                                                                                                                                                                           |
| value           | string | Rating value given to the object. This can be a single value or can also contain an inequality operator like gt, gte, lt, lte. This can also contain an inequality expression containing logical operators like && and |        | .                                                  |

### 3.44 Region

Describes an arbitrary region of space. The network policy should contain a published list of supported regions by the network.

| Property   | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                                                         | Format | Enum    |
| ---------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------- |
| dimensions | string | The number of dimensions that are used to describe any point inside that region. The most common dimensionality of a region is 2, that represents an area on a map. There are regions on the map that can be approximated to one-dimensional regions like roads, railway lines, or shipping lines. 3 dimensional regions are rarer, but are gaining popularity as flying drones are being adopted for various fulfillment services. |        | 1, 2, 3 |
| type       | string | The type of region. This is used to specify the granularity of the region represented by this object. Various examples of two-dimensional region types are city, country, state, district, and so on. The network policy should contain a list of all possible region types supported by the network.                                                                                                                               |
| name       | string | Name of the region as specified on the map where that region exists.                                                                                                                                                                                                                                                                                                                                                                |
| code       | string | A standard code representing the region. This should be interpreted in the same way by all network participants.                                                                                                                                                                                                                                                                                                                    |
| boundary   | string | A string representing the boundary of the region. One-dimensional regions are represented by polylines. Two-dimensional regions are represented by polygons, and three-dimensional regions can represented by polyhedra.                                                                                                                                                                                                            |
| map_url    | string | The url to the map of the region. This can be a globally recognized map or the one specified by the network policy.                                                                                                                                                                                                                                                                                                                 |

### 3.45 ReplacementTerm

The replacement policy of an item or an order

| Property          | Type   | Description                                                                                                                                                                               |
| ----------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| fulfillment_state | array  | The state of fulfillment during which this term is applicable. [Fulfillment State](#7326-fulfillmentstate)                                                                                |
| replace_within    | array  | Applicable only for buyer managed returns where the buyer has to replace the item before a certain date-time, failing which they will not be eligible for replacement. [Time](#7354-time) |
| external_ref      | object | [Media File](#7333-mediafile)                                                                                                                                                             |

### 3.46 ReturnTerm

Describes the return policy of an item or an order

| Property               | Type    | Description                                                                                                                                                                                      | Format | Enum               |
| ---------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ | ------------------ |
| fulfillment_state      | array   | The state of fulfillment during which this term IETF''s applicable. [Fulfillment State](#7326-fulfillmentstate)                                                                                  |
| return_eligible        | boolean | Indicates whether the item is eligible for return                                                                                                                                                |
| return_time            | array   | Applicable only for buyer managed returns where the buyer has to return the item to the origin before a certain date-time, failing which they will not be eligible for refund.[Time](#7354-time) |
| return_location        | array   | The [location](#7332-location) where the item or order must / will be returned to                                                                                                                |
| fulfillment_managed_by | string  | The entity that will perform the return                                                                                                                                                          |        | CONSUMER, PROVIDER |

### 3.47 Scalar

Describes a scalar

| Property        | Type   | Description                         | Format | Enum               |
| --------------- | ------ | ----------------------------------- | ------ | ------------------ |
| type            | string |                                     |        | CONSTANT, VARIABLE |
| value           | object | [Decimal Value](#7318-decimalvalue) |
| estimated_value | object | [Decimal Value](#7318-decimalvalue) |
| computed_value  | object | [Decimal Value](#7318-decimalvalue) |
| range           | object |                                     |
| unit            | string |                                     |

### 3.48 Schedule

Describes schedule as a repeating time period used to describe a regularly recurring event. At a minimum a schedule will specify frequency which describes the interval between occurrences of the event. Additional information can be provided to specify the schedule more precisely. This includes identifying the timestamps(s) of when the event will take place. Schedules may also have holidays to exclude a specific day from the schedule.<br>This has properties like frequency, holidays, times

| Property  | Type   | Description                |
| --------- | ------ | -------------------------- |
| frequency | object | [Duration](#7321-duration) |
| holidays  | array  |                            |
| times     | array  |                            |

### 3.49 State

A bounded geopolitical region of governance inside a country.

| Property | Type   | Description                                          |
| -------- | ------ | ---------------------------------------------------- |
| name     | string | Name of the state                                    |
| code     | string | State code as per country or international standards |

### 3.50 Stop

A logical point in space and time during the fulfillment of an order.

| Property       | Type   | Description                                                                             |
| -------------- | ------ | --------------------------------------------------------------------------------------- |
| id             | string |                                                                                         |
| parent_stop_id | string |                                                                                         |
| location       | array  | [Location](#7332-location) of the stop                                                  |
| type           | string | The type of stop. Allowed values of this property can be defined by the network policy. |
| time           | array  | Timings applicable at the stop. [Time](#7354-time)                                      |
| instructions   | array  | Instructions that need to be followed at the stop [Descriptor](#7319-descriptor)        |
| contact        | array  | Contact details of the stop [Contact](#7313-contact)                                    |
| person         | array  | The details of the person present at the stop [Person](#7339-person)                    |
| authorization  | object | [Authorization](#735-authorization)                                                     |

### 3.51 Support

Details of customer support

| Property       | Type   | Description | Format | Enum |
| -------------- | ------ | ----------- | ------ | ---- |
| ref_id         | string |             |
| callback_phone | string |             | phone  |      |
| phone          | string |             | phone  |      |
| email          | string |             | email  |      |
| url            | string |             | uri    |      |

### 3.52 Tag

Describes a tag. This is used to contain extended metadata. This object can be added as a property to any schema to describe extended attributes. For Application Platforms, tags can be sent during search to optimize and filter search results. Provider Platforms can use tags to index their catalog to allow better search functionality. Tags are sent by the Provider Platform as part of the catalog response in the `on_search` callback. Tags are also meant for display purposes. Upon receiving a tag, Application Platforms are meant to render them as name-value pairs. This is particularly useful when rendering tabular information about a product or service.

| Property   | Type    | Description                                                                                                                                                                                                                |
| ---------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| descriptor | array   | Description of the Tag, can be used to store detailed information. [Descriptor](#7319-descriptor)                                                                                                                          |
| value      | string  | The value of the tag. This set by the Provider Platform and rendered as-is by the Application Platform.                                                                                                                    |
| display    | boolean | This value indicates if the tag is intended for display purposes. If set to `true`, then this tag must be displayed. If it is set to `false`, it should not be displayed. This value can override the group display value. |

### 3.53 TagGroup

A collection of tag objects with group level attributes. For detailed documentation on the Tags and Tag Groups schema go to https://github.com/beckn/protocol-specifications/discussions/316

| Property   | Type    | Description                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| display    | boolean | Indicates the display properties of the tag group. If display is set to false, then the group will not be displayed. If it is set to true, it should be displayed. However, group-level display properties can be overriden by individual tag-level display property. As this schema is purely for catalog display purposes, it is not recommended to send this value during search. Has a default value of true. |
| descriptor | array   | Description of the TagGroup, can be used to store detailed information. [Descriptor](#7319-descriptor)                                                                                                                                                                                                                                                                                                            |
| list       | array   | An array of Tag objects listed under this group. This property can be set by Application Platforms during search to narrow the `search` and achieve more relevant results. When received during `on_search`, Application Platforms must render this list under the heading described by the `name` property of this schema.                                                                                       |

### 3.54 Time

Describes time in its various forms. It can be a single point in time; duration; or a structured timetable of operations<br>This has properties like label, time stamp,duration,range, days, schedule

| Property  | Type   | Description                                          | Format    | Enum |
| --------- | ------ | ---------------------------------------------------- | --------- | ---- |
| label     | string |                                                      |
| timestamp | string |                                                      | date-time |      |
| duration  | object | [Duration](#7321-duration)                           |
| range     | object |                                                      |
| days      | string | comma separated values representing days of the week |
| schedule  | object | [Schedule](#7348-schedule)                           |

### 3.55 Tracking

Contains tracking information that can be used by the Application Platform to track the fulfillment of an order in real-time. which is useful for knowing the location of time sensitive deliveries.

| Property | Type   | Description                                                                                                                                                                                                                                                                                                                                                                                 | Format | Enum             |
| -------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------------- |
| id       | string | A unique tracking reference number                                                                                                                                                                                                                                                                                                                                                          |
| url      | string | A URL to the tracking endpoint. This can be a link to a tracking webpage, a webhook URL created by the Application Platform where Provider Platform can push the tracking data, or a GET url creaed by the Provider Platform which the Application Platform can poll to get the tracking data. It can also be a websocket URL where the Provider Platform can push real-time tracking data. | uri    |                  |
| location | array  | In case there is no real-time tracking endpoint available, this field will contain the latest [location](#7332-location) of the entity being tracked. The Provider Platform will update this value everytime the Application Platform calls the track API.                                                                                                                                  |
| status   | string | This value indicates if the tracking is currently active or not. If this value is `active`, then the Application Platform can begin tracking the order. If this value is `inactive`, the tracking URL is considered to be expired and the Application Platform should stop tracking the order.                                                                                              |        | active, inactive |

### 3.56 Vehicle

Describes a vehicle is a device that is designed or used to transport people or cargo over land, water, air, or through space.<br>This has properties like category, capacity, make, model, size,variant,color,energy_type,registration

| Property          | Type    | Description |
| ----------------- | ------- | ----------- |
| category          | string  |             |
| capacity          | integer |             |
| make              | string  |             |
| model             | string  |             |
| size              | string  |             |
| variant           | string  |             |
| color             | string  |             |
| energy_type       | string  |             |
| registration      | string  |             |
| wheels_count      | string  |             |
| cargo_volumne     | string  |             |
| wheelchair_access | string  |             |
| code              | string  |             |
| emission_standard | string  |             |

### 3.57 XInput

Contains any additional or extended inputs required to confirm an order. This is typically a Form Input. Sometimes, selection of catalog elements is not enough for the Provider Platform to confirm an order. For example, to confirm a flight ticket, the airline requires details of the passengers along with information on baggage, identity, in addition to the class of ticket. Similarly, a logistics company may require details on the nature of shipment in order to confirm the shipping. A recruiting firm may require additional details on the applicant in order to confirm a job application. For all such purposes, the can choose to send this object attached to any object in the catalog that is required to be sent while placing the order. This object can typically be sent at an item level or at the order level. The item level XInput will override the Order level XInput as it indicates a special requirement of information for that particular item. Hence the Application Platform must render a separate form for the Item and another form at the Order level before confirmation.

| Property | Type    | Description                                                                                            |
| -------- | ------- | ------------------------------------------------------------------------------------------------------ |
| form     | object  | [Form](#7324-form)                                                                                     |
| required | boolean | Indicates whether the form data is mandatorily required by the Provider Platform to confirm the order. |
