---
title: LogiNext API

language_tabs:
  - json

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

LogiNext welcomes you to the world of organised logistics.

Using the LogiNext API, you can integrate all the segments of your delivery logistics and supply chain network into our product platform to create seamless experience for your operations and executive team.

The LogiNext API is designed to allow our client partners to add resources, shipments, plan a route, start the trip, track and follow the updates till the trip is completed and shipment is delivered at the desired location.

# Requests

The base URL for all requests to the LogiNext API is:

https://api.loginextsolutions.com/

Our API is REST-based. This means:

1. It make use of standard HTTP verbs like GET, POST, DELETE.

2. The API uses standard HTTP error responses to describe errors.

3. Authentication is specified with HTTP Basic Authentication.

Also,the input dates like 2016-07-01T11:18:00.000Z are accepted in Coordinated Universal Time (UTC) format.

### Request Headers

Header | Sample Value | Description
--------- | ------- | -------------
Content-Type | application/json | JSON request
WWW-Authenticate | BASIC 51bbe3f7-1671-476c-818a-e7fbbca10202 | Authentication token
CLIENT_SECRET_KEY | $2a$08$LQEqG3s.LF2jBt7Baq| Authentication key

### Versioning

Versioning allows us to provide developers a consistent experience. All endpoints are prefixed with a version such as /v1. This version refers to the overall layout of the endpoints and response standards.

# Responses

The LogiNext API uses HTTP status codes to indicate the status of your requests. This includes Success and Error codes.

Code | Description
---------- | -------
200 | Success -- Your request is successfully processed.
201 | Created -- Your requested resource got successfully created.
400 | Bad Request -- Your request is incorrect.
401 | Unauthorized -- Your API key is wrong.
404 | Not Found -- The specified resource could not be found.
405 | Method Not Allowed -- You tried to access a resource with an invalid method.
409 | Conflict -- Your request could not be completed due to a conflict with the current state of the target resource.
415 | Unsupported Media Type -- Your request is not in the format accepted by this method of target resource.
429 | Too Many Requests -- You're requesting too many resources.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.

# Authentication

##Authenticate

LogiNext API uses Basic Authentication to provide you an authorized access. Please use the the below URL Endpoint to authenticate yourself as a user of this API.

You will have to pass the username and password which is provided to you either by our system (in case of auto sign-up) or by our assigned CSAs(Customer Service Associate).

The response will contain a session token and a Client Secret key, which is unique in relation to every specific customer. The validity of this session token is 1 day (24 hours).
Please ensure that you add the token and secret key as part of every Loginext API call.

> Definition

```json
https://api.loginextsolutions.com/LoginApp/login/authenticate?username=<username>&password=<password>
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": null,
  "hasError": false
}
```

LogiNext uses Basic Authentication to provide authorized access to its API.

Use the following URL endpoint to authenticate yourself as a user of this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/LoginApp/login/authenticate?username=<username>&password=<password>`

### Request Headers

Header | Sample Value | Description
--------- | ------- | -------------
Content-Type | application/json | JSON request

### Request Parameters

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
username | String | Mandatory | Username provided by LogiNext
password | String | Mandatory | Password provided by LogiNext


### Response Headers

Header | Sample Value
--------- | -------
WWW-Authenticate | BASIC 075b8961-bd02-454c-83eb-259f965f313f
CLIENT_SECRET_KEY | $2a$08$bCi0ja4B5S02BKQt3VdxNuReERpSV8SiAbwVrHNyhC7mD

##Invalidate

You can fetch a fresh session token and a key by calling the below API. This call will invalidate the existing token and key and then you will be provided with a new token and a key which you will have to pass everytime in every other API call.

> Definition

```json
https://api.loginextsolutions.com/LoginApp/login/token/refresh
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": null,
  "hasError": false
}
```

This endpoint invalidates a user.

### Request

<span class="post">GET</span>`https://api.loginextsolutions.com/LoginApp/login/token/refresh`


### Request Headers

Header | Sample Value
--------- | ------- | -------------
WWW-Authenticate | BASIC 075b8961-bd02-454c-83eb-259f965f313f
CLIENT_SECRET_KEY | $2a$08$bCi0ja4B5S02BKQt3VdxNuReERpSV8SiAbwVrHNyhC7mD

### Response Headers

Header | Sample Value
--------- | -------
WWW-Authenticate | BASIC 075b8961-bd02-454c-83eb-259f965f313f
CLIENT_SECRET_KEY | $2a$08$bCi0ja4B5S02BKQt3VdxNuReERpSV8SiAbwVrHNyhC7mD


# Haul


1. Once the resources are created, then you can create trips by calling Create Trips API. You need to provide the Unique trip name along with the Origin and Destination Address details and the Journey date. The acknowledgement consists of the Reference ID for each of the trips created which needs to be stored in your system for future references.
Please check with our assigned CSAs on the address format based on the model type configured for you as either the Pin Code or Hub to Hub.

2. Further you can mark the trip as started by calling the Start Trip API and mark the same trip as stopped by calling Stop Trip API. In both these API you will have to pass the trip reference ID.

3. Finally you can track your vehicle in transit through the Track Last Location API. in this case also you need to pass the Trip Reference ID.

## Create Vehicle

For Haul and Mile products, you can create and maintain details of the vehicles in the LogiNext system using Create Vehicle API. This API requires vehicle’s primary information (mandatory - Vehicle Number) and returns a "Reference Id" which you need to store in order to update the vehicle information in future. 

> Definition

```json
https://api.loginextsolutions.com/VehicleApp/v1/vehicle/create
```


> Request Body

```json
[
	{
    		"vehicleNumber":"MH-111",
    		"vehicleMake":"2015",
   		    "vehicleModel":"OMNI",
    		"typeOfBody":"Flat Bed",
    		"unladdenWeight":10,
    		"capacityInWeight":10,
    		"capacityInUnits":10,
    		"capacityInVolume":10,
    		"chasisNumber":"CHASIS-123",
    		"engineNumber":"A-12353-D1234",
    		"markerName":"Marker-1",
    		"registrationNumber":"",
    		"pucValidity":"2016-07-01T11:18:00.000Z",
    		"insuranceValidity":"2016-07-01T11:18:00.000Z",
    		"vehiclePermit":"Andhra Pradesh,Delhi",
    		"ownership":"company",
    		"ownerName":"ABC",
    		"transporter":"",
    		"financer":"",
    		"accidentHistory":"",
    		"rentStartDate":null,
    		"rentEndDate":null,
    		"deviceId":{
        		"barcode":"LN12345678"
    		}
    }
]  
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": [
			{
				"vehicleNumber":"MH-111",
				"referenceId":"a9be39b9347911e6829f000d3aa04450"
			}
	   ],
  "hasError": false
}
```

Create a new vehicle by passing form data through json.

The acknowledgement will provide the vehicle number and reference ID.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/VehicleApp/v1/vehicle/create`


### Request Parameters

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
vehicleNumber | String | Mandatory | Unique name to identify the vehicle
vehicleMake | String | Optional | Make of the vehicle like Mazda, Volvo, etc.
vehicleModel | String |Optional | Model of the vehicle as specified by manufacturer like FH series, MethaneDiesel, etc.
typeOfBody | String |Optional | Body Type of the vehicle. This is static field and can have only one of the below values - 4 wheeler, 2 wheeler, 24FT, 20FT, 32FT, 19FT, TATA 407, 14 FT CANTER, 17FT, TRLR, TRUCK 
unladdenWeight | Integer |Optional | Unladen weight of the vehicle in Kg.
capacityInWeight | Integer |Optional | Capacity of vehicle in Kg.
capacityInUnits | Integer |Optional | Capacity of the vehicle in Units
capacityInVolume | Integer |Optional | Capacity of the vehicle on cubic centimeters
chasisNumber | String |Optional | Chassis / VIN number of the vehicle
engineNumber | String |Optional | Engine Number of the vehicle
markerName | String |Optional | Name of the Sub-ordinate who is driving along with the driver
registrationNumber | String |Optional | Registered Number provided by the local transportation authority
pucValidity | Date |Optional | Date till which PUC certificate is valid.Format - YYYY-MM-DDTHH:MM:SS.SSSZ e.g. : 2016-07-01T11:18:00.000Z. This date always needs to be later than the current date.
insuranceValidity | Date |Optional | Date till which insurance of the vehicle is valid. Format - YYYY-MM-DDTHH:MM:SS.000Z e.g. : 2016-07-01T11:18:00.SSSZ.This date always needs to be later than the current date.
vehiclePermit | String |Optional | Full name of the states of India in which the vehicle is permitted to travel
ownership | String |Optional | Entity that owns the vehicle. This field can have only below two values - Company , Vendor
ownerName | String |Optional | Name of the Company / Vendor who owns the vehicle
transporter | String |Optional | Name of the transporter / carrier / 3PL provider responsible for the delivery.
financer | String |Optional | If the vehicle is on loan, then name of the financer
accidentHistory | String |Optional | If vehicle has any accident records against it, then please document the details through this field
rentStartDate | Date |Optional | This input is valid only if the vehicle’s owner is a vendor. Format - YYYY-MM-DDTHH:MM:SS.000Z e.g. : 2016-07-01T11:18:00.SSSZ. Rent start Date should be earlier than the Rent End Date.
rentEndDate  | Date |Optional | This input is valid only if the vehicle’s owner is a vendor. Format - YYYY-MM-DDTHH:MM:SS.000Z e.g. : 2016-07-01T11:18:00.SSSZ. Rent End Date should be later than the Rent Start Date.
deviceId.barcode | String |Optional | This input should be the barcode of the tracker in order to map the Vehicle with the tracker.In order to get the list of tracker that you can attach to a vehicle, you will need to either access the Loginext portal with your login or contact our CSA and get the list. On LogiNext portal, you can access the tracker list - LogiNext → Resource → Tracker.

## Get Vehicle (Single)

> Definition

```json
https://api.loginextsolutions.com/VehicleApp/v1/vehicle/:reference_id
```



> Response

```json
{
  "status": 200,
  "message": "success",
  "data": {
    "vehicleId": 1182,
    "vehicleName": null,
    "guid": "1efc418bd9f54bd99955cfdcccdc27a2",
    "vehicleNumber": "VH-00-1122",
    "vehicleMake": null,
    "vehicleModel": null,
    "vehicleType": null,
    "typeOfBody": "",
    "unladdenWeight": null,
    "capacityInUnits": 12,
    "capacityInVolume": 12,
    "capacityInWeight": 12,
    "chasisNumber": null,
    "engineNumber": null,
    "markerName": null,
    "registrationNumber": null,
    "pucValidity": null,
    "insuranceValidity": null,
    "vehiclePermit": null,
    "ownerName": null,
    "clientBranchId": 1402,
    "rentEndDate": null,
    "rentStartDate": null,
    "transporter": null,
    "financer": null,
    "permit": null,
    "ownership": null,
    "accidentHistory": null,
    "deviceId": {
      "deviceId": null,
      "barcode": null,
      "statusCd": null,
      "trackeeId": null
    },
    "registrationCopy": [],
    "pucCopy": [],
    "insuranceCopy": [],
    "registrationCopyExists": null,
    "insuranceCopyExists": null,
    "pucValidityExists": null,
    "clientBranchName": "Mahindra Logistics Ltd"
  },
  "hasError": false
}

```

Use this API to read all data for a particular vehicle using its reference ID.


### Request

<span class="post">GET</span>`https://api.loginextsolutions.com/VehicleApp/v1/vehicle/:reference_id`


### Request Parameters

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
reference_id | String | Mandatory | Reference Id associated with the vehicle.


## Get Vehicle

> Definition

```json
https://api.loginextsolutions.com/VehicleApp/v1/vehicle
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": {
    "totalCount": 119,
    "otherCount": 0,
    "results": [
      {
        "vehicleName": null,
        "vehicleNumber": "VH-00-1122",
        "vehicleMake": null,
        "vehicleModel": null,
        "vehicleType": null,
        "typeOfBody": "",
        "previousVehiclenumber": null,
        "unladdenWeight": null,
        "capacityInUnits": 12,
        "capacityInVolume": 12,
        "capacityInWeight": 12,
        "chasisNumber": null,
        "engineNumber": null,
        "markerName": null,
        "batteryPercentage": null,
        "registrationNumber": null,
        "mediaList": null,
        "pucValidity": null,
        "insuranceValidity": null,
        "vehiclePermit": null,
        "ownerName": null,
        "rentEndDate": null,
        "rentStartDate": null,
        "transporter": null,
        "financer": null,
        "status": "Available",
        "permit": null,
        "ownership": null,
        "lat": null,
        "lng": null,
        "accidentHistory": null,
        "lastTrackingDate": null,
        "vendorName": null,
        "deviceId": {
          "barcode": "Not Assigned",
          "statusCd": null,
          "trackeeId": null
        },
  
        "tripDetail": null,
        "insuranceAlertWindow": null,
        "pucAlertWindow": null,
        "lastInsuranceAlertSentDt": null,
        "lastPUCAlertSentDt": null,
        "gpsStatus": null,
        "speed": null,
        "branchName": null,
        "referenceId": "538649a7b9fc45be8d75b5932cc8ab60"
      }
    ]
  },
  "hasError": false
}

```

This API is used to list all existing vehicles in the system. All vehicle related data values will be returned.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/VehicleApp/v1/vehicle`


## Update Vehicle

You can update the data for any of the vehicle mapped to your account by using this API. 
You will have to pass the reference Id of the vehicle whose information needs to be updated.This reference Id is supplied to you as a response when the vehicle is created.

Note that you will be able to update the information of only those vehicles which are available. If the status of the vehicle is “In Transit”, then that vehicle cannot be updated and in this case you will get error message - 400 - Bad Request

> Definition

```json
https://api.loginextsolutions.com/VehicleApp/v1/vehicle
```

> Request Body

```json
[
	{
		"referenceId":"a9be39b9347911e6829f000d3aa04450",        
    		"vehicleNumber":"MH-111",
    		"vehicleMake":"2015",
   		    "vehicleModel":"OMNI",
    		"typeOfBody":"Flat Bed",
    		"unladdenWeight":1099,
    		"capacityInWeight":1099,
    		"capacityInUnits":1099,
    		"capacityInVolume":1099,
    		"chasisNumber":"CHASIS-123",
    		"engineNumber":"A-12353-D1234",
    		"markerName":"Marker-1",
    		"registrationNumber":"",
    		"pucValidity":"2016-07-01T11:18:00.000Z",
    		"insuranceValidity":"2016-07-01T11:18:00.000Z",
    		"vehiclePermit":"Andhra Pradesh,Delhi",
    		"ownership":"company",
    		"ownerName":"ABC",
    		"transporter":"",
    		"financer":"",
    		"accidentHistory":"",
    		"rentStartDate":null,
    		"rentEndDate":null,
    		"deviceId":{
        		"barcode":""
    		}
    }
]
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": null,
  "hasError": false
}

```
This API is used to update a particular vehicle based on its reference ID.

### Request

<span class="post">PUT</span>`https://api.loginextsolutions.com/VehicleApp/v1/vehicle`


### Request Parameters

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
vehicleNumber | String | Mandatory | Unique name to identify the vehicle
vehicleMake | String | Optional | Make of the vehicle like Mazda, Volvo, etc.
vehicleModel | String |Optional | Model of the vehicle as specified by manufacturer like FH series, MethaneDiesel, etc.
typeOfBody | String |Optional | Body Type of the vehicle. This is static field and can have only one of the below values - 4 wheeler, 2 wheeler, 24FT, 20FT, 32FT, 19FT, TATA 407, 14 FT CANTER, 17FT, TRLR, TRUCK 
unladdenWeight | Integer |Optional | Unladen weight of the vehicle in Kg.
capacityInWeight | Integer |Optional | Capacity of vehicle in Kg.
capacityInUnits | Integer |Optional | Capacity of the vehicle in Units
capacityInVolume | Integer |Optional | Capacity of the vehicle on cubic centimeters
chasisNumber | String |Optional | Chassis / VIN number of the vehicle
engineNumber | String |Optional | Engine Number of the vehicle
markerName | String |Optional | Name of the Sub-ordinate who is driving along with the driver
registrationNumber | String |Optional | Registered Number provided by the local transportation authority
pucValidity | Date |Optional | Date till which PUC certificate is valid.Format - YYYY-MM-DDTHH:MM:SS.SSSZ e.g. : 2016-07-01T11:18:00.000Z. This date always needs to be later than the current date.
insuranceValidity | Date |Optional | Date till which insurance of the vehicle is valid. Format - YYYY-MM-DDTHH:MM:SS.000Z e.g. : 2016-07-01T11:18:00.SSSZ.This date always needs to be later than the current date.
vehiclePermit | String |Optional | Full name of the states of India in which the vehicle is permitted to travel
ownership | String |Optional | Entity that owns the vehicle. This field can have only below two values - Company , Vendor
ownerName | String |Optional | Name of the Company / Vendor who owns the vehicle
transporter | String |Optional | Name of the transporter / carrier / 3PL provider responsible for the delivery.
financer | String |Optional | If the vehicle is on loan, then name of the financer
accidentHistory | String |Optional | If vehicle has any accident records against it, then please document the details through this field
rentStartDate | Date |Optional | This input is valid only if the vehicle’s owner is a vendor. Format - YYYY-MM-DDTHH:MM:SS.000Z e.g. : 2016-07-01T11:18:00.SSSZ. Rent start Date should be earlier than the Rent End Date.
rentEndDate  | Date |Optional | This input is valid only if the vehicle’s owner is a vendor. Format - YYYY-MM-DDTHH:MM:SS.000Z e.g. : 2016-07-01T11:18:00.SSSZ. Rent End Date should be later than the Rent Start Date.
deviceId.barcode | String |Optional | This input should be the barcode of the tracker in order to map the Vehicle with the tracker.In order to get the list of tracker that you can attach to a vehicle, you will need to either access the Loginext portal with your login or contact our CSA and get the list. On LogiNext portal, you can access the tracker list - LogiNext → Resource → Tracker.
deviceId.barcode | String |Optional | Barcode of the tracker.


## Delete Vehicle

Using this API, you can delete any of the vehicles listed against your account name.
You will have to pass the reference Id of the vehicle whose information needs to be updated.This reference Id is supplied to you as a response when the vehicle is created.

You can delete multiple vehicles, by passing multiple reference IDs in the Request body.

Note that you will be able to delete only which are available. If the status of the vehicle is “In Transit”, then that vehicle cannot be deleted. You will get error message - 400- Bad Request

Also, note that if a tracker is mapped to the vehicles that is deleted, then that tracker will be unmapped from that vehicle.

> Definition

```json
https://api.loginextsolutions.com/VehicleApp/v1/vehicle
```

> Request Body

```json
["a9be39b9347911e6829f000d3aa04450"]
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": null,
  "hasError": false
}

```

This API is used to delete a particular vehicle based on its reference ID.

### Request

<span class="post">DELETE</span>`https://api.loginextsolutions.com/VehicleApp/v1/vehicle`



### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
reference_ids | List | Mandatory | Reference Id associated with the vehicle.


## Create Driver
> Definition

```json
https://api.loginextsolutions.com/DriverApp/haul/v1/driver/create
```

> Request Body

```json
[
    {

        "driverName":"Test_Driver",
        "phoneNumber":1234565632,
        "emailId":"test@testing.com",
        "dateOfBirth":"2016-06-13",
        "languageList":[{"name":"English"},{"name":"Hindi"}],
        "salary":"10000",
        "maritalStatus":"married",
        "gender":"male",
        "experience":10,

        "licenseValidity":"2020-06-13",
        "licenseNumber":"LIC_104",
        "licenseType":"4 wheeler",
        "licenseIssueBy":"Maharashtra Govt.",

        "addressList":[ {
                                "apartment":"A-901",
                                "streetName": "Hiranandani Street",
                                "landmark":"DMart",
                                "countryShortCode":"IND",
                                "stateShortCode":"MH",
                                "city":"Mumbai",
                                "pincode":400076,
                                "isCurrentAddress":true
                        },
                        {
                                "apartment":"A-902",
                                "streetName": "LBS Marg",
                                "landmark":"SBI",
                                "countryShortCode":"IND",
                                "stateShortCode":"MH",
                                "city":"Mumbai",
                                "pincode":400092,
                                "isCurrentAddress":false
                        }],


        "driverEmployeeId":"D23",
        "shiftList":[{
                            "shiftStartTime"  :"07:03pm",
                            "shiftEndTime":"07:10am",
                            "startTime":"2016-06-13",
                            "endTime":"2016-06-15"

        }],

        "previousCompanyName":"ABC",
        "reportingManager":"Rahul",
        "managerPhoneNumber":1234567890,
        "managerEmailId":"test@test.com"
    }    
]
```

>  Response

```json
{
  "status": 201,
  "message": "success",
  "data": [
    "122e948ce6cc40fb85492c4c5a600816"
  ],
  "hasError": false
}

```

Create a new driver by passing form data through json.

The acknowledgement will provide the driver reference ID.

### Request

<span class="post">POST</span> `https://api.loginextsolutions.com/DriverApp/haul/v1/driver/create`



### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
driverName | String | Mandatory |  Driver's full name
phoneNumber | String | Mandatory | Phone No
emailId | String | Optional | EmailId
dateOfBirth | String | Optional | Date of Birth
languageList | List of Objects | Optional | Language(s) known to the driver
languageList.name | String | Optional | Name of language
salary | String | Optional | Current salary of Driver
maritalStatus | String | Optional | Marital Status. Ex: married, unmarried.
gender | String | Optional | Gender. Ex - male,female.
experience | Integer | Optional | No of yrs. of driving experience
licenseValidity | String | Optional | License validity date
licenseNumber | String | Mandatory | License No
licenseType | String | Optional | License Type. Ex: 2 wheeler, 4 wheeler
licenseIssueBy | String | Optional | License Issuing Authority Name
addressList.apartment | String | Mandatory | Apartment name/no
addressList.streetName | String | Mandatory | Society/Street name
addressList.landmark | String | Mandatory | Landmark
addressList.areaName | String | Optional | Locality/Area name
addressList.countryShortCode | String | Mandatory | Country short code
addressList.stateShortCode | String | Mandatory | State short code
addressList.city | String | Mandatory | City
addressList.pincode | Integer | Mandatory | Pincode
addressList.isCurrentAddress | Boolean | Mandatory | Indicates whether this is current address of driver or not. Ex: true - Current Address, false - Permanent Address
driverEmployeeId | String | Optional | EmployeeId
shiftList.startTime | String | Mandatory | Shift start date
shiftList.endTime | String | Mandatory | Shift end date
shiftList.shiftStartTime | String | Mandatory | Shift start time
shiftList.shiftEndTime | String | Mandatory | Shift end time
previousCompanyName | String | Optional | Driver's last company name
reportingManager | String | Optional | Driver's last company's reporting manager's name
managerPhoneNumber | String | Optional | Driver's last company'smanager's phone no
managerEmailId | String | Optional | Driver's last company's manager's email id






## Get Driver

> Definition

```json
https://api.loginextsolutions.com/DriverApp/haul/v1/driver/list
```

> Request Body

```json
["1c3a551f47534b98a29d916b0405fd6d"]
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": [
    {
  
      "driverName": "Test_Driver",
      "phoneNumber": "1234565632",
      "emailId": "test@testing.com",
      "licenseNumber": "LIC_104",
      "licenseValidity": 1591986600000,
      "license": null,
      "salary": 10000,
      "capacity": null,
      "addressLine1": null,
      "addressLine2": null,
      "city": null,
      "state": null,
      "pinCode": null,
      "licenseType": "4 wheeler",
      "defaultVehicle": null,
      "vehicleNumber": null,
      "licenseAlertWindow": null,
      "attendance": null,
      "workHour": null,
      "status": null,
      "dateOfBirth": 1465776000000,
      "experience": "11",
      "maritalStatus": "married",
      "gender": "female",
      "languageList": [
        {
          "name": "English",
        },
        {
          "name": "Hindi",
        }
        
      ],
      "licenseIssueBy": "Maharashtra Govt.",
      "tripName": null,
      "previousCompanyName": "ABC",
      "driverEmployeeId": "D23",
      "reportingManager": "Rahul",
      "managerPhoneNumber": "1234567890",
      "managerEmailId": "test@test.com",
      "deviceId": null,
      "trackingDate": null,
      "deviceBarcode": null,
      "shiftList": [
        {
          "shiftStartTime": "01, Jan 1970 07:03 PM",
          "shiftEndTime": "01, Jan 1970 07:10 AM",
          "startTime": null,
          "endTime": null
        }
      ],
      "addressList": [],
      "lat": null,
      "lng": null,
      "isPresent": null,
      "tripStatus": null,
      "batteryPerc": null,
      "lastLicenseAlertSentDt": null,
      "clientBranchName": null,
      "previousPhoneNumber": null,
      "referenceId": "1c3a551f47534b98a29d916b0405fd6d"
    }
  ],
  "hasError": false
}

```

Use this API to read all data for a particular driver using its reference ID.

### Request

<span class="post">POST</span>` https://api.loginextsolutions.com/DriverApp/haul/v1/driver/list`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
reference_ids | List | Mandatory | Reference Id associated with the driver.


## Update Driver

> Definition

```json
https://api.loginextsolutions.com/DriverApp/haul/v1/driver/update
```

> Request Body

```json
{
        "referenceId":"1c3a551f47534b98a29d916b0405fd6d",
        "driverName":"Test_Driver",
        "phoneNumber":"1234565632",
        "emailId":"test@testing.com",
        "dateOfBirth":"2016-06-13",
        "languageList":[{"name":"English"},{"name":"Hindi"}],
        "salary":"10000",
        "maritalStatus":"married",
        "gender":"female",
        "experience":11,

        "licenseValidity":"2020-06-13",
        "licenseNumber":"LIC_104",
        "licenseType":"4 wheeler",
        "licenseIssueBy":"Maharashtra Govt.",

        "addressList":[ {
                                "apartment":"A-901",
                                "streetName": "Hiranandani Street",
                                "landmark":"DMart",
                                "countryShortCode":"IND",
                                "stateShortCode":"MH",
                                "city":"Mumbai",
                                "pincode":400076,
                                "isCurrentAddress":true
                        },
                        {
                                "apartment":"A-902",
                                "streetName": "LBS Marg",
                                "landmark":"SBI",
                                "countryShortCode":"IND",
                                "stateShortCode":"MH",
                                "city":"Mumbai",
                                "pincode":400092,
                                "isCurrentAddress":false
                        }],


        "driverEmployeeId":"D23",
        "shiftList":[{
                            "shiftStartTime"  :"07:03pm",
                            "shiftEndTime":"07:10am",
                            "startTime":"2016-06-13",
                            "endTime":"2016-06-15"

        }],

        "previousCompanyName":"ABC",
        "reportingManager":"Rahul",
        "managerPhoneNumber":"1234567890",
        "managerEmailId":"test@test.com"
    }    
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": null,
  "hasError": false
}

```

This API is used to update a particular driver based on its reference ID.

### Request

<span class="post">PUT</span>`https://api.loginextsolutions.com/DriverApp/haul/v1/driver/update`



### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
referenceId | String | Mandatory |  ReferenceId of the record
driverName | String | Mandatory |  Driver's full name
phoneNumber | String | Mandatory | Phone No
emailId | String | Optional | EmailId
dateOfBirth | String | Optional | Date of Birth
languageList | List of Objects | Optional | Language(s) known to the driver
languageList.name | String | Optional | Name of language
salary | String | Optional | Current salary of Driver
maritalStatus | String | Optional | Marital Status. Ex: married, unmarried.
gender | String | Optional | Gender. Ex - male,female.
experience | Integer | Optional | No of yrs. of driving experience
licenseValidity | String | Optional | License validity date
licenseNumber | String | Mandatory | License No
licenseType | String | Optional | License Type. Ex: 2 wheeler, 4 wheeler
licenseIssueBy | String | Optional | License Issuing Authority Name
addressList.apartment | String | Mandatory | Apartment name/no
addressList.streetName | String | Mandatory | Society/Street name
addressList.landmark | String | Mandatory | Landmark
addressList.areaName | String | Optional | Locality/Area name
addressList.countryShortCode | String | Mandatory | Country short code
addressList.stateShortCode | String | Mandatory | State short code
addressList.city | String | Mandatory | City
addressList.pincode | Integer | Mandatory | Pincode
addressList.isCurrentAddress | Boolean | Mandatory | Indicates whether this is current address of driver or not. Ex: true - Current Address, false - Permanent Address
driverEmployeeId | String | Optional | EmployeeId
shiftList.startTime | String | Mandatory | Shift start date
shiftList.endTime | String | Mandatory | Shift end date
shiftList.shiftStartTime | String | Mandatory | Shift start time
shiftList.shiftEndTime | String | Mandatory | Shift end time
previousCompanyName | String | Optional | Driver's last company name
reportingManager | String | Optional | Driver's last company's reporting manager's name
managerPhoneNumber | String | Optional | Driver's last company'smanager's phone no
managerEmailId | String | Optional | Driver's last company's manager's email id

=======

## Delete Driver

> Definition

```json
https://api.loginextsolutions.com/DriverApp/haul/v1/driver/delete
```

> Request Body

```json
["e0eaebdd84ac4c40af72d827ab610090"]
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": null,
  "hasError": false
}

```

This API is used to delete a particular driver based on its reference ID.

### Request

<span class="post">DELETE</span>`https://api.loginextsolutions.com/DriverApp/haul/v1/driver/delete`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
reference_ids | List | Mandatory | Reference Id associated with the driver.


## Create Trip

> Definition

```json
https://api.loginextsolutions.com/TripApp/haul/v1/trip/create
```

> Request Body

```json
{
  "shipmentType": "Bag",
  sealNumber:"SN-123",
  "lrNumber":"LR123",
  "originAddr": "CNND",
  "destinationAddr": "NAGD",
  "name": "CNN-NAG-12221",
  "packageWeight": 6,
  "packageValue": 8,
  "packageVolume": 10,
  "vehicleReportingDate": "2016-02-27T18:30:00.000Z",
  "vehicleReportingTime": "1970-02-01T07:30:00.000Z",
  "modeOfTransport": "ROAD",
  "vehicleNumber": "MH40AK0175",
  "barcode": "LN00590915",
  "startNow":false
}
```

> Response

```json
{
  "status": 200,
  "message": "Trip created successfully.Reference Id for future access:1880d6906e9d426995b815a83aa3927f",
  "data": null,
  "hasError": false
}

```

Create a new trip using this API. Form data is passed through json.

The acknowledgement will contain the trip reference ID.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/TripApp/haul/v1/trip/create`



### Request Parameters

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
shipmentType | String | Mandatory | Type of the Shipment being created.Examples:"Bag","Package","Manifest"
originAddr | String |Mandatory | Origin point of the trip.Examples:-AMDD,BLRX
destinationAddr | String |Mandatory | Destination point of the trip.Examples:-CNND,BOMX
name | String |Mandatory | Name of the trip.Has to be unique.Example:-CNND-BOMX-123_456
packageWeight | Integer |Optional | Capacity of the shipment in terms of Kgs
packageValue | Integer |Optional | Capacity of the shipment in terms of the number of units present in it
packageVolume | Integer |Optional | Capacity of the shipment in terms of cc
vehicleReportingDate | Date |Mandatory | Reporting date of the vehicle at the origin hub
vehicleReportingTime | Date |Mandatory | Reporting time of the vehicle at the origin hub
modeOfTransport | String |Mandatory | Mode of transit for the trip.Examples:-ROAD,AIR,RAIL
lrNumber | String | Mandatory(Conditional) | Lorry Receipt Number if modeOfTransport selected as ROAD.
flightNum| String |Mandatory(Conditional) | Flight Number if modeOfTransport selected as AIR.
trainNum| String |Mandatory(Conditional) | Rail Number if modeOfTransport selected as RAIL.
driverName | String |Optional | Name of the driver
vehicleNumber | String |Optional | Vehicle Number.
barcode | String |Mandatory | Barcode of the tracker used for attaching to vehicle during trip



## Start Trip

> Definition

```json
https://api.loginextsolutions.com/TripApp/haul/v1/trip/start
```

> Request Body

```json
{
    "tripReferenceIds":["ca7fbf96a133461aadce8f94678084ee"]
}
```

> Response

```json
{
  "status": 200,
  "message": "Trips started successfully",
  "data": null,
  "hasError": false
}

```

This API is used to start a trip using its reference ID.

### Request

<span class="post">PUT</span>`https://api.loginextsolutions.com/TripApp/haul/v1/trip/start`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
tripReferenceIds | List | Mandatory | Reference Id associated with the trip.

## Stop Trip


```json
https://api.loginextsolutions.com/TripApp/haul/v1/trip/stop
```

> Request Body

```json
{
    "tripReferenceIds":["ca7fbf96a133461aadce8f94678084ee"]
}
```

> Response

```json
{
  "status": 200,
  "message": "Trips ended successfully",
  "data": null,
  "hasError": false
}

```

This API is used to end an in-transit trip using its reference ID.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/TripApp/haul/v1/trip/stop`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
tripReferenceIds | List | Mandatory | Reference Id associated with the trip.

## Trip iFrame

> Definition

```json
https://api.loginextsolutions.com/track/#/order?tripname=Trip-123&aid=4b41a94b-521b-4986-920d-6e4c1cf15fd0b6&key=$2a$08$dfVg6jJLhrHEsqOUfD1EJHyuelHeIgcUyvgTfGaeRmnzN5jGVi86k
```

The iFrame displays the last tracking for a trip, including current location and trip history, based on the trip name.

### Request


<span class="post">GET</span>` https://api.loginextsolutions.com/track/#/order?tripname=<tripname>&aid=<aid>&key=<key>`

### Request Parameters

Parameter | Sample Value | Description
--------- | ------- | -------------
aid | f522631c-490c-46fd-9f79-ca8d14a704d7 | Value of authentication token without 'BASIC' keyword
key | $2a$08$Vg6jJLhrHEsqOUfD1EJHyuelHeIgcUyvgT | Client Secret Key
tripname | TestTripName| Trip name

## Get Location

> Definition

```json
https://api.loginextsolutions.com/TrackingApp/haul/v1/track/lastlocation
```

> Request Body

```json
["TestTripName"]
```

> Response

```json
{
  "status": 200,
  "message": "Latest Location found successfully",
  "data": [
    {
      "lat": 10.394535555555555,
      "lng": 77.96088,
      "shipmentReference": "aefe22d9ba934908a7a2aeb790964814",
      "geocodingSource": null,
      "types": null,
      "eta": "2016-08-18 06:43:00"
    }
  ],
  "hasError": false
}
```

Track API fetches the latest location (latitude / longitude) for a trip based on its name.

### Request

<span class="post">GET</span>`https://api.loginextsolutions.com/TrackingApp/haul/v1/track/lastlocation`


### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
tripnames | List | Mandatory | Trip names


## Create Tracking Record

> Create Tracking Record - Sample Request

```json
{
  "trackerId": "4568088900",
  "latitude": 12.9003884,
  "longitude": 14.9889999,
  "time": "2016-07-14T09:11:56Z",
  "batteryPerc": 70.5,
  "speed": 40.4,
  "messageType": "REG",
  "temperature": 30.5
}
```

This endpoint adds tracking record.

### Request

<span class="post">POST</span>`http://api.loginextsolutions.com/TrackingApp/track/put`


### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
trackerId | String | Mandatory |  Device's unique ID
latitude | Double | Mandatory | Latitude
longitude | Double | Mandatory | Longitude
time | Date | Mandatory | Tracking time in UTC
batteryPerc | Double | Mandatory | Battery Percentage of device
speed | Double | Optional | Speed with which consignment is moving
messageType | String | Mandatory | Message type. Ex: REG
temperature | Double | Optional | Consignment's temperature

# Mile

Mile Product refers to the first mile and last mile shipment deliveries. Mile product will help you create -  

Pick-up orders thereby catering to your first leg of logistics, wherein shipments are ‘picked’ from your customer / merchants / suppliers / vendors and transported to the hub for aggregation.

Delivery orders by loading the items for different orders from a Single Point of Pick Up (Hub) and deliver the same to your customers (Multiple Drop Points).


1. Once the resources are created, then you can add shipments or orders in the LogiNext database by calling Create Order API. You need to provide the Order Number, Date and time window on which order should be picked-up / delivered and the pick-up / delivery address details. Additionally you can also specify the Crate level and line item level details contained in that order.The response consists of the Reference ID against each Order ID which needs to be stored in your system for future references.

2. Once the optimization for capacity and route planning is completed, trips will be created by the LogiNext system and you can mark the trip as started by calling the Start Trip API and mark the same trip as stopped by calling Stop Trip API. In both these API you will have to pass the order reference ID.

3. Finally you can track your pick-up / delivery executive in transit through the Track Last Location API. In this case also you need to pass the Trip Reference Id.

4. You can also mark a particular order as cancelled by calling the Cancel Order API and passing the order reference ID.

5. In case, your account is being configured into the LogiNext system as a pick-up and delivery both, the you can also create the return shipment for the order thereby optimizing you reverse logistics and return planning.

## Create Order (Delivery)

> Definition

```json
https://api.loginextsolutions.com/ShipmentApp/mile/v1/create
```

> Request Body

```json
[
  {
    "orderNo": "DummyOrderNo",
    "awbNumber": "AWB001",
    "shipmentOrderTypeCd": "DELIVER",
    "orderState": "FORWARD",
    "shipmentOrderDt": "2016-07-15T10:30:00.000Z",
    "distributionCenter": "Gurgaon",
    "packageWeight":"10",
    "packageVolume": "4500",
    "paymentType": "Prepaid",
    "packageValue": "5000",
    "numberOfItems": "10",
    "partialDeliveryAllowedFl": "Y",
    "returnAllowedFl": "Y",
    "cancellationAllowedFl": "N",    
    "deliverBranch": "Gurgaon",
    "deliverServiceTime": "20",
    "deliverEndTimeWindow": "2016-07-18T10:31:00.000Z",
    "deliverStartTimeWindow": "2016-07-16T10:31:00.000Z",
    "deliveryType": "DLBOY",
    "deliveryLocationType":"PUP",
    "deliverAccountCode": "Customer001",
    "deliverAccountName": "TestUser",
    "deliverEmail":"test@test.com",
    "deliverPhoneNumber":"1234567890",
    "deliverApartment": "123",
    "deliverStreetName": "Powai",
    "deliverLandmark": "Dmart",
    "deliverLocality": "Hiranandani",
    "deliverCity": "Mumbai",
    "deliverState": "MH",
    "deliverCountry": "IND",
    "deliverPinCode": "400076",    
    "returnBranch": "Gurgaon",
    "shipmentCrateMappings": [
      {
        "crateCd": "CRATE001",
        "crateAmount":100.65,
        "crateType":"case",
        "noOfUnits":10,
        "shipmentlineitems": [
          {
            "itemCd": "CODE001",
            "itemName": "ITEM1",
            "itemPrice": 100,
            "itemQuantity": 2,
            "itemType": "TYPE1",
            "itemWeight": 10
          },
          {
            "itemCd": "CODE002",
            "itemName": "ITEM2",
            "itemPrice": 50,
            "itemQuantity": 3,
            "itemType": "TYPE2",
            "itemWeight": 10
          }
        ]

      }
    ]
  }
]
```



> Response

```json
{
  "status": 200,
  "message": "Order created successfully",
  "referenceId": [
    "dcd883efcccc4d2299da962a72b01f23"
  ],
  "data": null,
  "hasError": false
}

```
Place a new delivery leg order with this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/ShipmentApp/mile/v1/create`

### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
orderNo | String | Mandatory |  Order No.
awbNumber | String | Optional | Airway Bill No.
shipmentOrderTypeCd | String | Mandatory | Order type code. DELIVER for delivery leg order
orderState | String | Mandatory | State of order. Ex: FORWARD
shipmentOrderDt | Date | Mandatory | Order Date
distributionCenter | String | Mandatory | Distribution center's name
packageWeight | Double | Optional | Weight of package in Kg.
packageVolume | Double | Optional | Volume of package in CC
packageValue | Double | Optional | Value of package
numberOfItems | Integer | Optional | Number of crates
paymentType | String | Optional | Payment mode. Ex: Cash On Delivery, Prepaid
partialDeliveryAllowedFl | String | Optional | Is Partial Delivery allowed. Ex: Y/N. Default value is N.
returnAllowedFl | String | Optional | Is Return allowed. Ex: Y/N. Default value is Y.
cancellationAllowedFl | String | Optional | Is Cancellation allowed. Ex: Y/N. Default value is Y.
deliverBranch | String | Mandatory | Name of delivery branch
deliverServiceTime | Integer | Mandatory | Deliver service time in mins.
deliverStartTimeWindow | Date | Mandatory | Deliver start time window
deliverEndTimeWindow | Date | Mandatory | Deliver end time window
deliveryType | String | Optional | Order delivery type. Ex: TRK - Truck, VAN - Van, DLBOY - Delivery Boy
deliveryLocationType | String | Optional | Type of delivery location. Ex: CUSTOMER, PUP
deliverAccountCode | String | Mandatory | Deliver account code
deliverAccountName | String | Mandatory | Deliver account name
deliverEmail | String | Optional | Deliver email
deliverPhoneNumber | String | Optional | Deliver phone number
deliverApartment | String | Mandatory | Apartment
deliverStreetName | String | Mandatory | Street name
deliverLandmark | String | Optional | Landmark
deliverLocality | String | Mandatory | Locality
deliverCity | String | Mandatory | City
deliverState| String | Mandatory | State
deliverCountry | String | Mandatory | Country
deliverPinCode | String | Mandatory | Pincode
returnBranch | String | Mandatory | Name of return branch
  
### Request Parameters (Crates)

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
shipmentCrateMappings | Array of objects | Optional | Shipment crates
shipmentCrateMappings.crateCd | String | Mandatory | Crate code
shipmentCrateMappings.crateAmount | Double | Mandatory | Crate amount
shipmentCrateMappings.crateType | String | Mandatory | Crate type. Ex - ??
shipmentCrateMappings.noOfUnits | Integer | Mandatory | No. of items in crate
shipmentCrateMappings.shipmentlineitems.itemCd | String | Mandatory | Item code
shipmentCrateMappings.shipmentlineitems.itemName | String | Optional | Item name
shipmentCrateMappings.shipmentlineitems.itemPrice | Double | Mandatory | Item price
shipmentCrateMappings.shipmentlineitems.itemQuantity | Double | Mandatory | Item quantity
shipmentCrateMappings.shipmentlineitems.itemType | String | Optional | Item type
shipmentCrateMappings.shipmentlineitems.itemWeight | Double | Optional | Item weight

## Create Order (Pickup)

> Definition

```json
https://api.loginextsolutions.com/ShipmentApp/mile/v1/create
```

> Request Body

```json
[
  {
    "orderNo": "DummyOrderNo1",
    "awbNumber": "AWB001",
    "shipmentOrderTypeCd": "PICKUP",
    "orderState": "FORWARD",
    "shipmentOrderDt": "2016-07-15T10:30:00.000Z",
    "distributionCenter": "test",
    "packageWeight":"10",
    "packageVolume": "4500",
    "packageValue": "5000",
    "paymentType": "Prepaid",
    "numberOfItems": "1",
    "deliveryType":"DLBOY",
    "partialDeliveryAllowedFl": "Y",
    "returnAllowedFl": "Y",
    "cancellationAllowedFl": "N",
    "pickupBranch":"testbranch",
    "pickupServiceTime": "50",
    "pickupStartTimeWindow": "2016-07-16T14:24:00.000Z",
    "pickupEndTimeWindow": "2016-07-17T14:24:00.000Z",
    "pickupAccountCode": "Customer123",
    "pickupAccountName": "Customer001",
    "pickupEmail": "test@test.com",
    "pickupPhoneNumber": "9090909090",
    "pickupApartment": "123",
    "pickupStreetName": "Supreme Business Park",
    "pickupLandmark": "DMart",
    "pickupLocality": "Hiranandani",
    "pickupCity": "Mumbai",
    "pickupState": "MH",
    "pickupCountry": "IND",
    "pickupPinCode": "400076",
    "shipmentCrateMappings": [
      {
        "crateCd": "CRATE001",
        "crateAmount":100.65,
        "crateType":"case",
        "noOfUnits":10,
        "shipmentlineitems": [
          {
            "itemCd": "CODE001",
            "itemName": "ITEM1",
            "itemPrice": 100,
            "itemQuantity": 2,
            "itemType": "TYPE1",
            "itemWeight": 10
          },
          {
            "itemCd": "CODE002",
            "itemName": "ITEM2",
            "itemPrice": 50,
            "itemQuantity": 3,
            "itemType": "TYPE2",
            "itemWeight": 10
          }
        ]

      }
    ]
  }
]
```



> Response

```json
{
  "status": 200,
  "message": "Order created successfully",
  "referenceId": [
    "dcd883efcccc4d2299da962a72b01f23"
  ],
  "data": null,
  "hasError": false
}

```
Place a new pickup leg order with this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/ShipmentApp/mile/v1/create`



### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
orderNo | String | Mandatory |  Order No.
awbNumber | String | Optional | Airway Bill No.
shipmentOrderTypeCd | String | Mandatory | Order type code. DELIVER for delivery leg order
orderState | String | Mandatory | State of order. Ex: FORWARD
shipmentOrderDt | Date | Mandatory | Order Date
distributionCenter | String | Mandatory | Distribution center's name
packageWeight | Double | Optional | Weight of package in Kg.
packageVolume | Double | Optional | Volume of package in CC
packageValue | Double | Optional | Value of package
paymentType | String | Mandatory | Payment mode. Ex: Cash On Delivery, Prepaid
numberOfItems | Integer | Optional | Number of crates
deliveryType | String | Optional | Order delivery type. Ex: TRK - Truck, VAN - Van, DLBOY - Delivery Boy
partialDeliveryAllowedFl | String | Optional | Is Partial Delivery allowed. Ex: Y/N. Default value is N.
returnAllowedFl | String | Optional | Is Return allowed. Ex: Y/N. Default value is Y.
cancellationAllowedFl | String | Optional | Is Cancellation allowed. Ex: Y/N. Default value is Y.
pickupBranch | String | Mandatory | Name of pickup branch
pickupServiceTime | Integer | Mandatory | Pickup service time in mins.
pickupStartTimeWindow | Date | Mandatory | Pickup start time window
pickupEndTimeWindow | Date | Mandatory | Pickup end time window
pickupAccountCode | String | Mandatory | Pickup account code
pickupAccountName | String | Mandatory | Pickup account name
pickupEmail | String | Optional | Pickup email id
pickupPhoneNumber | Optional | Mandatory | Pickup phone no
pickupApartment | String | Mandatory | Pickup Apartment
pickupStreetName | String | Mandatory | Pickup Street name
pickupLandmark | String | Optional | Pickup Landmark
pickupLocality | String | Mandatory | Pickup Locality
pickupCity | String | Mandatory | Pickup City
pickupState| String | Mandatory | Pickup State
pickupCountry | String | Mandatory | Pickup Country
pickupPinCode | String | Mandatory | Pickup Pincode

  
### Request Parameters (Crates)

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
shipmentCrateMappings | Array of objects | Optional | Shipment crates
shipmentCrateMappings.crateCd | String | Mandatory | CRATE001
shipmentCrateMappings.crateAmount | Double | Optional | Crate amount
shipmentCrateMappings.crateType | String | Optional | Type of crate. Ex: cake, juice, sweet, furniture etc.
shipmentCrateMappings.noOfUnits | Integer | Optional | No. of crate units
shipmentCrateMappings.shipmentlineitems.itemCd | String | Mandatory | Item code
shipmentCrateMappings.shipmentlineitems.itemName | String | Optional | Item name
shipmentCrateMappings.shipmentlineitems.itemPrice | Double | Mandatory | Item price
shipmentCrateMappings.shipmentlineitems.itemQuantity | Double | Mandatory | Item quantity
shipmentCrateMappings.shipmentlineitems.itemType | String | Optional | Item type
shipmentCrateMappings.shipmentlineitems.itemWeight | Double | Optional | Item weight




### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
orderNo | String | Mandatory |  Order No.
awbNumber | String | Optional | Airway Bill No.
shipmentOrderDt | Date | Mandatory | Order Date
deliveryType | String | Optional | Order delivery type. Ex: TRK - Truck, VAN - Van, DLBOY - Delivery Boy
packageVolume | String | Optional | Volume of package in CC
paymentType | String | Mandatory | Payment mode. Ex: Cash On Delivery, Prepaid
packageValue | String | Optional | Cost of Package
isPartialDeliveryAllowedFl | String | Optional | Is Partial Delivery allowed. Ex: Y/N
returnAllowedFl | String | Optional | Is Return allowed. Ex: Y/N
cancellationAllowedFl | String | Optional | Is Cancellation allowed. Ex: Y/N
numberOfItems | String | Optional | Number of crates
pickupServiceTime | String | Optional | Pickup service time in mins.
pickupStartTimeWindow | Date | Optional | Pickup start time window
pickupEndTimeWindow | Date | Optional | Pickup end time window
shipmentOrderTypeCd | String | Mandatory | Order type code. PICKUP for pickup leg order
orderState | String | Mandatory | State of order. Ex: FORWARD
pickupBranch | String | Mandatory | Name of pickup branch
distributionCenter | String | Mandatory | Distribution center's name
pickupAccountCode | String | Mandatory | Pickup account code
pickupAccountName | String | Mandatory | Pickup account name
pickupApartment | String | Mandatory | Apartment
pickupStreetName | String | Mandatory | Street name
pickupLandmark | String | Optional | Landmark
pickupLocality | String | Mandatory | Locality
pickupCity | String | Mandatory | City
pickupState| String | Mandatory | State
pickupCountry | String | Mandatory | Country
pickupPinCode | String | Mandatory | Pincode

### Request Parameters (Crates)

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
shipmentCrateMappings | Array of objects | Optional | Shipment crates
shipmentCrateMappings.crateCd | String | Mandatory | CRATE001
shipmentCrateMappings.crateAmount | Double | Optional | Crate amount
shipmentCrateMappings.crateType | String | Optional | Type of crate. Ex: cake, juice, sweet, furniture etc.
shipmentCrateMappings.noOfUnits | Integer | Optional | No. of crate units
shipmentCrateMappings.shipmentlineitems.itemCd | String | Mandatory | Item code
shipmentCrateMappings.shipmentlineitems.itemName | String | Optional | Item name
shipmentCrateMappings.shipmentlineitems.itemPrice | Double | Mandatory | Item price
shipmentCrateMappings.shipmentlineitems.itemQuantity | Double | Mandatory | Item quantity
shipmentCrateMappings.shipmentlineitems.itemType | String | Optional | Item type
shipmentCrateMappings.shipmentlineitems.itemWeight | Double | Optional | Item weight

## Create Order (Pickup & Delivery)

> Definition

```json
https://api.loginextsolutions.com/ShipmentApp/mile/v1/create
```

> Request Body

```json
[
  {
    "orderNo": "DummyOrderNo13",
    "awbNumber": "AWB001",
    "shipmentOrderTypeCd": "BOTH",
    "orderState": "FORWARD",
    "shipmentOrderDt": "2016-07-15T10:30:00.000Z",
    "distributionCenter": "test",
    "packageWeight":"10",
    "packageVolume": "4500",
    "paymentType": "Prepaid",
    "packageValue": "5000",
    "numberOfItems": "10",
    "partialDeliveryAllowedFl": "Y",
    "returnAllowedFl": "Y",
    "cancellationAllowedFl": "N",    
    "deliverBranch": "test",
    "deliverServiceTime": "20",
    "deliverEndTimeWindow": "2016-07-18T10:31:00.000Z",
    "deliverStartTimeWindow": "2016-07-16T10:31:00.000Z",
    "deliveryType": "DLBOY",
    "deliveryLocationType":"PUP",
    "deliverAccountCode": "Customer001",
    "deliverAccountName": "TestUser",
    "deliverApartment": "123",
    "deliverStreetName": "Powai",
    "deliverLandmark": "Dmart",
    "deliverLocality": "Hiranandani",
    "deliverCity": "Mumbai",
    "deliverState": "MH",
    "deliverCountry": "IND",
    "deliverPinCode": "400076",    
    "pickupBranch":"Gurgaon",
    "pickupServiceTime": "50",
    "pickupStartTimeWindow": "2016-07-16T14:24:00.000Z",
    "pickupEndTimeWindow": "2016-07-17T14:24:00.000Z",
    "pickupAccountCode": "Customer123",
    "pickupAccountName": "Customer001",
    "pickupApartment": "123",
    "pickupStreetName": "Supreme Business Park",
    "pickupLandmark": "DMart",
    "pickupLocality": "Hiranandani",
    "pickupCity": "Mumbai",
    "pickupState": "MH",
    "pickupCountry": "IND",
    "pickupPinCode": "400076",    
    "returnBranch": "test",
    "returnStartTimeWindow": "2016-05-18T03:00:00.000Z", 
    "returnEndTimeWindow": "2016-05-18T16:00:00.000Z", 
    "returnAccountCode": "retAcc123",
    "returnAccountName": "retAcc1234",
    "returnEmail": "test@test.com",
    "returnPhoneNumber": "9090909090",
    "returnApartment": "sjlkd CHS",
    "returnStreetName": "kljsdl Road",
    "returnLandmark": "skjdlk Nagar",
    "returnLocality": "kldlk West",
    "returnCity": "Mumbai",
    "returnState": "MH",
    "returnCountry": "IND",
    "returnPinCode": "400104",
    "shipmentCrateMappings": [
      {
        "crateCd": "CRATE001",
        "crateAmount":100.65,
        "crateType":"case",
        "noOfUnits":10,
        "shipmentlineitems": [
          {
            "itemCd": "CODE001",
            "itemName": "ITEM1",
            "itemPrice": 100,
            "itemQuantity": 2,
            "itemType": "TYPE1",
            "itemWeight": 10
          },
          {
            "itemCd": "CODE002",
            "itemName": "ITEM2",
            "itemPrice": 50,
            "itemQuantity": 3,
            "itemType": "TYPE2",
            "itemWeight": 10
          }
        ]

      }
    ]
  }
]
```



> Response

```json
{
  "status": 200,
  "message": "Order created successfully",
  "referenceId": [
    "dcd883efcccc4d2299da962a72b01f23"
  ],
  "data": null,
  "hasError": false
}

```
Place a new delivery leg order with this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/ShipmentApp/mile/v1/create`


### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
orderNo | String | Mandatory |  Order No.
awbNumber | String | Optional | Airway Bill No.
shipmentOrderTypeCd | String | Mandatory | Order type code. BOTH for pickup & delivery leg order
orderState | String | Mandatory | State of order. Ex: FORWARD
shipmentOrderDt | Date | Mandatory | Order Date
distributionCenter | String | Mandatory | Distribution center's name
packageWeight | Double | Optional | Weight of package in Kg.
packageVolume | Double | Optional | Volume of package in CC
packageValue | Double | Optional | Value of package
numberOfItems | Integer | Optional | Number of crates
paymentType | String | Mandatory | Payment mode. Ex: Cash On Delivery, Prepaid
partialDeliveryAllowedFl | String | Optional | Is Partial Delivery allowed. Ex: Y/N
returnAllowedFl | String | Optional | Is Return allowed. Ex: Y/N
cancellationAllowedFl | String | Optional | Is Cancellation allowed. Ex: Y/N
deliverBranch | String | Mandatory | Name of delivery branch
deliverServiceTime | Integer | Mandatory | Deliver service time in mins.
deliverStartTimeWindow | Date | Mandatory | Deliver start time window
deliverEndTimeWindow | Date | Mandatory | Deliver end time window
deliveryType | String | Optional | Order delivery type. Ex: TRK - Truck, VAN - Van, DLBOY - Delivery Boy
deliveryLocationType | String | Optional | Type of delivery location. Ex: CUSTOMER, PUP
deliverAccountCode | String | Mandatory | Deliver account code
deliverAccountName | String | Mandatory | Deliver account name
deliverApartment | String | Mandatory | Apartment
deliverStreetName | String | Mandatory | Street name
deliverLandmark | String | Optional | Landmark
deliverLocality | String | Mandatory | Locality
deliverCity | String | Mandatory | City
deliverState| String | Mandatory | State
deliverCountry | String | Mandatory | Country
deliverPinCode | String | Mandatory | Pincode
pickupBranch | String | Mandatory | Name of pickup branch
pickupServiceTime | Integer | Mandatory | Pickup service time in mins.
pickupStartTimeWindow | Date | Mandatory | Pickup start time window
pickupEndTimeWindow | Date | Mandatory | Pickup end time window
pickupAccountCode | String | Mandatory | Pickup account code
pickupAccountName | String | Mandatory | Pickup account name
pickupApartment | String | Mandatory | Pickup Apartment
pickupStreetName | String | Mandatory | Pickup Street name
pickupLandmark | String | Optional | Pickup Landmark
pickupLocality | String | Mandatory | Pickup Locality
pickupCity | String | Mandatory | Pickup City
pickupState| String | Mandatory | Pickup State
pickupCountry | String | Mandatory | Pickup Country
pickupPinCode | String | Mandatory | Pickup Pincode
returnBranch | String | Mandatory | Name of return branch
returnStartTimeWindow | Date | Mandatory | Return start time window
returnEndTimeWindow | Date | Mandatory | Return end time window
returnAccountCode | String | Mandatory | Return account code
returnAccountName | String | Mandatory | Return account name
returnEmail | String | Mandatory | Return account code
returnPhoneNumber | String | Mandatory | Return account name
returnApartment | String | Mandatory | Return Apartment
returnStreetName | String | Mandatory | Return Street name
returnLandmark | String | Optional | Return Landmark
returnLocality | String | Mandatory | Return Locality
returnCity | String | Mandatory | Return City
returnState| String | Mandatory | Return State
returnCountry | String | Mandatory | Return Country
returnPinCode | String | Mandatory | Return Pincode



### Request Parameters (Crates)

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
shipmentCrateMappings | Array of objects | Optional | Shipment crates
shipmentCrateMappings.crateCd | String | Mandatory | CRATE001
shipmentCrateMappings.crateAmount | Double | Optional | Crate amount
shipmentCrateMappings.crateType | String | Optional | Type of crate. Ex: cake, juice, sweet, furniture etc.
shipmentCrateMappings.noOfUnits | Integer | Optional | No. of crate units
shipmentCrateMappings.shipmentlineitems.itemCd | String | Mandatory | Item code
shipmentCrateMappings.shipmentlineitems.itemName | String | Optional | Item name
shipmentCrateMappings.shipmentlineitems.itemPrice | Double | Mandatory | Item price
shipmentCrateMappings.shipmentlineitems.itemQuantity | Double | Mandatory | Item quantity
shipmentCrateMappings.shipmentlineitems.itemType | String | Optional | Item type
shipmentCrateMappings.shipmentlineitems.itemWeight | Double | Optional | Item weight

## Create Return Order

> Definition

```json
https://api.loginextsolutions.com/ShipmentApp/mile/v1/create/return
```

> Request Body

```json
["863fe69239bc4f738ca275a809c3b2e2"]
```

> Response

```json
{
  "status": 201,
  "message": "success",
  "referenceId": [
    "d7b0f3f8e1174742bd6a8ae451866cb1"
  ],
  "data":null,
  "hasError": false,

}
```


Place a new return order with this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/ShipmentApp/mile/v1/create/return`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
reference_ids | List | Mandatory | Reference Id associated with the order.

## Cancel Order

> Definition

```json
https://api.loginextsolutions.com/ShipmentApp/mile/v1/cancel
```

> Request Body

```json
["e0eaebdd84ac4c40af72d827ab610090"]
```

> Response

```json
{
  "status": 200,
  "message": "success",
  "data": null,
  "hasError": false
}
```

Use this API to cancel an order.

### Request

<span class="post">PUT</span>`https://api.loginextsolutions.com/ShipmentApp/mile/v1/cancel`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
reference_ids | List  | Mandatory | Reference Id associated with the order.

## Get Status of Order

> Definition

```json
https://api.loginextsolutions.com/ShipmentApp/mile/v1/status
```

> Request Body

```json
["c8714df4347911e6829f000d3aa04450"]
```

> Response

```json
{
  "status": 200,
  "message": null,
  "data": [
    {
      "state": "FORWARD",
      "status": "NOTDISPATCHED",
      "referenceId": "c8714528347911e6829f000d3aa04450"
    }
  ],
  "hasError": false
}

```
Know the status of an order using this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/ShipmentApp/mile/v1/status`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
reference_ids | List | Mandatory | Reference Id associated with the order.

## Download EPOD

This endpoint downloads the EPODs for given order, delivery dates and status of order. The response is in form of a zip file. NOTE: The dates accepted are in UTC.

### Request

<span class="post">GET</span>`http://api.loginextsolutions.com/ShipmentApp/shipment/fmlm/epod/list?orderstartdt=2015-06-16 00:00:00&orderenddt=2016-06-16 00:30:00&deliverystartdt=2015-06-15 00:00:00&deliveryenddt=2016-06-15 00:00:00&status=NOTDISPATCHED`

### Request Parameters

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
orderstartdt | String | Mandatory | Order start date
orderenddt | String | Mandatory | Order end date
deliverystartdt | String | Mandatory | Delivery start date
deliveryenddt | String | Mandatory | Delivery end date
status | String | Optional | Order status. <BR>Ex: NOTDISPATCHED,INTRANSIT,DELIVERED,<BR>NOTDELIVERED,PICKEDUP,NOTPICKEDUP,CANCELLED

## Start Trip

> Definition

```json
https://api.loginextsolutions.com/TripApp/mile/v1/trip/start
```

> Request Body

```json
["a9be39b9347911e6829f000d3aa04450"]
```

> Response

```json
{
  "status": 200,
  "message": "1 trip(s) started",
  "data": true,
  "hasError": false
}

```
Start the trip for a delivery medium using this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/TripApp/mile/v1/trip/start`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
reference_ids | List | Mandatory | Reference Id associated with the trip.

## Stop Trip

> Request Body

```json
[{
    "tripReferenceId":"a9be39b9347911e6829f000d3aa04450",
    "notDispatchedOrders":["c8714df4347911e6829f000d3aa04450"],
    "deliveredOrders":["c8714cac347911e6829f000d3aa04450"]
}]
```

> Response

```json
{
  "status": 200,  
  "message": "Trips ended successfully",
  "data": true,
  "hasError": false
}

```
Stop the trip for a delivery medium using this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/TripApp/mile/v1/trip/stop`

### Request Body

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
tripReferenceId | String  | Mandatory | Reference Id associated with the trip.
notDispatchedOrders | List  | Mandatory | Reference Id associated with the non-dispatched order.
deliveredOrders | List | Mandatory | Reference Id associated with the delivered order.

## Track Last Location

> Definition

```json
https://api.loginextsolutions.com/TrackingApp/mile/v1/track/lastlocation?shipmentReferences=25a565a9c9d540cd9e6c02fae890cb67,c7afc8b1b97b48468c3417aa425eff81,27121903f4f047bcb378a6457bee2fec,21b538edf7f047028334480036179c70
```



> Response

```json
{
  "status": 200,
  "message": "Latest Location found successfully",
  "data": [
    {
      "lat": 19.1119794,
      "lng": 72.9094968,
      "shipmentReference": "21b538edf7f047028334480036179c70"
    },
    {
      "lat": 19.0668898,
      "lng": 72.8320575,
      "shipmentReference": "25a565a9c9d540cd9e6c02fae890cb67"
    },
    {
      "lat": 19.0741246,
      "lng": 72.824772,
      "shipmentReference": "27121903f4f047bcb378a6457bee2fec"
    },
    {
      "lat": 19.1200864,
      "lng": 72.9010175,
      "shipmentReference": "c7afc8b1b97b48468c3417aa425eff81"
    }
  ],
  "hasError": false
}

```
Use this to find out last tracked location for any order/ delivery medium.

### Request

<span class="post">GET</span>`https://api.loginextsolutions.com/TrackingApp/mile/v1/track/lastlocation`

## Create Delivery Medium

> Create Delivery Medium - Sample Request

```json
[
  {
    "employeeId": "Test001",
    "userGroupName":"Washola",
    "deliveryMediumMasterName": "DummyDM",
    "phoneNumber": 1234566432,
    "imei": 990000852473864,
    "emailId": "test@test.com",
    "userName": "test001",
    "password": "admin",
    "capacityInUnits": 10,
    "capacityInVolume": 10,
    "capacityInWeight": 10,
    "dob": "2016-08-18",
    "gender": "Female",
    "deliveryMediumMasterTypeCd": "Truck",
    "isOwnVehicleFl": "Company",
    "vehicleNumber": "MH-12345",
    "weeklyOffList": [
      "Friday"
    ],
    "maxDistance": 10,
    "licenseValidity": "2018-06-09",
    "deliveryMediumMapList": [
      {
        "name": "ENGLISH"
      },
      {
        "name": "GUJARATI"
      }
    ],
    "shiftList": [
      {
        "shiftStartTime": "2016-08-18T12:30:00Z",
        "shiftEndTime": "2016-08-18T15:30:00Z"
      }
    ]
  }
]
```

> Create Delivery Medium - Sample Response

```json
{
  "status": 201,
  "message": "success",
  "data": [
    "d7b0f3f8e1174742bd6a8ae451866cb1"
  ],
  "hasError": false
}

```

This endpoint creates a new delivery medium.

### Request

<span class="post">POST</span>`http://api.loginextsolutions.com/DeliveryMediumApp/mile/v1/create`


### Request Parameters

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
employeeId | String | Mandatory | Employee Id
branchName | String | Mandatory | Client branch name
userGroupName | String | Mandatory | User group name
deliveryMediumMasterName | String |Mandatory | Full name of Delivery medium
phoneNumber | String | Mandatory | Mobile no
imei | String |Optional | IMEI no
emailId | String | Optional | Email id
userName | String | Mandatory | Username
password | String | Mandatory | Password
capacityInUnits | Integer | Mandatory | Capacity of Delivery medium in units
capacityInVolume | Integer | Optional | Capacity of Delivery medium in volume
capacityInWeight | Integer |Optional | Capacity of Delivery medium in weight
dob | String | Optional | Date of birth
gender | String |Optional | Gender. Ex - Male,Female
deliveryMediumMasterTypeCd | String |Optional | Delivery medium type. Ex - Truck, Delivery Boy
isOwnVehicleFl | String |Optional | Owner of vehicle. Ex - Owned, Company
vehicleNumber | String | Optional | Vehicle number to be assigned to the delivery medium
weeklyOffList  | String |Optional | Array of week's off days. Ex - Monday, Tuesday etc.
maxDistance | Integer |Optional | Max. allowed distance
licenseValidity | String |Optional | License validity date
deliveryMediumMapList.name | String | Optional | Name of language
shiftList.shiftStartTime  | String |Optional | Shift start time
shiftList.shiftEndTime  | String |Optional | Shift end time

## Get Geocode

> Get Geocode - Sample Request

```json
{
  "apartment": "summerset building",
  "streetName": "powai",
  "landmark": "dmart",
  "locality": "hirananddanin",
  "city": "mumbai",
  "country": "India",
  "state": "Maharashtra",
  "pincode": "400076"
}
```

> Get Geocode - Sample Response

```json
{
 "status": 200,
 "message": "Geocodes Fetched Successfully",
 "referenceId": null,
 "data": [
   {
     "lat": 19.11736939999999,
     "lng": 72.9103214,
     "shipmentReference": null,
     "geocodingSource": "GOOGLE_PLACES"
   }
 ],
 "hasError": false
}

```

This API gets coordinates for a given location.

### Request

<span class="post">POST</span>`http://api.loginextsolutions.com/CommonApp/mile/v1/geocode`


### Request Parameters

Parameter | DataType |  Required | Description
-----------|-------|------- | ----------
apartment | String | Optional | Apartment 
streetName | String | Optional | Street name
landmark | String | Optional | Landmark
locality | String | Optional | Locality
city | String | Optional | City
country | String | Optional | Country
state | String |Optional | State
pincode | String | Mandatory | Pincode

## iFrame

> Definition

```json
https://api.loginextsolutions.com/track/#/order?ordno=1234&aid=4b41a94b-521b-4986-920d-6e4c1cf15fd0b6&key=$2a$08$dfVg6jJLhrHEsqOUfD1EJHyuelHeIgcUyvgTfGaeRmnzN5jGVi86k
```

The iFrame displays the last tracking for an order, including current location, based on the order no.

### Request


<span class="post">GET</span>` https://api.loginextsolutions.com/track/#/order?ordno=<ordno>&aid=<aid>&key=<key>`

### Request Parameters

Parameter | Sample Value | Description
--------- | ------- | -------------
aid | f522631c-490c-46fd-9f79-ca8d14a704d7 | Value of authentication token without 'BASIC' keyword
key | $2a$08$Vg6jJLhrHEsqOUfD1EJHyuelHeIgcUyvgT | Client Secret Key
ordno | 1234| Order no

# OnDemand


## Create Order (Fixed Pickup)

> Definition

```json
https://api.loginextsolutions.com/ShipmentApp/ondemand/v1/create
```

> Request Body

```json
[
  {
    "cashOnDelivery" : 1000,
    "cashOnPickup" : 1000,
    "customerName" : "TestName",
    "locality" : "Andheri East",
    "subLocality" : "JVLR",
    "address" : "JVLR Powai",
    "deliverPhoneNumber" : "8888889999",
    "orderNo" : "35629171418620161809",
    "distributionCenter" : "Mumbai",
    "paymentType" : "COD"
  }
]
```



> Response

```json
{
  "status": 200,
  "message": "success",
  "referenceId": [
    "80ecfaccd4544980805aedeefc9325d3"
  ],
  "data": null,
  "hasError": false
}


```
Place a new delivery leg order with this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/ShipmentApp/ondemand/v1/create`

### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
orderNo | String | Mandatory |  Order No.
distributionCenter | String | Mandatory | Distribution center's name
paymentType | String | Mandatory | Payment mode. Ex: COD, Prepaid
packageValue | Double | Optional | Package Value (This will be used when paymentType is Prepaid)
cashOnDelivery | Double | Mandatory(if paymentType is COD) | Cash to be collected on delivery
cashOnPickup | Double | Optional | Cash to be given on pickup
locality | String | Mandatory | Locality name
subLocality | String | Mandatory | Sub-locality name
address | String | Optional | Address where delivery should be done
deliverPhoneNumber | String | Mandatory | End customer contact number
customerName | String | Mandatory | End customer name



## Create Order (Variable Pickup)

> Definition

```json
https://api.loginextsolutions.com/ShipmentApp/ondemand/v1/create
```

> Request Body

```json
[
  {
    "cashOnDelivery" : 1000.0,
    "cashOnPickup" : 1000.0,
    "pickupAccountCode" : "1234",
    "pickupAccountName" : "Name1",
    "pickupEmail" : "demo1@ymail.com",
    "pickupPhoneNumber" : "8888990000",
    "pickupApartment" : "Apartment1",
    "pickupStreetName" : "SC1",
    "pickupLandmark" : "LM1",
    "pickupLocality" : "AN1",
    "pickupCountry" : "INDIA",
    "pickupState" : "Maharashtra",
    "pickupCity" : "Mumbai",
    "pickupPinCode" : "400076",
    "pickupStartTimeWindow": "2016-07-15T08:00:00.000Z",
    "pickupEndTimeWindow": "2016-07-15T08:45:00.000Z",
    "pickupLatitude" : 19.1239285,
    "pickupLongitude" : 72.9094407,
    "pickupNotes" : "",
    "deliverAccountCode" : "5678",
    "deliverAccountName" : "Name2",
    "deliverEmail" : "demo2@ymail.com",
    "deliverPhoneNumber" : "7788888899",
    "deliverApartment" : "Apartment2",
    "deliverStreetName" : "SC2",
    "deliverLandmark" : "LM2",
    "deliverLocality" : "AN2",
    "deliverCountry" : "INDIA",
    "deliverState" : "Maharashtra",
    "deliverCity" : "Mumbai",
    "deliverPinCode" : "400077",
    "deliverNotes" : "",
    "deliverStartTimeWindow": "2016-07-16T08:00:00.000Z",
    "deliverEndTimeWindow": "2016-07-16T10:00:00.000Z",
    "deliverLatitude" : 19.0778737,
    "deliverLongitude" : 72.9055627,
    "orderNo" : "TestOrder",
    "paymentType" : "COD",
    "distributionCenter":"Mumbai",
    "isPartialDeliveryAllowedFl" : "N",
    "shipmentOrderDt" : "2016-07-15T08:00:00.000Z",
    "deliverCapacityInVolume":123.32,
    "deliverCapacityInWeight":15.2
  }
]
```



> Response

```json
{
  "status": 200,
  "message": "success",
  "referenceId": [
    "80ecfaccd4544980805aedeefc9325d3"
  ],
  "data": null,
  "hasError": false
}


```
Place a new delivery leg order with this API.

### Request

<span class="post">POST</span>`https://api.loginextsolutions.com/ShipmentApp/ondemand/v1/create`

### Request Parameters

Param | DataType |  Required | Description
--------- | ------- | ---------- | ------------
orderNo | String | Mandatory |  Order No.
distributionCenter | String | Mandatory | Distribution center's name
shipmentOrderDt | Double | Mandatory | Order Date
deliverCapacityInVolume | Double | Optional | Weight of package in Kg.
deliverCapacityInUnits | Double | Optional | Volume of package in CC
paymentType | String | Optional | Payment mode. Ex: COD, Prepaid
packageValue | Double | Optional | Package Value (This will be used when paymentType is Prepaid)
cashOnDelivery | Double | Mandatory(if paymentType is Delivery) | Cash to be collected on delivery
cashOnPickup | Double | Optional | Cash to be given on pickup
isPartialDeliveryAllowedFl | String | Optional | Is Partial Delivery allowed. Ex: Y/N
pickupAccountCode | String | Mandatory | Pickup customer-id
pickupAccountName | String | Mandatory | Pickup customer name
pickupEmail | String | Optional | Pickup customer email-id
pickupPhoneNumber | String | Mandatory | Pickup customer contact number
pickupApartment | String | Mandatory | Pickup customer apartment
pickupStreetName | String | Mandatory | Pickup customer street name
pickupLandmark | String | Optional | Pickup customer landmark
pickupLocality | String | Mandatory | Pickup area name
pickupCountry | String | Mandatory | Pickup country
pickupState | String | Mandatory | Pickup state
pickupCity | String | Mandatory | Pickup city
pickupPincode | String | Mandatory | Pickup pincode
pickupStartTimeWindow | Date | Mandatory | Pickup Start time window of order
pickupEndTimeWindow | Date | Mandatory | Pickup End time window of order
pickupLatitude | Double | Optional | Latitude of pickup location
pickupLongitude | Double | Optional | Longitude of pickup location
pickupNotes | String | Optional | Pickup notes
deliverAccountCode | String | Mandatory | Deliver customer-id
deliverAccountName | String | Mandatory | Deliver customer name
deliverEmail | String | Optional | Deliver customer email-id
deliverPhoneNumber | String | Mandatory | Deliver customer contact number
deliverApartment | String | Mandatory | Deliver customer apartment
deliverStreetName | String | Mandatory | Deliver customer street name
deliverLandmark | String | Optional | Deliver customer landmark
deliverLocality | String | Mandatory | Deliver area name
deliverCountry | String | Mandatory | Deliver country
deliverState | String | Mandatory | Deliver state
deliverCity | String | Mandatory | Deliver city
deliverPincode | String | Mandatory | Deliver pincode
deliverStartTimeWindow | Date | Mandatory | Deliver Start time window of order
deliverEndTimeWindow | Date | Mandatory | Deliver End time window of order
deliverLatitude | Double | Optional | Latitude of deliver location
deliverLongitude | Double | Optional | Longitude of deliver location
deliverNotes | String | Optional | Deliver notes

# Webhooks


## Create Order

> Response

```json
{
  "clientShipmentId": "TestOrderNo",
  "orderState":"FORWARD",
  "orderLeg":"PICKUP",
  "awbNumber":"AWB001",
  "notificationType": "ORDERCREATIONNOTIFICATION",
  "billNumber":"TestOrderNo",
  "billType":"RETURN",
  "timestamp":"2016-07-01 03:05:08"
}
```


This notification is sent when an order is created.

Param | DataType | Description
--------- | ------- | ----------
clientShipmentId | String | Order No..
orderState | String | State of order. Ex: FORWARD, REVERSE
orderLeg | String | Order leg Ex: PICKUP, DELIVER, SALES, RETURN
awbNumber | String | AWB Number for the order
notificationType | String | ORDERCREATIONNOTIFICATION
billNumber | String | Will contain order no.
billType | String | Bill Type. RETURN for PICKUP order leg, SALES for DELIVER order leg
timestamp | String | Order creation timestamp

## Update Order

> Response

```json
{
  "clientShipmentId": "TestOrderNo",
  "notificationType": "ORDERUPDATENOTIFICATION",
  "deliveryMediumName":"TestDeliveryMedium",
  "phoneNumber":1234567890,
  "startTimeWindow":"2016-07-01 00:09:00",
  "endTimeWindow":"2016-07-01 00:09:00"
}
```


This notification is sent when an order is updated.

Param | DataType | Description
--------- | ------- | ----------
clientShipmentId | String | Order No..
notificationType | String | ORDERUPDATENOTIFICATION
deliveryMediumName | String | Name of delivery medium
phoneNumber | Long | Delivery medium's phone no.
startTimeWindow | String | Order's start time window
endTimeWindow | String  | Order's end time window

## Accept Order

> Response

```json
{
  "clientShipmentId": "TestOrder",
  "status": "ORDER ACCEPTED",
  "deliveryMediumName": "TestDM",
  "phoneNumber": 1234567890,
  "tripName": "TestTrip",
  "updatedOn": "2016-06-30 19:43:07"
}

```

This notification is sent when an order is accepted by a delivery boy.



### Response Parameters


Param | DataType | Description
--------- | ------- | ----------
clientShipmentId | String | Order No..
status | String | Status of the order
deliveryMediumName | String | Name of delivery medium
phoneNumber | Long | Delivery medium's phone no.
tripName | String | Trip name
updatedOn | String | Accept order timestamp

## Reject Order

> Response

```json
{
  "clientShipmentId": "TestOrder",
  "status": "ORDER REJECTED",
  "tripName": "TestTrip",
  "updatedOn": "2016-06-30 20:47:20",
  "reasonOfRejection": "Cannot reach on ETA"
}

```

This notification is sent when an order is rejected by a delivery boy.

### Response Parameters

Param | DataType | Description
--------- | ------- | ----------
clientShipmentId | String | Order No..
status | String | Status of the order
tripName | String | Trip name
updatedOn | String | Reject order timestamp
reasonOfRejection | String | Reason provided by Delivery medium while rejecting the order


## Load Items

```json
{
  "clientShipmentId": "TestOrderNo",  
  "orderState":"FORWARD",
  "orderLeg":"PICKUP",
  "awbNumber":"AWB001",
  "notificationType": "LOADITEMNOTIFICATION",
   "shipmentCrateMapping": [
      {
        "crateCd": "CRATE001",
        "crateType": "CRATE001",
        "statusCd": "CRATE001",
        "crateAmount": 100.30,
        "crateQuantity":3,
        "shipmentlineitems": [
          {
            "itemCd": "CODE001",
            "statusCd":"StatusCd",
            "itemName": "ITEM1",
            "itemPrice": 100,
            "itemQuantity": 1
          }
        ]
      }
    ]
}
```


This notification is sent when crates are loaded onto an order.

Param | DataType | Description
--------- | ------- | ----------
clientShipmentId | String | Order No..
orderState | String | State of order. Ex: FORWARD, REVERSE
orderLeg | String | Order leg Ex: PICKUP, DELIVER, SALES, RETURN
awbNumber | String | AWB Number for the order
notificationType | String | LOADITEMNOTIFICATION
shipmentCrateMapping.crateCd | String | Crate code
shipmentCrateMapping.crateType | String | Crate type
shipmentCrateMapping.statusCd | String | Crate Status. It will be LOADED.
shipmentCrateMapping.crateAmount | Double | Crate amount
shipmentCrateMapping.crateQuantity | Integer | No of loaded units
shipmentCrateMapping.shipmentlineitems.itemCd | String | Item code
shipmentCrateMapping.shipmentlineitems.statusCd | String | Item status. It will be LOADED.
shipmentCrateMapping.shipmentlineitems.itemName | String | Name of item
shipmentCrateMapping.shipmentlineitems.itemPrice | Double | Item price
shipmentCrateMapping.shipmentlineitems.itemQuantity | Integer | Item quantity

## Load Complete

```json
{
  "clientShipmentId": "TestOrderNo",
  "deliveryMediumName":"TestDeliveryMedium",
  "tripName":"TestTrip",
  "startTime":"2016-07-01 09:13:00",
  "phoneNumber":1234567890,
  "driverName":"TestDriverName",
  "vehicleNumber":"MH 03992"
  "revisedEta" : "2016-07-01 10:13:00",
  "notificationType": "LOADINGDONENOTIFICATION"
}
```


This notification is sent when crates are loaded onto an order.

### Response Parameters

Param | DataType | Description
--------- | ------- | ----------
clientShipmentId | String | Order No..
deliveryMediumName | String | Name of delivery medium
tripName | String | Trip name
startTime | Date | Time when loading is completed.
phoneNumber | Long | Delivery medium's phone no.
driverName | String | Driver's name
vehicleNumber | String | Vehicle no.
revisedEta | Date | Revised ETA
notificationType | String | LOADINGDONENOTIFICATION

## Delivered

> Response

```json
{
  "clientShipmentId": "TestOrder",
  "latitude": 28.283528,
  "longitude": 76.921535,
  "notificationType": "DELIVEREDNOTIFICATION",
  "customerComment": "Test comments",
  "customerRating": 5,
  "deliveryTime": "2016-06-30 19:12:49",
  "cashAmount": 100,
  "deliveryLocationType": "Customer",
  "transactionId": "0",
  "actualCashAmount": 100,
  "recipientName": "RecipientName"
}

```

This notification is sent when an order is delivered by a delivery boy to customer.



### Response Parameters

Key | DataType | Description
--------- | ------- |-------
clientShipmentId | String | Order No..
latitude | Double | Latitude where order was delivered
longitude | Double | Longitude where order was delivered
notificationType | String | DELIVEREDNOTIFICATION
customerComment | String | Customer comments
customerRating | Integer | Rating provided by customer
deliveryTime | String | Delivery timestamp
cashAmount | Double | Cash amount to collect
deliveryLocationType | String | Delivery Location
transactionId | String | Transaction id
actualCashAmount | Double | Cash amount actually collected
recipientName | String | Name of recipient


## Partially Delivered

> Response

```json
{
  "clientShipmentId": "TestOrder",
  "statusCd": "PARTIALLYDELIVERED",
  "notificationType": "PARTIALDELIVERYNOTIFICATION",
  "customerComments": "",
  "customerRating": 4,
  "reason": "Customer not accepting",
  "reasonCd": "",
  "deliveryTime": "2016-06-30 19:12:49",
  "cashAmount": 1,
  "transactionId": "",
  "actualCashAmount": 100,
  "shipmentCrateMappingList": [],
  "recipientName": "John2"
}

```

This notification is sent when an order is partially delivered by a delivery boy to customer.



### Response Parameters

Key | DataType | Description
--------- | ------- |-------
clientShipmentId | String | Order No..
statusCd | String | PARTIALLYDELIVERED
notificationType | String | PARTIALDELIVERYNOTIFICATION
customerComments | String |Customer comments
customerRating | Integer | Rating provided by customer
reason | String | Reason for the order being partially delivered
reasonCd | String | Reason code
deliveryTime | String | Delivery timestamp
cashAmount | Double | Cash amount to be collected
transactionId | String | Transaction id
actualCashAmount | Double | Cash amount actually collected
shipmentCrateMappingList | Array of Objects | Shipment crates
recipientName | String | Name of recipient

## Not Delivered

> Response

```json
{
  "clientShipmentId": "TestOrder",
  "notificationType": "NOTDELIVEREDNOTIFICATION",
  "customerComments": "Test comments",
  "customerRating": 5,
  "reason": "Product damaged in transit",
  "reasonCd": "",
  "deliveryTime": "2016-06-30 19:12:49",
  "deliveryLocationType": "Customer",
  "recipientName": "RecipientName"
}

```

This notification is sent when an order is not delivered by a delivery boy.



### Response Parameters

Key | DataType | Description
--------- | ------- | -------
clientShipmentId | String | Order No..
notificationType | String | NOTDELIVEREDNOTIFICATION
customerComments | String | Customer comments
customerRating | Integer | Rating provided by customer
reason | String | Reason for the order not delivered
reasonCd | String | Reason code
deliveryTime | String | Undelivered timestamp
deliveryLocationType | String | Delivery location
recipientName | String | Name of recipient

## Pickedup

> Response

```json
{
  "clientShipmentId": "TestOrder",
  "latitude": 28.283528,
  "longitude": 76.921535,
  "pickedUpTime": "2016-06-30 19:10:56",
  "status": "PICKEDUPNOTIFICATION"
}

```

This notification is sent when an order is picked up by a delivery boy.



### Response Parameters

Key | DataType | Description
--------- | ------- |-------
clientShipmentId | String | Order No..
latitude | Double | Latitude where order was pickedup
longitude | Double | Longitude where order was pickedup
pickedUpTime | String | Pickup order timestamp
status | String | PICKEDUPNOTIFICATION

## Not Pickedup

> Response

```json
{
  "clientShipmentId": "TestOrder",
  "notificationType": "NOTPICKEDUPNOTIFICATION",
  "orderLeg": "DELIVER",
  "awbNumber": "",
  "customerComments": "",
  "customerRating": 0,
  "deliveryMediumName": "TestDM",
  "phoneNumber": 1234567890,
  "orderState": "FORWARD",
  "customerName": "Customer name",
  "reason": "Product damaged in transit",
  "reasonCd": ""
}

```

This notification is sent when an order is picked up by a delivery boy.



### Response Parameters

Key | DataType | Description
--------- | ------- |-------
clientShipmentId | String |  Order No..
notificationType | String |  NOTPICKEDUPNOTIFICATION
orderLeg | String |  Order leg
awbNumber | String | Airway Bill Number
customerComments | String |  Customer comments
customerRating | Integer | Rating provided by customer
deliveryMediumName | String |  Name of delivery medium
phoneNumber | Long | Phone no of delivery medium
orderState | String | State of the order
customerName | String | Name of customer
reason | String | Reason for order not pickedup
reasonCd | String | Reason code

## Route Planning

> Response

```json
{
  "clientShipmentId": "TestOrder",
  "deliveryMediumName": "TestDM",
  "tripName": "TestTripName",
  "driverName":"TestDriverName",
  "vehicle":"MH01-1223",
  "latitude":19.1111232,
  "longitude":72.12334221,
  "deliveryOrder":1,
  "phoneNumber": 1234567890,  
  "notificationType": "DELIVERYPLANNING",
  "startTimeWindow":"2014-01-01 13:12:00",
  "endTimeWindow":"2014-01-01 13:12:00"
}

```

This notification is sent when orders are assigned to delivery boy for a particular window of time.



### Response Parameters

Key | DataType | Description
--------- | ------- |-------
clientShipmentId | String |  Order No..
deliveryMediumName | String |  Name of delivery medium
tripName | String |  Trip name
driverName | String |  Name of driver
vehicle | String |  Vehicle no.
latitude | Double |  Latitude
longitude | Double | Longitude
deliveryOrder | Integer |  Delivery order
phoneNumber | Long | Phone no of delivery medium
notificationType | String |  DELIVERYPLANNING
startTimeWindow | String |  Estimated start time of trip
endTimeWindow | String |  Estimated end time of trip

## Start Trip

> Response

```json
{
  "clientShipmentIds": ["TestOrder1","TestOrder2"],
  "notificationType": "STARTTRIP",
  "tripName": "TestTripName",
  "vehicleNumber":"MH01-1223",
  "driverName":"TestDriverName",
  "deliveryMediumName": "TestDM",
  "phoneNumber": 1234567890,  
  "startTime":"2014-01-01 13:12:00"
}

```

This notification is sent when a trip is started.

### Response Parameters

Key | DataType | Description
--------- | ------- |-------
clientShipmentIds | List<String> |  Order No.s.
notificationType | String |  STARTTRIP
tripName | String |  Trip name
vehicleNumber | String |  Vehicle no.
driverName | String |  Name of driver
deliveryMediumName | String |  Name of delivery medium
phoneNumber | Long | Phone no.
startTime | String |  Trip start time


## Stop Trip

> Response

```json
{
  "clientShipmentIds": ["TestOrder1","TestOrder2"],
  "notificationType": "DELIVEREDNOTIFICATION",
  "tripName": "TestTripName",
  "vehicleNumber":"MH01-1223",
  "driverName":"TestDriverName",
  "deliveryMediumName": "TestDM",
  "endTime":"2014-01-01 13:12:00"
}

```

This notification is sent when a trip is ended.

### Response Parameters

Key | DataType | Description
--------- | ------- |-------
clientShipmentIds | List<String> |  Order No.s.
notificationType | String |  DELIVEREDNOTIFICATION
tripName | String |  Trip name
vehicleNumber | String |  Vehicle no.
driverName | String |  Name of driver
deliveryMediumName | String |  Name of delivery medium
startTime | String |  Trip end time

## Hub In

> Response

```json
{

  "url" : "endpoint url",
  "data" : "<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"yes\"?>\n
          <geofencePushNotificationDTO>\n    
            <hubName>HubName</hubName>\n    
            <latitude>13.018617777777777</latitude>\n    
            <longitude>80.01128</longitude>\n    
            <sightingDate>201603091412</sightingDate>\n    
            <status>HUB IN</status>\n    
            <tripId>TripName</tripId>\n
          </geofencePushNotificationDTO>\n",
  "notificationType" : "HUB IN",
  "updatedDate" : "2016-03-09 07:09:10"
}

```

This notification is sent when a vehicle enters inside a hub.



### Response Parameters

Key | DataType | Description
--------- | ------- |-------
url | String |  Endpoint URL
data | String |  Data in XML format
notificationType | String |  HUB IN
updatedDate | String | Timestamp


## Hub Out

> Response

```json
{

  "url" : "endpoint url",
  "data" : "<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"yes\"?>\n
          <geofencePushNotificationDTO>\n    
            <hubName>HubName</hubName>\n    
            <latitude>13.018617777777777</latitude>\n    
            <longitude>80.01128</longitude>\n    
            <sightingDate>201603091412</sightingDate>\n    
            <status>HUB OUT</status>\n    
            <tripId>TripName</tripId>\n
          </geofencePushNotificationDTO>\n",
  "notificationType" : "HUB OUT",
  "updatedDate" : "2016-03-09 07:09:10"
}

```

This notification is sent when a vehicle enters inside a hub.



### Response Parameters

Key | DataType | Description
--------- | ------- |-------
url | String |  Endpoint URL
data | String |  Data in XML format
notificationType | String |  HUB IN
updatedDate | String | Timestamp
