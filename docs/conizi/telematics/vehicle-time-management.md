# conizi Vehicle Time Management Endpoint (v1)
The conizi Advanced ETA Prognosis-Timeline endpoint will listen to:

* Production: https://conizi.io/api/telematics/v1/Prognosis
* Preproduction: https://preproduction.dev.conizi.io/api/telematics/v1/Prognosis
* Staging: https://staging.dev.conizi.io/api/telematics/v1/Prognosis

# API Documentation

* Production: https://conizi.io/api/telematics/swagger/index.html
* Preproduction: https://preproduction.dev.conizi.io/api/telematics/swagger/index.html
* Staging: https://staging.dev.conizi.io/api/telematics/swagger/index.html


# Authenticate via conizi

## Token Endpoints conizi
* Production: https://id.conizi.io/connect/token
* Preproduction: https://id.preproduction.dev.conizi.io/connect/token
* Staging: https://id.staging.dev.conizi.io/connect/token

## Needed Scopes
* ocora.telematics.read

## Token Request
    curl --location --request POST 'https://id.staging.dev.conizi.io/connect/token' \
    --header 'Content-Type: application/x-www-form-urlencoded' \
    --header 'Content-Type: application/json' \
    --data-urlencode 'grant_type=client_credentials' \
    --data-urlencode 'scope=ocora.telematics.read' \
    --data-urlencode 'client_id=${conizi_app_id}' \
    --data-urlencode 'client_secret=${conizi_app_secret}' \
    --data-urlencode 'response_type=token'

## Prognosis Request
*Example with vehicleId*

    curl --location --request GET 'http://conizi.localtest.me:8002/api/telematics/v1/Prognosis/vehicle/{vehicleId}/{timestamp}/{calculationType}' \ --header 'Authorization: Bearer ${token}'
calculationType can be used as string or number (Optimistic/0)

## Prognosis Response
    {
        "DriverActivityPrognosis": [
            {   //First Entry is always the LastActivity until {ReceivedDateTime}
                "Duration": "00:04:16", 
                "ActivityType": "Resting"
            },
            {   
                "Duration": "00:25:44",
                "ActivityType": "Resting"
            },
            {
                "Duration": "00:14:00",
                "ActivityType": "Driving"
            },
            {
                "Duration": "00:15:00",
                "ActivityType": "Resting"
            },
            {
                "Duration": "02:50:12",
                "ActivityType": "Driving"
            }
        ],
        "ReceivedDateTime": "2020-02-27T14:41:23.229Z",
        "NextReceivingDateTime": "2020-02-27T14:56:09.038Z"
    }

