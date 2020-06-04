
# Manual usage of the Mila API

## Setup vars

```bash
export API_KEY=<api-key>
export ENDPOINT=https://api.mila.cloud/int
```

## Get Packages

```
curl -H "x-api-key: $API_KEY" $ENDPOINT/service-packages?enterpriseSlugName=mila-ch
```

Response:

The id's are the package id's to be used for booking service calls:

```json
[
    {
        "id": "57bc6064-6c75-4512-adac-2bfaa837c783",
    }
]

```

## Terms and conditions

```bash
curl -H "x-api-key: $API_KEY" "$ENDPOINT/terms-and-conditions?country=ch&language=en"
```

Response:

```json
{
    "legalRegionId": "fb581c89-97d7-4df1-807a-2b07433bdf11",
    "id": "5c48a721-d495-407e-b566-1bd26d9f0c77",
    "body": "..."
}
```

## Book service calls

### Stage / Integration package

Book a service call with data from `data/booking-data-stage.json` or `data/booking-data-stage-OTTO.json`:

```bash
export EXAMPLE_API_CALL_DATA=data/booking-data-stage.json
export EXAMPLE_API_CALL_DATA=data/booking-data-stage-OTTO.json
```

### Production package

Book a service call with data from `data/booking-data-prod.json` or `data/booking-data-prod-OTTO.json`:

```bash
export EXAMPLE_API_CALL_DATA=data/booking-data-prod.json
export EXAMPLE_API_CALL_DATA=data/booking-data-prod-OTTO.json
```

### Execute the service call API call


```bash
curl -X POST -H "x-api-key: $API_KEY" -H "Content-Type: application/json" -d @$EXAMPLE_API_CALL_DATA $ENDPOINT/service-calls $ENDPOINT/service-calls
```

*Attention:* `serviceCallDate` is in Zulu Time (UTC, example: `019-07-16T15:00:00.000Z` is `2019-07-16T17:00:00+02:00 Europe/Berlin`). The date has to be at least 24hours ahead of the current datetime 


Response:

```json
{
  "hid": "13DD89",
  "id": "13dd8994-1e91-4c8c-8c66-59f95e6af7c7"
}
```

### Business Model PREPAID_INVOICED_TO_ENTERPRISE_PARTNER [OTTO etc.]

*Example*: 
```
{
    "servicePackageOfferingIds": ["d3960c83-eda3-46dd-8dbd-7fb1d86b4e2f"], // ⚠️ SPO has to be provided by the enterprise partner with 
    "email": "customeremail@somemailprovider.com",
    "communicationLanguage": "en",  // required
    "address": { // ⚠️ required, invoiceadress to be skipped instead; need to be valid as Google Maps checks correctness
    "firstName": "First",
    "lastName": "Last",
    "city": "City",
    "postalCode": "10115",
    "phoneNumber": "+491605551769",
    "street": "Strasse",
    "streetNumber": "1",
    "country": "DE" // ⚠️
    },
    "serviceCallData": { // optionally, some internal service call data can be also provided and saved by us
        "origin": "EPName", // enterprise partner name
        "enterprisePartnerOrderId": "34235234" // internal order Id
    },
    "termsAndConditionsId": "caf18cf4-acb4-482a-b6af-b3dc502692db", // ⚠️ should not be required but we got some errors, this is German T&C for customers without cancellation but any valid T&C is ok
    "legalRegionId": "2e0eb199-5e96-4642-aeaa-0fec252eaab9", // ⚠️ required, this is Germany in Prod
    "businessModel": "PREPAID_INVOICED_TO_ENTERPRISE_PARTNER" // ⚠️⚠️⚠️ Without this, the subsequent steps in the booking are wrong (change of price, invoice address, etc.)
    }
```
