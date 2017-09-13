# LeadPro WebForm
LeadPro is a tool for estate agents that instantly replies to portal enquiries, acknowledging their receipt and providing the enquirer with a short questionnaire to assess their suitability.

You can use the Web Form below to have LeadPro qualify enquiries sent through your homepage also.

A live demonstration is available here:
<http://leadpro-webform.propertytechnology.co.uk/demo>

## Setup and Customisation
The source code for the web form is available here:
<https://github.com/PropertyTechnology/PT-WebForm>

### Page Attributes
The following hidden fields must be set by the page on each submission of the form. Every field is mandatory.

`agency_username`
```
The unique identifier for your agency, supplied to you by Property Technology.
```

`office_username`
```
The unique identifier for your office, supplied to you by Property Technology.
```

`property_address`
```
The full street address (including postcode) for the property being enquired about.
```

`property_reference`
```
The unique identifier for the property in your property management software.
```

`kind`
```
The type of property listing - valid values are 'let' or 'sale'.
```

### Consumer Attributes
The following visible fields must be set by the consumer on each submission of the form. All fields except `message` are mandatory.

`first_name`
```
The first name of the consumer making the enquiry.
```

`last_name`
```
The last name of the consumer making the enquiry.
```

`email`
```
The contact email address of the consumer making the enquiry.
```

`phone`
```
The contact phone number of the consumer making the enquiry.
```

`message`
```
Any other message the consumer making the enquiry wishes to send to the agency.
```

## Building Your Own Form
If you wish to write your own form send data as a `POST` request to:

`https://api.propertytechnology.co.uk/consumer_enquiries`

You can test your implementation against the staging server by sending requests to: 

`https://api-staging.propertytechnology.co.uk/consumer_enquiries`

Using the following agency and office identifiers:

```
agency_username: propertytechnology
office_username: london
```

### Responses
The response to a submission will conform with the JSON API specification.

#### On Success
Success responses will follow this format:
<http://jsonapi.org/examples/#sparse-fieldsets>

```json
{
    "data": [
        {
            "type": "consumer_enquiries",
            "id": "a93882e3b5",
            "attributes": {
                "property_address": "40 Islington High St",
                "property_reference": "123456",
                "kind": "let",
                "name": "John Smith",
                "email": "j.smith@example.com",
                "phone": "+442078927246",
                "message": "Hello, when could i view?"
            }
        }
    ]
}
```

#### On Failure
Failure responses will follow this format:
<http://jsonapi.org/examples/#error-objects>

```json
{
    "errors": [
        {
            "status": 404,
            "source": {
                "parameter": "agency_username"
            },
            "title": "Agency Not Found",
            "detail": "Agency not found with agency_username: 'invalid-agency'"
        }
    ]
}
```

```json
{
    "errors": [
        {
            "status": 422,
            "source": {
                "pointer": "/data/attributes/kind"
            },
            "title": "Invalid Attribute",
            "detail": "Kind is not included in the list, can't be blank"
        }
    ]
}
```
