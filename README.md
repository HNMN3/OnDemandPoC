# Overview

This API allows you to search properties from our vast range of footprints data. 

# Guides

The API gets authenticated using AWS IAM. Here are the different ways to hit the API. 

## Hitting API in Postman
1. In Postman, on the Authorization tab, do the following:
- For Type, choose AWS Signature.
- For AccessKey and SecretKey, enter your IAM access key ID and secret access key.

2. In the Enter request URL field, paste your API's invoke URL.
3. Select the appropriate Request Method.
4. Put your lat/lng in the Params section.
5. Finally, hit the API.

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
2. Here is the python code to hit the API (Get request)
```python
import requests
from aws_requests_auth.aws_auth import AWSRequestsAuth


def m2m_request(access_key, secret_key, lat, lng):
    api_url = "https://api.geox-ai.com/poc/api/"
    aws_details = {
        'aws_access_key': access_key,
        'aws_secret_access_key': secret_key,
        'aws_host': "api.geox-ai.com",
        'aws_region': "us-east-1",
        'aws_service': "execute-api"
    }
    auth = AWSRequestsAuth(**aws_details)
    params = {
        "lat": lat,
        "lng": lng,
    }
    res = requests.get(api_url, auth=auth, params=params)
    assert res.status_code == 200, f"Request failed with status: {res.status_code}"
    res_data = res.json()
    return res_data


if __name__ == '__main__':
    m2m_request("YOUR_API_KEY", "YOUR_API_SECRET", "LATITUDE", "LONGITUDE")
```
3. Here is the python code to hit the API (Post request)
```python
import requests
from aws_requests_auth.aws_auth import AWSRequestsAuth


def m2m_request(access_key, secret_key):
    api_url = "https://api.geox-ai.com/poc/api/"
    aws_details = {
        'aws_access_key': access_key,
        'aws_secret_access_key': secret_key,
        'aws_host': "api.geox-ai.com",
        'aws_region': "us-east-1",
        'aws_service': "execute-api"
    }
    auth = AWSRequestsAuth(**aws_details)

    data = {
        "locations": [
            {
                "lat": 30.74716231727497,
                "lng": -95.61527307112493,
                "corellationId": "unique_id"
            },
            # ... more locations
        ]
    }

    res = requests.post(api_url, json=data, auth=auth)
    assert res.status_code == 200, f"Request failed with status: {res.status_code}"
    res_data = res.json()
    return res_data


if __name__ == '__main__':
    m2m_request("YOUR_API_KEY", "YOUR_API_SECRET")
```
## Hitting API with cURL
The API request needs to be signed with AWS Signature Version 4. Please follow this [link](https://docs.aws.amazon.com/general/latest/gr/sigv4-signed-request-examples.html) for more details. 
```shell
curl --location --request GET 'https://api.geox-ai.com/poc/api/?lat=32.866004&lng=-96.652901' \
--header 'X-Amz-Date: 20211124T102508Z' \
--header 'Authorization: AWS4-HMAC-SHA256 Credential=XXXXXXXXXX/20211124/us-east-1/execute-api/aws4_request, SignedHeaders=host;x-amz-date, Signature=XXXXXXXXXXXXXXXXXXXXXXXX' \
--data-raw ''
```

# Request and Response Samples
Here are the sample request and response

## Request Sample
```shell
https://api.geox-ai.com/poc/api/?lat=32.866004&lng=-96.652901
```

## Response Sample
```json
{
    "data": {
        "results": [
            {
                "state_addr": "washington",
                "gx_key_bu": "2ff8f064-786c-470f-be7a-4db177516bdb",
                "footprint_count_p": "2",
                "solar_panel_area_b": "318.50283521527814",
                "air_conditioner_count_b": null,
                "parcel_area": "6022.168511194452",
                "temporary_pool_area_p": "108.06922830555568",
                "roof_type": "Gable",
                "tree_over_b": null,
                "pool_area_p": null,
                "footprint_over_p": "37.31",
                "tree_area_b": null,
                "pool_count_p": null,
                "footprint_area_p": "2246.6343956111136",
                "skylight_count_b": null,
                "dis_coast_b": "37.032741789772665",
                "dis_water_body_b": "0.217964150883838",
                "footprint_area": "2136.646393732116",
                "rust_area_b": null,
                "ponding_area_b": null,
                "tarp_area_b": null,
                "parking_lot_area_p": null,
                "trampoline_count_p": null,
                "air_conditioner_area_b": null,
                "roof_condition": "good",
                "roof_condition_score": null,
                "roof_type_score": null,
                "geom_id_p": "123378.0",
                "table_name_p": "wa_clark"
            }
        ]
    },
    "msg": "OK",
    "status": 200
}
```

# API Reference
Please visit this [link](https://db8sxr6e96.execute-api.us-west-1.amazonaws.com/api/docs) for API reference
