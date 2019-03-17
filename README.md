# Sunbit javascript SDK
## Setup
### Supported browsers
### Basic setup
The Sunbit SDK for JavaScript doesn't require  any standalone files to be downloaded or installed. Instead you simply need to include a short piece of regular JavaScript in your HTML that will asynchronously load the SDK into your pages. The async load means that it does not block loading other elements of your page.
The following snippet of code will give the basic version of the SDK where the options are set to their most common defaults. You should insert it on each page you want to load it, directly after the opening tag.

```html
    <script>
        window.sunbitAsyncInit = function() {
          SUNBIT.init({
            token          : '<YOUR TOKEN>', //mandatory
            location       : '<YOUR LOCATION NAME>', //optional
            representative : '<REPRESENTATIVE>', //optional
            ro: '<RO>' //optional
          });
        };
      </script>
  <script async defer src="https://static.sunbit.com/sdk/sunbit-sdk.js"></script>
```
## UI Elements
The SDK provides, ready made HTML components to integrate into your webpage, make sure you call SUNBIT.init before using any of these components.
### Anchor link to Sunbit's check financing options page
1. Add a container for the Sunbit's element and add an ID to it.
2. Call the SUNBIT.UI.checkFinancingLink method to add it to the document. You can add any of the following  configuration objects to set a theme for the text:
    1. "sunbitBlue" - set text color to Sunbit's blue
    2. "sunbitYellow" - set text color to Sunbit's yellow
    3. "default" - use the site default styled for link elements

### Example

```html
<body>
...
<div id="sunbit-cfl"></div>
</body>
```
```js
<script>
    window.sunbitAsyncInit = function() {
        SUNBIT.init({
            token          : '<YOUR TOKEN>', //mandatory
            location       : '<YOUR LOCATION NAME>', //mandatory for check financing link
            representative : '<REPRESENTATIVE>', //optional
            ro: '<RO>' //optional
        });
        SUNBIT.UI.checkFinancingLink("sunbit-cfl", {
            theme: "sunbitBlue"
        });  
    };
</script>
<script async defer src="https://static.sunbit.com/sdk/sunbit-sdk.js"></script>
```

### As-Low-As text element
1. Add containers for Sunbit's element and  disclaimer (it is mandatory to also show Sunbit's disclaimer when the as-low-as component is being presented) and add IDs for these elements
2. Call the SUNBIT.UI.asLowAs method to add it to the document, this call accepts the following

| Param | Type | Description |
| ------ | ------ | ------ |
| elementId | String | Container element ID |
| disclaimerElementId | String | Disclaimer Container element ID |
| config | Obejct | Object containing the following: totalAmount - Number|
3. Provide the total amount figure to the elements for calculating an estimation, after the intial sum can be updated or a new element can be rendered, see below

To update the estimation when cart total or amount changes, either:
1. Call SUNBIT.UI.asLowAs, with the same container elements and a new SUM,  or;
2. Use updateAmount method returned from calling SUNBIT.UI.asLowAs, it accepts a new amount - Number.
### Example

```html
<body>
...
<div id="sunbit-ala"></div>
<div id="sunbit-ala-disclaimer"></div>
</body>
```
```js
<script>
    window.sunbitAsyncInit = function() {
        SUNBIT.init({
            token          : '<YOUR TOKEN>', //mandatory
            location       : '<YOUR LOCATION NAME>', //mandatory for check financing link
            representative : '<REPRESENTATIVE>', //optional
            ro: '<RO>' //optional
        });
        window.sunbitAla = SUNBIT.UI.asLowAs("sunbit-ala", "sunbit-ala-disclaimer", {
            totalAmount: <your initial total amount value>
        });  
    };
    ...
    // your cart update code might look like this:
    function recalculateCart(){
        // calculate cart
        window.sunbitAla.updateAmount(newTotal);
    }
</script>
<script async defer src="https://static.sunbit.com/sdk/sunbit-sdk.js"></script>
```

### As-Low-As text element including link to check financing options
This options works the same as As-Low-As above, except it also add a link to "check finance options".
### Example

```html
<body>
...
<div id="sunbit-ala"></div>
<div id="sunbit-ala-disclaimer"></div>
</body>
```
```js
<script>
    window.sunbitAsyncInit = function() {
        SUNBIT.init({
            token          : '<YOUR TOKEN>', //mandatory
            location       : '<YOUR LOCATION NAME>', //mandatory for check financing link element
            representative : '<REPRESENTATIVE>', //optional
            ro: '<RO>' //optional
        });
        window.sunbitAla = SUNBIT.UI.asLowAsWithLink("sunbit-ala", "sunbit-ala-disclaimer", {
            totalAmount: <your initial total amount value>,
            theme: 'default'
        });  
    };
    ...
    // your cart update code might look like this:
    function recalculateCart(){
        // calculate cart
        window.sunbitAla.updateAmount(newTotal);
    }
</script>
<script async defer src="https://static.sunbit.com/sdk/sunbit-sdk.js"></script>
```

## API calls
You can also use the SDK to make the API calls and use the data. 
After calling SUNBIT.init:
The Sunbit api method accepts the following:
| Param | Type | Description |
| ------ | ------ | ------ |
| methodName | String | one of the api method names, see list below |
| config | Object | api call configuration object, see specific api call below |
### Supported API Calls
* getPaymentEstimation - Estimate monthly payment amount for multiple items in a single request
* getOnlineUrl - Get the URL for Sunbit's online checkout - **Requires Location to be set**
### **Getting URL to Sunbit's Online checkot**
**!!! Be sure to set the Location during the SUNBIT.init !!!**
#### Usage
| Param | Type | Description |
| ------ | ------ | ------ |
| cb | Function| A callback function that receives error and responseData, if there is no error, it will be null |
| data | Array| An array of object containing: url - String |

#### Example
```js
SUNBIT.api('getOnlineUrl', {
    // a callback method for response 
    cb: function(error, responseData){
        console.log(error, responseData) //response: {url:'<url to sunbit online checkout>'}
    },
});
```

### **Getting payment estimation**
See [documentation](https://api.sunbit.com/purchase-service/payment-estimation/index.html#introduction)

#### Usage
| Param | Type | Description |
| ------ | ------ | ------ |
| cb | Function| A callback function that receives error and responseData, if there is no error, it will be null |
| data | Array| An array of object containing: id - String, totalAmount - Number \| String |
#### Example
```js
SUNBIT.api('getPaymentEstimation', {
    // a callback method for response 
    cb: function(error, responseData){
        console.log(error, responseData)
    },
    // required data as described in the documentation
    data:[{  "id":"123",  "totalAmount":"600"},{  "id":567, "totalAmount":1200}]
});
```
