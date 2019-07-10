
# Manual usage of the Mila API

## Setup vars

```bash
export API_KEY=<api-key>
export ENDPOINT=https://api.mila.cloud/staging
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

*Attention:* `serviceCallDate` is in Zulu Time. (Example: "2019-07-16T15:00:00.000Z" is 2019-07-16T17:00:00+02:00 Europe/Berlin)

Book a service call with data from `data/booking-data.json`:

```bash
curl -X POST -H "x-api-key: $API_KEY" -H "Content-Type: application/json" -d @data/booking-data.json $ENDPOINT/service-calls
```

Response:

```json
{
  "hid": "13DD89",
  "id": "13dd8994-1e91-4c8c-8c66-59f95e6af7c7"
}
```
