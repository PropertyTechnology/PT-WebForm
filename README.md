# LeadPro WebForm
The Property Technology Consumer Enquiries API allows information to be submitted to Property Technology. This may be done using the sample web form provided, or via an alternate implementation that supplies a valid POST request to the Property Technology system.

A live demonstration example of the web form is available at the following URL:
http://leadpro-webform.propertytechnology.co.uk/PT-WebForm/demo

The source code for the web form is available here:
https://github.com/PropertyTechnology/PT-WebForm

Data Fields
POST data is taken from a number of agency-set (hidden) and user-supplied (visible) fields.

There are hidden static fields which contain details on the Agency and Office:
- Agency Username (agency_username)
- Office Username (office_username)

There dynamic hidden fields which must be populated per property:
- Property Address (address)
- Property ID (property_reference)
- Whether the property is a ‘let’ or a ‘sale’ (kind)

There are visible fields which are populated using the form:
- First Name (first_name)
- Last Name (last_name)
- Email Address (email)
- Phone Number (phone)
- Message (message)
Data Content
With the exception of the Message field (message), all of the above fields are mandatory.

The hidden static fields must be populated with with content that will validate against the Property Technology system - i.e. valid Agency/Office usernames, and property Address/Reference details. The ‘kind’ attribute must be populated by ‘let’ or ‘sale’.

Basic validation is carried out pre-submission on the user supplied fields.
Submission Response
The response to a submission will conform to the JSON api with sparse fieldsets.
On Success:
An example successful submission could have the following response:
{"data":[{
"type":"consumer_enquiries","
Id":"90a7d1a8fc",
"Attributes":{
"property_address":"1 Mayfair, N1 1AA",
"property_reference":"property ABC",
"Kind":"let",
"name":"John Smith",
"email":"j.smith@example.com",
"Phone":"+442078927246",
"message":null}}]}

An unsuccessful submission (example: invalid agency username) receives an error response::
{"errors":[{
"Status":404,
"Source":{
"Parameter":"agency_username"},
"title":"Agency Not Found",
"detail":"Agency not found with agency_username: 'invalid-agency'"}]}

The sample web form indicates will indicate the success/failure of the submission to the user:

