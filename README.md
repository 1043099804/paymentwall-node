#About Paymentwall
[Paymentwall](http://paymentwall.com/?source=gh-node) is the leading digital payments platform for globally monetizing digital goods and services. Paymentwall assists game publishers, dating sites, rewards sites, SaaS companies and many other verticals to monetize their digital content and services. 
Merchants can plugin Paymentwall's API to accept payments from over 100 different methods including credit cards, debit cards, bank transfers, SMS/Mobile payments, prepaid cards, eWallets, landline payments and others. 

To sign up for a Paymentwall Merchant Account, [click here](http://paymentwall.com/signup/merchant?source=gh-node).

#Paymentwall Node.js Library
This library allows developers to use [Paymentwall APIs](http://paymentwall.com/en/documentation/API-Documentation/722?source=gh-node) (Virtual Currency, Digital Goods featuring recurring billing, and Virtual Cart).

To use Paymentwall, all you need to do is to sign up for a Paymentwall Merchant Account so you can setup an Application designed for your site.
To open your merchant account and set up an application, you can [sign up here](http://paymentwall.com/signup/merchant?source=gh-node).

#Installation
To install the library in your environment, simply run the following command:

```
npm install paymentwall
```

Then use a code sample below.

#Code Samples

##Digital Goods API

[Web API details](http://www.paymentwall.com/en/documentation/Digital-Goods-API/710#paymentwall_widget_call_flexible_widget_call)

####Initializing Paymentwall

```javascript
var Paymentwall = require('paymentwall');
Paymentwall.configure(
  Paymentwall.Base.API_GOODS,
  'YOUR_APPLICATION_KEY',
  'YOUR_SECRET_KEY'
);
```

####Widget Call

The widget is a payment page hosted by Paymentwall that embeds the entire payment flow: selecting the payment method, completing the billing details, and providing customer support via the Help section. You can redirect the users to this page or embed it via iframe. The sample code below renders an iframe with Paymentwall Widget.

```javascript
var widget = new Paymentwall.Widget(
  'user40012',      // id of the end-user who's making the payment
  'p1',             // widget code, e.g. p1; can be picked in the Widgets section of your merchant account 
  [                 // product details for Flexible Widget Call. 
                    // Leave empty if product selection happens on Paymentwall's side
    new Paymentwall.Product(
      'product301',                           // id of the product in your system  
      9.99,                                   // price
      'USD',                                  // currency code
      'Gold Membership',                      // product name
      Paymentwall.Product.TYPE_SUBSCRIPTION,   // this is a time-based product
      1,                                      // duration is 1
      Paymentwall.Product.PERIOD_TYPE_MONTH,   //              month
      true                                    // this is a recurring product
    )
  ],
  {'email': 'user@hostname.com'}              // additional parameters. for full list check API docs
);
console.log(widget.getHtmlCode());
```

####Pingback Processing

The Pingback is a webhook notifying about a payment being made. Pingbacks are sent via HTTP/HTTPS to your servers. To process pingbacks use the following code:

```javascript
var pingback = new Paymentwall.Pingback(queryData, ipAddress);
if (pingback.validate()) {
  var productId = pingback.getProduct().getId();
  if (pingback.isDeliverable()) {
    // deliver the product
  } else if (pingback.isCancelable()) {
    // withdraw the product
  } 
  console.log('OK'); // Paymentwall expects the string OK in response, otherwise the pingback will be resent
} else {
  console.log(pingback.getErrorSummary());
}
```

## Virtual Currency API

[Web API Details](https://www.paymentwall.com/en/documentation/Virtual-Currency-API/711)

#### Initializing Paymentwall

```javascript
var Paymentwall = require('paymentwall');

Paymentwall.configure(
  Paymentwall.Base.API_VC,
  'YOUR_APPLICATION_KEY',
  'YOUR_SECRET_KEY'
);
```

#### Widget Call

```javascript
var widget = new Paymentwall.Widget(
  'user40012',
  'p10',
  [],
  {'email': 'user@hostname.com'}
);
console.log(widget.getHtmlCode());
```

#### Pingback Processing

```javascript
var pingback = new Paymentwall.Pingback(queryData, ipAddress);

if (pingback.validate()) {
  var virtualCurrency = pingback.getVirtualCurrencyAmount();
  if (pingback.isDeliverable()) {
    // deliver the virtual currency
  } else if (pingback.isCancelable()) {
    // withdraw the virtual currency
  } 
  console.log('OK'); // Paymentwall expects the string OK in response, otherwise the pingback will be resent
} else {
  console.log(pingback.getErrorSummary());
}
```

## Cart API

[Web API Details](https://www.paymentwall.com/en/documentation/Shopping-Cart-API/1098)

#### Initializing Paymentwall

```javascript
var Paymentwall = require('paymentwall');

Paymentwall.configure(
  Paymentwall.Base.API_CART,
  'YOUR_APPLICATION_KEY',
  'YOUR_SECRET_KEY'
);
```

#### Widget Call

```javascript
var widget = new Paymentwall.Widget(
  'user40012',
  'p10',
  [
    new Paymentwall.Product('product301', 3.33, 'EUR'), // first product in cart
    new Paymentwall.Product('product607', 7.77, 'EUR')  // second product in cart
  ],
  {'email': 'user@hostname.com'}
);
console.log(widget.getHtmlCode());
```

#### Pingback Processing

```javascript
var pingback = new Paymentwall.Pingback(queryData, ipAddress);

if (pingback.validate()) {
  var productId = pingback.getProduct().getId();
  if (pingback.isDeliverable()) {
    // deliver the product
  } else if (pingback.isCancelable()) {
    // withdraw the product
  } 
  console.log('OK'); // Paymentwall expects the string OK in response, otherwise the pingback will be resent
} else {
  console.log(pingback.getErrorSummary());
}
```
