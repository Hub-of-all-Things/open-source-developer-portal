---
layout: twoColumn
section: Funding source verification
type: article
title:  "Instant account verification"
weight: 1
description: "Programmatically verify a bank to initiate a bank transfer."
---

# Funding source verification

## Instant account verification (IAV)
Instant Account Verification (IAV) gives you the ability to add and verify a Customer's bank account in a matter of seconds, which is much faster than the multiple days required for micro-deposits to transfer and then be verified by the Customer. Your application will utilize the client-side `dwolla.js` JavaScript library to render the IAV flow within a container in your application. 

<ol class="alerts">
    <li class="alert icon-alert-info">This article will walk through IAV functionality using dwolla.js which is only available for Access API integrations. For more information about the Access API, please <a href="https://www.dwolla.com/contact">contact sales</a>.</li>
</ol>

### Step 1: Generate a single-use IAV token for a Customer
To initiate the IAV flow, dwolla.js requires a single-use IAV token for a Customer. Your server initiates a POST request to Dwolla, specifying for which Customer you want to add/verify a bank account. Dwolla will respond with a non-expiring single-use `token`. This IAV token will be sent to the client and used to authenticate the HTTP request asking Dwolla to render the IAV flow. 

```raw
curl -X POST 
\ -H "Content-Type: application/vnd.dwolla.v1.hal+json"
\ -H "Accept: application/vnd.dwolla.v1.hal+json"
\ -H "Authorization: Bearer qe634nV7dIYpYDf3VGZPciziPU2BCboUZ7G7EG8XEyGswKkBV5"
\ "https://api.dwolla.com/customers/06B51D56-7A6C-4535-A0CC-2C0106F56BA6/iav-token"

HTTP/1.1 200 OK
{  
   "_links":{  
      "self":{  
         "href":"https://api.dwolla.com/customers/06b51d56-7a6c-4535-a0cc-2c0106f56ba6/iav-token"
      }
   },
   "token":"lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL"
}
```
```ruby
customer_url = 'https://api-uat.dwolla.com/customers/06b51d56-7a6c-4535-a0cc-2c0106f56ba6'

# Using DwollaV2 - https://github.com/Dwolla/dwolla-v2-ruby (Recommended)
customer = account_token.post "#{customer_url}/iav-token"
customer.token # => "lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL"

# Using DwollaSwagger - https://github.com/Dwolla/dwolla-swagger-ruby
customer = DwollaSwagger::CustomersApi.get_customer_iav_token(customer_url)
customer.token # => "lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL"
```
```javascript
// Using dwolla-v2 - https://github.com/Dwolla/dwolla-v2-node
var customerUrl = 'https://api-uat.dwolla.com/customers/06b51d56-7a6c-4535-a0cc-2c0106f56ba6';

accountToken
  .post(`${customerUrl}/iav-token`)
  .then(function(res) {
    res.body.token; // => 'lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL'
  });
```
```python
customer_url = 'http://api.dwolla.com/customers/06b51d56-7a6c-4535-a0cc-2c0106f56ba6'
customers_api = dwollaswagger.CustomersApi(client)

token = customers_api.get_customer_iav_token(customer_url)
print token['token'] # => 'lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL'
```
```php
<?php
$customersApi = new DwollaSwagger\CustomersApi($apiClient);

$fsToken = $customersApi->getCustomerIavToken("https://api-uat.dwolla.com/customers/06b51d56-7a6c-4535-a0cc-2c0106f56ba6");
$fsToken->token; # => "lr0Ax1zwIpeXXt8sJDiVXjPbwEeGO6QKFWBIaKvnFG0Sm2j7vL"
?>
```

### Step 2: Include dwolla.js and add container for IAV
We'll begin the client-side implementation by including dwolla.js in the HEAD of our HTML page. Notice, we are utilizing development version 1 of dwolla.js, alternatively you can include the minified version of `<script src="https://cdn.dwolla.com/1/dwolla.min.js"></script>`. 

```htmlnoselect
<head>
<script src="https://cdn.dwolla.com/1/dwolla.js"></script>
</head>
```

Next, add the container to the body of the page where you want to render the IAV flow. Note: This container is responsive, meaning content is resized appropriately across all screen resolutions and devices.

```htmlnoselect
<div id="mainContainer">
	<input type="button" id="start" value="Add Bank">
</div>	

<div id="iavContainer"></div>
```


### Step 3: Configure and call JavaScript function to start IAV
Now that you have initialized dwolla.js on your page you can configure and create the function that will call `dwolla.iav.start()`. For simplicity of this example, use jQuery and call the function to start IAV when the Customer clicks the "Add Bank" button on your page. To test IAV in the sandbox environment you will use the `dwolla.configure` helper function and pass in the value of `uat`. As you can see below, `dwolla.iav.start` takes three arguments: the iavContainer element where IAV will render, a string value of the single-use IAV token, and a callback function that will handle any error or response.


```javascriptnoselect
<script type="text/javascript">
$('#start').click(function() {
  var iavToken = '4adF858jPeQ9RnojMHdqSD2KwsvmhO7Ti7cI5woOiBGCpH5krY';
  dwolla.configure('sandbox');
  dwolla.iav.start('8zN400zyPUobbdmeNfhTGH2Jh5JkFREJa9YBI8SLXp0ERXNTMT', {
  container: 'iavContainer',
  stylesheets: [
    'http://fonts.googleapis.com/css?family=Lato&subset=latin,latin-ext',
    'http://myapp.com/iav/someStylesheet.css'
  ],
  microDeposits: false,
  fallbackToMicroDeposits: false
}, function(err, res) {
  console.log('Error: ' + JSON.stringify(err) + ' -- Response: ' + JSON.stringify(res));
});
});
</script>
```

In order to facilitate testing IAV in the Sandbox, we allow you to specify any value in the IAV text field inputs to proceed through the flow.

### Step 4: Handle the error or response
The callback function (err, res) allows you to determine if there is an error with the request (i.e. `InvalidIavToken`) or if the response was successful.

* If there is an error: generate a new IAV token and re-attempt the IAV flow.
	* Example: `{code: "InvalidIavToken", message: "Invalid IAV token." }`
* If successful: You will receive a JSON response that includes a link to the newly created and verified funding source. You can then pass this value to either initiate a transfer or store the created resource in your database to be used later.
	* Example:  `{"_links":{"funding-source":{"href":"https://api-uat.dwolla.com/funding-sources/80275e83-1f9d-4bf7-8816-2ddcd5ffc197"}}}`

* * *

#### View:

*   [Dwolla.js - Instant account verification](/resources/dwolla-js/instant-account-verification.html)