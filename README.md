# LeadPro WebForm
LeadPro is a tool for estate agents that instantly replies to portal enquiries, acknowledging their receipt and providing the enquirer with a short questionnaire to assess their suitability.

Read more about LeadPro at <http://propertytechnology.co.uk>

You can use the web form below to have LeadPro also qualify enquiries sent through your homepage.

A live demonstration is available here:
<http://leadpro-webform.propertytechnology.co.uk/demo>

## Setup and Customisation
The source code for an example web form is available here:
<https://github.com/PropertyTechnology/PT-WebForm>

### Page Attributes
The following hidden fields must be set by the page on each submission of the form.
`listing_url` is highly recommended as users can forget which property they enquired about.

`agency_username`, mandatory
```
The unique identifier for your agency, supplied to you by Property Technology.
```

`office_username`, mandatory
```
The unique identifier for your office, supplied to you by Property Technology.
```

`property_address`, optional
```
The full street address (including postcode) for the property being enquired about.
If the address isn't provided, then a placeholder "No address" is used
```

`property_reference`, optional
```
The unique identifier for the property in your property management software.
```

`listing_url`, optional
```
The url for the property on your website.
```

`kind`, mandatory
```
The type of property listing - valid values are 'let' or 'sale'.

To send a sales viewing lead  - {kind : 'sale'}
To send a lettings viewing lead - {kind : 'let'}
```

`enforced_leads`, optional
```
A comma-separated list of lead types which should be created for this
enquiry.

For example 
To send a 'vendor valuation lead' - {kind: 'sale' , enforced_leads : 'vendor,market_appraisal'}
To send a 'landlord valuation lead' -  {kind : 'let' , enforced_leads : 'landlord,market_appraisal'} 
```

`additional_info`, optional
```
Any other information you would like tagged to the enquiry, it will not be shown to the consumer.
```

### Consumer Attributes
The following visible fields can be set by the consumer on each submission of the form.
Only the `email` field is mandatory, others are optional.

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
The contact telephone number of the consumer making the enquiry.
```

`address`
```
The contact address of the consumer making the enquiry.
```

`message`
```
Any other message the consumer making the enquiry wishes to send to the agency.
```

`day`
```
The day when the applicant wants to view the property.
```

`hours`
```
Hours when the applicant wants to view the property.
```

## Building Your Own Form
If you wish to write your own form send data as a `POST` request to:

`https://api.propertytechnology.co.uk/consumer_enquiries`

You can test your implementation against a staging server by sending requests to:

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
                "listing_url": "http://yourpage.com/property",
                "additonal_info": "Section A property",
                "kind": "let",
                "name": "John Smith",
                "email": "j.smith@example.com",
                "phone": "+442078927246",
                "address": "1 Malloy St",
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
