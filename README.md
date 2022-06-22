# Overview

This API allows you to search properties from our vast range of footprints data. 

# Guides

The API gets authenticated using AWS IAM. Here are the different ways to hit the API. 

## Hitting API in Postman
1. In Postman, on the Authorization tab, do the following:
- For Type, choose AWS Signature.
- For AccessKey and SecretKey, enter your IAM access key ID and secret access key.

2. In the Request URL field, paste your API's invoke URL which is https://api.geox-ai.com/api/v2/platform/parcels
3. Select the POST HTTP method
4. Now go to the Body section and select the raw button then from the format dropdown select the JSON as your body format.
5. Now put your lat/lng in the following format
```json
{
    "locations": [
        {
            "lat": 33.4469749,
            "lng": -86.8209358,
            "corellationId": "a correlation id"
        }
    ]
}
```
6. Finally, hit the API.

The locations key in the Requst body is a list which can contain multiple lat/lng values simultaneously. 
The corellationId will be sent back in the response along with the resposne of corresponding lat/lng provided.  

### Postman Sample call
![Postman](postman_example_new.png)

### Note 
To manually authenticate your requests using another tool or environment,
use the [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html).
For more information, see [Signing requests](https://docs.aws.amazon.com/apigateway/api-reference/signing-requests/).

## Hitting API with Python
1. We will need following libraries to be installed
   - requests
   - aws_requests_auth
### Python code for API Call with Post Method
```python
import json
import requests
from aws_requests_auth.aws_auth import AWSRequestsAuth


def m2m_request(access_key, secret_key, lat, lng):
    api_url = "https://api.geox-ai.com/api/v2/platform/parcels"
    aws_details = {
        'aws_access_key': access_key,
        'aws_secret_access_key': secret_key,
        'aws_host': "api.geox-ai.com",
        'aws_region': "us-east-1",
        'aws_service': "execute-api"
    }
    auth = AWSRequestsAuth(**aws_details)
    request_body = {
        "locations": [
            {
                "lat": lat,
                "lng": lng,
                "corellationId": "1e6a747f11f16540a27e"
            },
            # more lat/lng values can be added here
        ]
    }
    res = requests.post(api_url, auth=auth, json=request_body)
    assert res.status_code == 200, f"Request failed with status: {res.status_code}"
    res_data = res.json()
    return res_data


if __name__ == '__main__':
    m2m_response = m2m_request("YOUR_API_KEY", "YOUR_API_SECRET",
                               45.6696163800542, -122.5038830583887)
    print(json.dumps(m2m_response, indent=2))
```

## Hitting API with cURL
The API request needs to be signed with AWS Signature Version 4. Please follow this [link](https://docs.aws.amazon.com/general/latest/gr/sigv4-signed-request-examples.html) for more details. 
```shell
curl --location --request POST 'https://api.geox-ai.com/api/v2/platform/parcels' \
--header 'Accept-Encoding: application/gzip' \
--header 'X-Amz-Content-Sha256: beaead3198f7da1e70d03ab969765e0821b24fc913697e92XXXXXXXXXXXXXXXX' \
--header 'X-Amz-Date: 20220622T064739Z' \
--header 'Authorization: AWS4-HMAC-SHA256 Credential=AKIA2TITFGXXXXXXXXXX/20220622/us-east-1/execute-api/aws4_request, SignedHeaders=accept-encoding;host;x-amz-content-sha256;x-amz-date, Signature=7982cac59526dc4c6915244d110ca6cc12d3a03e6588b086XXXXXXXXXXXXXXXX' \
--header 'Content-Type: application/json' \
--data-raw '{
    "locations": [
        {
            "lat": 33.4469749,
            "lng": -86.8209358,
            "corellationId": "1e6a747f11f16540a27e"
        }
    ]
}'
```

## Hitting API with wget
```shell
wget --no-check-certificate --quiet \
  --method POST \
  --timeout=0 \
  --header 'Accept-Encoding: application/gzip' \
  --header 'X-Amz-Content-Sha256: beaead3198f7da1e70d03ab969765e0821b24fc913697e92XXXXXXXXXXXXXXXX' \
  --header 'X-Amz-Date: 20220622T065830Z' \
  --header 'Authorization: AWS4-HMAC-SHA256 Credential=AKIA2TITXXXXXXXXXXXX/20220622/us-east-1/execute-api/aws4_request, SignedHeaders=accept-encoding;host;x-amz-content-sha256;x-amz-date, Signature=b797c5a5c5fe9295067c83b2d3585e7a4362f26d8bfaXXXXXXXXXXXXXXXXXXXX' \
  --header 'Content-Type: application/json' \
  --body-data '{
    "locations": [
        {
            "lat": 33.4469749,
            "lng": -86.8209358,
            "corellationId": "1e6a747f11f16540a27e"
        }
    ]
}' \
   'https://api.geox-ai.com/api/v2/platform/parcels'
```

# Request and Response Samples
Here are the sample request and response

## Request URL
```shell
https://api.geox-ai.com/api/v2/parcels
```

## Request Body Sample
```json
{
    "locations": [
        {
            "lat": 33.4469749,
            "lng": -86.8209358,
            "corellationId": "1e6a747f11f16540a27e"
        }
    ]
}
```

## Response Sample
```json
{
    "data": [
        {
            "corellationId": "1e6a747f11f16540a27e",
            "results": [
                {
                    "footprint_count_p": "1",
                    "solar_panel_area_b": null,
                    "air_conditioner_count_b": null,
                    "parcel_area": "336505.7262927573",
                    "temporary_pool_area_p": null,
                    "roof_type": "flat",
                    "pool_area_p": null,
                    "footprint_over_p": "0.47",
                    "pool_count_p": null,
                    "footprint_area_p": "1571.0940800277797",
                    "skylight_count_b": null,
                    "dis_coast_b": "-1",
                    "dis_water_body_b": "2.1262471890782795",
                    "footprint_area": "1571.0469118150065",
                    "rust_area_b": null,
                    "ponding_area_b": null,
                    "tarp_area_b": null,
                    "parking_lot_area_p": null,
                    "sport_field_area_p": null,
                    "chimney_count_b": null,
                    "trampoline_count_p": null,
                    "air_conditioner_area_b": null,
                    "roof_condition": "good",
                    "parking_lot_over_p": null,
                    "tank_area_p": null,
                    "slope_area_b": "719.8874491111119",
                    "n_floors": null,
                    "building_height": null,
                    "roof_material": "concrete",
                    "number_of_floors": null
                },
                {
                    "footprint_count_p": "1",
                    "solar_panel_area_b": "121.52406250694459",
                    "air_conditioner_count_b": "81",
                    "parcel_area": "336505.7262927573",
                    "temporary_pool_area_p": null,
                    "roof_type": "flat",
                    "pool_area_p": null,
                    "footprint_over_p": "0.47",
                    "pool_count_p": null,
                    "footprint_area_p": "1571.0940800277797",
                    "skylight_count_b": "70",
                    "dis_coast_b": "-1",
                    "dis_water_body_b": "1.9416754592802998",
                    "footprint_area": "339361.6067604386",
                    "rust_area_b": null,
                    "ponding_area_b": null,
                    "tarp_area_b": "123.89211332638904",
                    "parking_lot_area_p": null,
                    "sport_field_area_p": null,
                    "chimney_count_b": null,
                    "trampoline_count_p": null,
                    "air_conditioner_area_b": "4441.817505236117",
                    "roof_condition": "damaged",
                    "parking_lot_over_p": null,
                    "tank_area_p": null,
                    "slope_area_b": "1836.3157718055577",
                    "n_floors": null,
                    "building_height": 28.25,
                    "roof_material": "concrete",
                    "number_of_floors": "3"
                }
            ]
        }
    ],
    "msg": "OK",
    "status": 200
}
```

# API Reference
Please visit this [link](https://api.geox-ai.com/api/v2/redoc) for more detailed description about API response attributes
