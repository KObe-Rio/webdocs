# API (for Karl)
There are four routes for signups that need to be implemented. All responses and requests will be JSON. 400-499 statuses are considered errors and should contain a descriptive message.

### Active Forms 
`GET /activeforms`
The first request in an app's session. Requests the signup form(s) active at the time. The server should respond with an array with all forms currently available for members to complete. A form should contain the following:
##### The Form Object
`title` (required, string) - a user friendly title to be displayed at the top of the form
`id` (required, string) - a unique identifier for the form
`closes` (required, number) - epoch timestamp for when the form closes, in seconds
`options` (required, array) - an array of options (of the below format) given to the user. Right now, only one can be selected at once

##### The Options Object
`id` (required, string) - a unique identifier for the option
`text` (required, string) - the text to display to the user
`limit` (optional, number) - the number of spots available
`occupied` (optional, number) - the number of spots occupied already

##### Example 
An example of a response to `/activeforms` is below
```
const ActiveForms = [{
    title: 'Week 8 Training 6/3 - 7/3',
    id: '20389fj0sdjf',
    closes: 1583458383,
    options: [
        {
            text: 'Beginners Thursday 6:30',
            id: '230r884103298',
            occupied: 16,
            limit: 30
        },
        {
            text: 'Advanced Thursday 7:30',
            id: '239r8hsdf',
            occupied: 27,
            limit: 30
        },
        {
            text: 'Beginners Friday 6:30',
            id: '2f0f0f0f0293jfr9j',
            occupied: 1,
            limit: 30
        },
        {
            text: 'Advanced Friday 6:30',
            id: '010101010r94urh9328hr',
            occupied: 29,
            limit: 30
        }
    ]
}]
res.send({
    forms: ActiveForms
})
```

### Has Signed Up
`POST /hassignedup`
The seconds request made in an app's session. Checks which option a user has already signed up for. Request body sends the user's email and form Id in the format: `{email: 'abc@123.com', formId: '239487sd98fh'}`
Response should return the option the user previously signed up for, or `null` if the user hasn't signed up yet. The response should be of the format:
```
// if already signed up
res.send({
    signedUpOption: '239872134234'
})

// if no option already signed up
res.send({
    signedUpOption: null
})
```

### Submit Signup
`POST /submitsignup`
When the user submits their response to a form, they wil issue a POST request to /submitsignup, with the request body containing the user's email, the form Id, the option Id, and the previous option Id just in case their previous response needs to be removed from the record. The previous option id will be null if there was no previous option selected. The request body should be of the format below.
##### Example Request
```
{
    email: 'abc@123.com',
    formId: 'sdf9jsdfk',
    optionId: '239872134234',
    previousOptionId: null
}
```
The response to such a request should be an object such as the following: `{message: 'success'}`. If the request cannot be carried out, the response should return the same object with an error message: `{message: 'error error message here'}`. The error message should (for now) be user friendly as it will be displayed on the front end.
##### Example Response
```
res.send({
    message: 'Error storing response: the signup form has closed.'
})
```