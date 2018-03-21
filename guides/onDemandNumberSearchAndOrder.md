{% multimethod %}
{% endmultimethod %}

# Search and order Phone Numbers on Demand {#top}

## About {#about}

This guide will walk through the recommended approach to searching and ordering phone numbers using the Bandwidth [Phone Number API](https://dashboard.bandwidth.com).  This will cover how to setup a subscription to manage phone number order alerts, how to search for available phone numbers, and finally how to order phone numbers.

### Use-cases

This guide is best suited for use-cases where an end-user is assigned a specific phone number that may want to customize their phone number.

## Assumptions
* You have signed up for the [Bandwidth Phone Number API](https://bandwidth.com)
* You are familiar with:
  * [Your API Credentials](../concepts/security.md)
  * [Async Nature of ordering phone numbers](../concepts/asyncOrder.md)
  * [Managing Locations](../concepts/manageLocations.md)
  * [Managing Sites](../concepts/manageSites.md)
  * Receiving HTTP Callbacks/Webhooks

## Steps
[Create Subscription](#create-subscription)
[Search for Phone Numbers](#search-for-phone-numbers)

[Test Subscription](#test-subscription)

## Create Subscription for Orders {#create-subscription}

The Bandwidth Phone Number API allows users to manage notifications on their account through the `/subscriptions` resource.  Subscriptions can be configured to send a HTTP Callback to a valid publically addressable URL or send an email to a valid email address.

This guide _only_ covers creating a `<CallbackSubscription>`.  For more information see the full guide on [managing subscriptions](./managingSubscriptions.md)

### Base URL
<code class="post">POST</code>`https://dashboard.bandwidth.com/api/accounts/{{accountId}}/subscriptions`

{% extendmethod %}

### Basic Parameters

| Parameter                                  | Required | Description                                                                                                    |
|:-------------------------------------------|:---------|:---------------------------------------------------------------------------------------------------------------|
| `OrderType`                                | Yes      | The Specific type of order in which to configure the subscription. <br> For this guide, the value is: `orders` |
| `CallbackSubscription`                     | Yes      | Contains the information about the callback url                                                                |
| `CallbackSubscription.URL`                 | Yes      | The URL to send the <code class="post">POST</code> request when order status changes.                          |
| `CallbackSubscription.Expiry`              | Yes      | How long to keep the subscription active. <br> Can be very large (100 years in seconds `3153600000`)           |
| `CallbackSubscription.CallbackCredentials` | No       | Container for the authentication credentials for the specified `URL`                                           |
| `CallbackCredentials.BasicAuthentication`  | No       | Container for Basic Authentication credentials.                                                                |
| `BasicAuthentication.UserName`             | No       | Username for Basic Authentication scheme. <br> Max 100 characters                                              |
| `BasicAuthentication.Password`             | No       | Password for Basic Authentication scheme. <br> Encrypted at rest and never returned by the API                 |
| `CallbackCredentials.PublicKey`            | No       | A BASE64 encoded public key that matches the server specified in the `URL`                                     |

The Basic authentication scheme is straightforward, but the requires a little more explanation. Please read more on [securing HTTP Callbacks](../concepts/secureCallbacks.md)

{% common %}

### Example XML to Create Subscription

```http
POST https://dashboard.../{{accountId}}/subscriptions HTTP/1.1
Content-Type: application/xml; charset=utf-8
Authorization: {user:password}
```
```xml
<Subscription>
    <OrderType>orders</OrderType>
    <CallbackSubscription>
        <URL>https://your-secure-url.com</URL>
        <Expiry>3153600000</Expiry>
        <CallbackCredentials>
            <BasicAuthentication>
                <Username>User15</Username>
                <Password>Hunter15</Password>
            </BasicAuthentication>
            <PublicKey>LS0[...]LS0tLQ0K</PublicKey>
        </CallbackCredentials>
    </CallbackSubscription>
</Subscription>
```

{% endextendmethod %}

## Searching For Phone Numbers {#search-for-phone-numbers}
Finding numbers can be achieved by searching the Bandwidth inventory.

This step is optional – the telephone numbers can be ordered directly using search criteria, but if there is need to examine the numbers before activating them on the account, the search can be used to return a list of available numbers.

There are a number of search approaches that can be used; the NPA NXX search is used for this example.  Please see the [API documentation](https://test.dashboard.bandwidth.com/apidocs/) for the other applicable search types.

### Base URL
<code class="get">GET</code>`https://dashboard.bandwidth.com/api/accounts/{{accountId}}/availableNumbers`

{% extendmethod %}

### Query Parameters

| Parameters                       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|:---------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `areaCode`                       | The allowed number ranges are [2-9] for the first digit and [0-9] for both the second and third digits.                                                                                                                                                                                                                                                                                                                                                                  |
| `npaNxx`<br> -or- <br> `npaNxxx` | NPA NXX combination to be searched. <br> - Valid npa values:[2-9] for the first digit, and [0-9] for both the second and third digits. <br> - Valid Nxx values:[2-9] for the first digit, and [0-9] for both the second and third digits. <br> - Valid x values [0-9].                                                                                                                                                                                                   |
| `rateCenter`                     | The abbreviation for the Rate Center                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `state`                          | The two-letter abbreviation of the state the RateCenter is in.                                                                                                                                                                                                                                                                                                                                                                                                           |
| `city`                           | The name of the city.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `zip`                            | A five-digit (XXXXX) or nine-digit (XXXXX-XXXX) zip-code value.                                                                                                                                                                                                                                                                                                                                                                                                          |
| `lata`                           | A maximum five digit (XXXXX) numeric format.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `localVanity`                    | Requested vanity number. Valid range is from 4 to 7 alphanumeric characters                                                                                                                                                                                                                                                                                                                                                                                              |
| `tollFreeVanity`                 | The Toll Free requested vanity number. Valid range is from 4 to 7 alphanumeric characters                                                                                                                                                                                                                                                                                                                                                                                |
| `tollFreeWildCardPattern`        | The requested Toll Free 3 digit wild card pattern. Examples: `8**`, `80*`, `87*`, etc.                                                                                                                                                                                                                                                                                                                                                                                   |
| `quantity`                       | The desired quantity of requested numbers. Values range from 1-5000. If no quantity is specified, the default of 5000 is returned.                                                                                                                                                                                                                                                                                                                                       |
| `enableTNDetail`                 | If set to true, a list of details associated with the telephone number (rate center abbreviation, rate center host city, state, and LATA) will be displayed along with the telephone number. This value is set to false by default.                                                                                                                                                                                                                                      |
| `LCA`                            | Local Calling Area. If this parameter is specified then Telephone Numbers that are likely in the Local Calling Area of the stated Rate Center, NPANXX or NPANNXX will be returned, in addition to those that *exactly* match the query will be returned. Since LCA logic is not always symmetric and not always inclusive of RC and NPANXX search criteria, this result reflects somewhat of an approximation. The parameter value is true or false. The default is true |
| `endsIn`                         | Intended to use with localVanity only. The parameter value is true or false. If set to true, the search will look for only numbers which end in specified localVanity, otherwise localVanity sequence can be met anywhere in last 7 number digits. The default is false.                                                                                                                                                                                                 |
| `orderBy`                        | The field by which the returned numbers will be sorted. Only supported if at least one of the following search criteria is specified: npaNxx or npaNxxx, rateCenter, city, zip, tollFreeVanity, tollFreeWildCardPattern. Allowed values are fullNumber, npaNxxx, npaNxx, and areaCode>                                                                                                                                                                                   |
| `protected`                      | A query parameter, that governs, how the Protected status of numbers impacts the search results: <br> - `None` : The numbers returned in the payload will not contain any numbers that are tagged as Protected <br> - `ONLY` :The numbers returned in the payload will all be tagged as Protected. No "unProtected" numbers will be returned <br> - `MIXED` : The protected status of the numbers will be ignored in the search - all types of numbers will be returned  |

{% common %}

### Example: Search by NPA NXX

```http
GET https://dashboard.bandwidth.com/api/accounts/{{accountId}}/availableNumbers?npaNxx=540551&quantity=7 HTTP/1.1
Content-Type: application/xml; charset=utf-8
Authorization: {user:password}
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/xml; charset=utf-8

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<SearchResult>
    <ResultCount>7</ResultCount>
    <TelephoneNumberList>
        <TelephoneNumber>5405514342</TelephoneNumber>
        <TelephoneNumber>5405515330</TelephoneNumber>
        <TelephoneNumber>5405515329</TelephoneNumber>
        <TelephoneNumber>5402278098</TelephoneNumber>
        <TelephoneNumber>5402270905</TelephoneNumber>
        <TelephoneNumber>5402278089</TelephoneNumber>
        <TelephoneNumber>5402278090</TelephoneNumber>
    </TelephoneNumberList>
</SearchResult>
```

{% endextendmethod %}

## Order Phone Numbers {#order-phone-numbers}

To successfully order a Phone Number that was previously returned in a search on the `/availableNumbers` you will create a `<ExistingTelephoneNumberOrderType>` with a `<TelephoneNumberList>` containing at least 1 phone number.

### Base URL
<code class="post">POST</code>`https://dashboard.bandwidth.com/api/accounts/{{accountId}}/orders`

{% extendmethod %}

### Common Request Parameters

| Parameter                             | Required | Description                                                                                                                                                                                                                                                                                                                                          |
|:--------------------------------------|:---------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Name`                                | Yes      | The name of the order. Max length restricted to 50 characters.                                                                                                                                                                                                                                                                                       |
| `CustomerOrderId`                     | No       | Optional value for Id set by customer                                                                                                                                                                                                                                                                                                                |
| `SiteId`                              | Yes      | The ID of the Site (_or sub-account_) that the SIP Peer is to be associated with.                                                                                                                                                                                                                                                                    |
| `PeerId`                              | No       | The ID of the SIP Peer (_or location_) that the telephone numbers are to be assigned to. <br> <br> ⚠️ The `PeerId` **MUST** already exist under the `Site` (_or sub-account_) EX: `/accounts/{accountId}/sites/{siteId}/sippeers/{PeerId}`                                                                                                           |
| `PartialAllowed`                      | No       | By default all order submissions are fulfilled partially. Setting the `PartialAllowed` to false would trigger the entire order to be fulfilled (any error encountered such as 1 TN not being available would fail all TNs in the order) <br><br> Default: `false`                                                                                    |
| `BackOrderRequested`                  | No       | `BackOrderRequested` will indicate to the system that if the entire quantity of numbers is not available on the first attempt to fill the new number order, the request will be repeated periodically until the request is successful or canceled. <br> `true` - Backorder numbers if the entire quantity is not available <br><br> Default: `false` |
| `TelephoneNumberList`                 | Yes      | A list of telephone numbers to order.                                                                                                                                                                                                                                                                                                                |
| `TelephoneNumberList.TelephoneNumber` | Yes      | `TelephoneNumberList` must have at least one `TelephoneNumber` to order                                                                                                                                                                                                                                                                              |

{% common %}

### Example: Order 1 Number Returned in the Search above

```http
POST https://dashboard.bandwidth.com/api/accounts/{{accountId}}/orders HTTP/1.1
Content-Type: application/xml; charset=utf-8
Authorization: {user:password}

<Order>
    <CustomerOrderId>123456789</CustomerOrderId>
    <Name>My Custom Name or UUID of a customer session</Name>
    <ExistingTelephoneNumberOrderType>
        <TelephoneNumberList>
            <TelephoneNumber>5402278098</TelephoneNumber>
        </TelephoneNumberList>
    </ExistingTelephoneNumberOrderType>
    <SiteId>743</SiteId>
</Order>
```

### Response

```http
HTTP/1.1 201 Created
Content-Type: application/xml; charset=utf-8
Location: https://dashboard.bandwidth.com/api/accounts/{{accountId}}/orders/c9dg306c-gdh7-77j7-812c-0520c8f49123

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<OrderResponse>
    <Order>
        <CustomerOrderId>1234567896</CustomerOrderId>
        <Name>Existing Number Order 3</Name>
        <OrderCreateDate>2018-03-20T19:51:20.886Z</OrderCreateDate>
        <BackOrderRequested>false</BackOrderRequested>
        <id>c9dg306c-gdh7-77j7-812c-0520c8f49123</id>
        <ExistingTelephoneNumberOrderType>
            <TelephoneNumberList>
                <TelephoneNumber>5402278098</TelephoneNumber>
            </TelephoneNumberList>
        </ExistingTelephoneNumberOrderType>
        <PartialAllowed>true</PartialAllowed>
        <SiteId>13606</SiteId>
    </Order>
    <OrderStatus>RECEIVED</OrderStatus>
</OrderResponse>
```

{% endextendmethod %}

## Receive HTTP Callback with Order Status {#receive-callback}

The Bandwidth Phone Number API will send a HTTP Callback (webhook) to the URL specified in the `<URL>...</URL>` when [creating the subscription](#create-subscription).

The HTTP Callback will contain information if the order was successful or failed.  If status is **anything other than `COMPLETE`** the order has failed.  The most likely scenario is that another customer ordered the desired phone number between the time 'searched' and 'ordered'.  If the order is **not** `COMPLETE`, either try ordering a different phone number, or [search for more numbers](#search-for-phone-numbers).

{% extendmethod %}

### Callback Request Parameters

| Parameter                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|:--------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Status`                                    | Following this tutorial should only yield two possible `Status` values: <br> - `COMPLETE` : The order succeeded and all phone numbers requested were ordered and will be sent in the `<CompletedTelephoneNumbers>` list. <br> - `FAILED` : The order _did not_ succeed (at least 1 phone number sent in the order was unable to be ordered). <br><br> To learn more about the order states, see the [Advanced Ordering Overview](advancedOrdering.md#ordering-overview) |
| `SubscriptionId`                            | The unique Id associated with the subscription that was configured for `orders`. This is the same value that is returned in `Location` Header when [creating the subscription](#create-subscription).                                                                                                                                                                                                                                                                   |
| `Message`                                   | A specific message related to the order.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `OrderId`                                   | The unique Id associated with the order. This is the same value that is returned in the `Location` Header when [creating the order](#order-phone-numbers)                                                                                                                                                                                                                                                                                                               |
| `OrderType`                                 | The specific type of order that was created. <br> For this example, the value will be `orders`. <br><br>For more information see [managing subscriptions](managingSubscriptions.md).                                                                                                                                                                                                                                                                                    |
| `CompletedTelephoneNumbers`                 | Contains the list of Phone Numbers that were attempted to be ordered.                                                                                                                                                                                                                                                                                                                                                                                                      |
| `CompletedTelephoneNumbers.TelephoneNumber` | The actual Phone Number that was attempted to be ordered.                                                                                                                                                                                                                                                                                                                                                                                                                  |

{% common %}

### Example: Successful Phone Number Order

```http
POST https://your-server.com HTTP/1.1
Content-Type: application/xml; charset=utf-8
Authorization: {user:password}

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Notification>
    <Status>COMPLETE</Status>
    <SubscriptionId>d8274fa6-e49b-469a-93bf-c7dd17615950</SubscriptionId>
    <Message>Created a new number order for 1 number from WASHINGTON, VA</Message>
    <OrderId>a8ef295b-fcf4-44a7-945c-0520c8f49123</OrderId>
    <OrderType>orders</OrderType>
    <CompletedTelephoneNumbers>
        <TelephoneNumber>5402278098</TelephoneNumber>
    </CompletedTelephoneNumbers>
</Notification>
```

### Example: Failed Phone Number Order

```http
POST https://your-server.com HTTP/1.1
Content-Type: application/xml; charset=utf-8
Authorization: {user:password}

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Notification>
    <Status>FAILED</Status>
    <SubscriptionId>333643f0-3eac-4c8f-8b15-c712f49e09fe</SubscriptionId>
    <Message>5005: The telephone number is unavailable for ordering</Message>
    <OrderId>4a58b348-892c-4426-8900-97fc4555c42c</OrderId>
    <OrderType>orders</OrderType>
    <CompletedTelephoneNumbers>
       <TelephoneNumber>5402278098</TelephoneNumber>
    </CompletedTelephoneNumbers>
</Notification>
```

{% endextendmethod %}

