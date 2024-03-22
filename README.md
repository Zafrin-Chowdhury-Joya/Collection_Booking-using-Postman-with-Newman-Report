### **Rest Booking API Testing with Postman Newman**
This project demonstrates API testing using Postman, providing a collection of tests to validate various endpoints of the API. 

### **Features**

- Tests for GET, POST, PUT, DELETE requests
- Collection of tests covering different API endpoints
- Environment setup for easy switching between environments
- Pre-request scripts for data setup
- Test scripts for assertions and validations

## API Documentation:
- https://documenter.getpostman.com/view/33506250/2sA2xpRogN
### **Technology used:**
- Postman
- Newman

### **Prerequisite:**
- Node Js
- Newman
- Newman Html Report Library

### **Installation**

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:
 ```console 
  git clone https://github.com/Zafrin-Chowdhury-Joya/Collection_Booking-using-Postman-with-Newman-Report.git
```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```
### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

## **Testing**

## Test Case Scenarios:

## _**1. Create New Booking**_

### Request URL: https://restful-booker.herokuapp.com/booking/
### Request Method: POST
### Pre-request Script:
```console 
var firstname = pm.variables.replaceIn("{{$randomFirstName}}")
console.log(firstname)
pm.environment.set("Firstname",firstname)

var lastname = pm.variables.replaceIn("{{$randomLastName}}")
console.log(lastname)
pm.environment.set("Lastname",lastname)

var totalprice = pm.variables.replaceIn("{{$randomInt}}")
console.log(totalprice)
pm.environment.set("Totalprice",totalprice)

var depositpaid = pm.variables.replaceIn("{{$randomBoolean}}")
console.log(depositpaid)
pm.environment.set("Depositpaid",depositpaid)

const date =require('moment')
const today = date()
console.log(today.format('YYYY-MM-DD'))

var checkIn = today.add(3,'d').format('YYYY-MM-DD')
console.log(checkIn)
pm.environment.set("CheckIn",checkIn)

var checkOut = today.add(5,'d').format('YYYY-MM-DD')
console.log(checkOut)
pm.environment.set("CheckOut",checkOut)

var additionalneeds = pm.variables.replaceIn("{{$randomNoun}}")
console.log(additionalneeds)
pm.environment.set("AdditionalNeeds",additionalneeds)
```
  **Request Body:** 
 ```console 
{
	"firstname" : "{{Firstname}}",
	"lastname" : "{{Lastname}}",
	"totalprice" : {{Totalprice}},
	"depositpaid" : {{Depositpaid}},
	"bookingdates" : {
    	"checkin" : "{{CheckIn}}",
    	"checkout" : "{{CheckOut}}"
	},
	"additionalneeds" : "{{AdditionalNeeds}}"
}
```
### Tests :
```console
var bookinginfo = pm.response.json()
var responseCode = pm.response.code
console.log(responseCode)
if (responseCode==200)
{
   pm.environment.set("ID",bookinginfo.bookingid)
}
else
{
    pm.test("Could not create the id ")
}
```
  **Response Body:**
 ```console 
{
    "bookingid": 1267,
    "booking": {
        "firstname": "Gregory",
        "lastname": "Beatty",
        "totalprice": 888,
        "depositpaid": true,
        "bookingdates": {
            "checkin": "2024-03-21",
            "checkout": "2024-03-26"
        },
        "additionalneeds": "firewall"
    }
}
```

 ## _**2. Get Booking Details By ID**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Tests :
```console
var responseCode = pm.response.code;

if (responseCode === 200) {
    var bookinginfo = pm.response.json();

    pm.test("First name validation", function () {
        pm.expect(pm.environment.get("Firstname")).to.eql(bookinginfo.firstname);
    });

    pm.test("Last name validation", function () {
        pm.expect(pm.environment.get("Lastname")).to.eql(bookinginfo.lastname);
    });

    pm.test("Total Price validation", function () {
        pm.expect(pm.environment.get("Totalprice")).to.eql(bookinginfo.totalprice.toString());
    });

    pm.test("Deposit paid validation", function () {
        pm.expect(pm.environment.get("Depositpaid")).to.eql(bookinginfo.depositpaid.toString());
    });

    pm.test("CheckIn date validation", function () {
        pm.expect(pm.environment.get("CheckIn")).to.eql(bookinginfo.bookingdates.checkin);
    });

    pm.test("CheckOut date validation", function () {
        pm.expect(pm.environment.get("CheckOut")).to.eql(bookinginfo.bookingdates.checkout);
    });

    pm.test("Additional needs validation", function () {
        pm.expect(pm.environment.get("AdditionalNeeds")).to.eql(bookinginfo.additionalneeds);
    });

} else if (responseCode === 404) {
    pm.test("Not Found");
} else if (responseCode === 500) {
    pm.test("Server error");
} else {
    pm.test("Something went wrong. Please check it");
}
```
### Response Body:
 ```console 
{
    "firstname": "Gregory",
    "lastname": "Beatty",
    "totalprice": 888,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-21",
        "checkout": "2024-03-26"
    },
    "additionalneeds": "firewall"
}
```
## _**3. Create A Token For Authentication.**_
### Request URL: https://restful-booker.herokuapp.com/auth
### Request Method: POST
### Pre-request Script: None
### Request Body:
 ```console 
{
	"username": "admin",
	"password": "password123"
}
```
  **Response Body:**
 ```console 
{
    "token": "ef9166f30e29a9d"
}
```

 ## _**4. Update the Booking Details**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: PUT
### Pre-request Script:
```console 
var firstnameupdated = pm.variables.replaceIn("{{$randomFirstName}}")
console.log(firstnameupdated)
pm.environment.set("UpdatedFirstname",firstnameupdated)

var lastnameupdated = pm.variables.replaceIn("{{$randomLastName}}")
console.log(lastnameupdated)
pm.environment.set("UpdatedLastname",lastnameupdated)

var totalpriceupdated = pm.variables.replaceIn("{{$randomInt}}")
console.log(totalpriceupdated)
pm.environment.set("UpdatedTotalprice",totalpriceupdated)

var depositpaidupdated = pm.variables.replaceIn("{{$randomBoolean}}")
console.log(depositpaidupdated)
pm.environment.set("UpdatedDepositpaid",depositpaidupdated)

const date =require('moment')
const today = date()
console.log(today.format('YYYY-MM-DD'))

var checkInupdated = today.add(3,'d').format('YYYY-MM-DD')
console.log(checkInupdated)
pm.environment.set("UpdatedCheckIn",checkInupdated)

var checkOutupdated = today.add(5,'d').format('YYYY-MM-DD')
console.log(checkOutupdated)
pm.environment.set("UpdatedCheckOut",checkOutupdated)

var additionalneedsupdated = pm.variables.replaceIn("{{$randomNoun}}")
console.log(additionalneedsupdated)
pm.environment.set("UpdatedAdditionalNeeds",additionalneedsupdated)
```
  **Request Body:** 
 ```console
{
	"firstname" : "{{UpdatedFirstname}}",
	"lastname" : "{{UpdatedLastname}}",
	"totalprice" : {{UpdatedTotalprice}},
	"depositpaid" : {{UpdatedDepositpaid}},
	"bookingdates" : {
    	"checkin" : "{{UpdatedCheckIn}}",
    	"checkout" : "{{UpdatedCheckOut}}"
	},
	"additionalneeds" : "{{UpdatedAdditionalNeeds}}"
}
```
  **Response Body:**
 ```console 
{
    "firstname": "Elenor",
    "lastname": "Huels",
    "totalprice": 430,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-21",
        "checkout": "2024-03-26"
    },
    "additionalneeds": "port"
}
```
 ## _**5.Get the updated Booking Details**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Pre-request Script: NONE
###Tests: 
 ```console 
var responseCode = pm.response.code;
var bookinginfo = pm.response.json();

if (responseCode === 200) {
    pm.test("Updated First name validation", function () {
        pm.expect(pm.environment.get("UpdatedFirstname")).to.eql(bookinginfo.firstname);
    });

    pm.test("Updated Last name validation", function () {
        pm.expect(pm.environment.get("UpdatedLastname")).to.eql(bookinginfo.lastname);
    });

    pm.test("Updated Total Price validation", function () {
        pm.expect(pm.environment.get("UpdatedTotalprice")).to.eql(bookinginfo.totalprice.toString());
    });

    pm.test("Updated Deposit paid validation", function () {
        pm.expect(pm.environment.get("UpdatedDepositpaid")).to.eql(bookinginfo.depositpaid.toString());
    });

    pm.test("Updated CheckIn date validation", function () {
        pm.expect(pm.environment.get("UpdatedCheckIn")).to.eql(bookinginfo.bookingdates.checkin);
    });

    pm.test("Updated CheckOutdate validation", function () {
        pm.expect(pm.environment.get("UpdatedCheckOut")).to.eql(bookinginfo.bookingdates.checkout);
    });

    pm.test("Updated Additionalneeds validation", function () {
        pm.expect(pm.environment.get("UpdatedAdditionalNeeds")).to.eql(bookinginfo.additionalneeds);
    });
} else if (responseCode === 403) {
    pm.test("Forbidden");
} else if (responseCode === 404) {
    pm.test("Not Found");
} else if (responseCode === 500) {
    pm.test("Server error");
} else {
    pm.test("Something went wrong. Please check it");
}
```
  **Response Body:**
 ```console 
{
    "firstname": "Elenor",
    "lastname": "Huels",
    "totalprice": 430,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-21",
        "checkout": "2024-03-26"
    },
    "additionalneeds": "port"
}
```

 ## _**6. Delete Booking Record**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: DELETE
### Response Body: None 

 ## _**7. Deleted booking check**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Pre-requisite Script : None
### Response Body: None
### Tests :
```console
var bookinginfo = pm.response.text();

pm.test("Booking Deleted successfully", function () {
    pm.expect(pm.response.code).to.equal(404);
});
```
## Run Command:  
- Run Command for Console: 
```console 
newman run API_Testing_ZAFRIN.postman_collection.json -e API_Testing_01.postman_environment.json 
```
- Run Command for Report: 
```console 
newman run API_Testing_ZAFRIN.postman_collection.json -e API_Testing_01.postman_environment.json -r cli,htmlextra
```
## Newman Report Summary:
![Newman Report Summary](https://github.com/Zafrin-Chowdhury-Joya/Collection_Booking-using-Postman-with-Newman-Report/assets/128959155/62ac3c65-9179-4958-ba92-e5b3e5e32f71)
![Newman Report Summary](https://github.com/Zafrin-Chowdhury-Joya/Collection_Booking-using-Postman-with-Newman-Report/assets/128959155/328c861d-0356-4d41-9a14-41cf8b3bf467)
![Newman Report Summary](https://github.com/Zafrin-Chowdhury-Joya/Collection_Booking-using-Postman-with-Newman-Report/assets/128959155/6796237b-94a5-477f-8546-78bffb3ca23b)
![Newman Report Summary](https://github.com/Zafrin-Chowdhury-Joya/Collection_Booking-using-Postman-with-Newman-Report/assets/128959155/c27ef392-26b4-48db-983d-1cfe03b95f47)

