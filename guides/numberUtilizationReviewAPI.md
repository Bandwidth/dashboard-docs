# Step By Step Guide To Number Assignment Using the Bandwidth Dashboard REST API (Limited Availability)

### Bulk assigning up to 5000 TNs with an active end-user

The Bandwidth Dashboard REST API Documentation can be found [here](../apiReference.md). The API endpoint for using the Number Utilization Review is /accounts /{accountId} /numbersAssignment.

The POST method on this endpoint is used to bulk assign/unassign your owned numbers. After retrieving and auditing your phone numbers, you can use this endpoint to bulk assign/unassign your numbers. A sample request and response can be found below

Request
```
curl -d @phoneNumberFile.xml -u {username}:{password} -X POST https://dashboard.bandwidth.com/api/accounts/{accountId}/numbersAssignment
```

Here's an example file for your phoneNumberFile.xml parameter

```
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<TelephoneNumbersAssignmentOrder>
    <CustomerOrderId>12345</CustomerOrderId>
    <Action>[ ASSIGN | UNASSIGN ]</Action>
    <TelephoneNumbers>
        <TelephoneNumber>9199918388</TelephoneNumber>
        <TelephoneNumber>4158714245</TelephoneNumber>
        <TelephoneNumber>4352154439</TelephoneNumber>
        <TelephoneNumber>4352154466</TelephoneNumber>
    </TelephoneNumbers>
</TelephoneNumbersAssignmentOrder>
```

Response
```

```
