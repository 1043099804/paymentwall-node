#About Paymentwall
[Paymentwall](http://paymentwall.com/?source=gh-node) is the leading digital payments platform for globally monetizing digital goods and services. Paymentwall assists game publishers, dating sites, rewards sites, SaaS companies and many other verticals to monetize their digital content and services. 
Merchants can plugin Paymentwall's API to accept payments from over 100 different methods including credit cards, debit cards, bank transfers, SMS/Mobile payments, prepaid cards, eWallets, landline payments and others. 

To sign up for a Paymentwall Merchant Account, [click here](http://paymentwall.com/signup/merchant?source=gh-node).

#Paymentwall Node.js Library
This library allows developers to use [Paymentwall APIs](http://paymentwall.com/en/documentation/API-Documentation/722?source=gh-node) (Virtual Currency, Digital Goods featuring recurring billing, and Virtual Cart).

To use Paymentwall, all you need to do is to sign up for a Paymentwall Merchant Account so you can setup an Application designed for your site.
To open your merchant account and set up an application, you can [sign up here](http://paymentwall.com/signup/merchant?source=gh-node).

#Installation
To install the library in your environment, you can download the [ZIP archive](https://github.com/paymentwall/paymentwall-node/archive/master.zip), unzip it and place into your project.

Alternatively, you can run:

  <code>git clone git://github.com/paymentwall/paymentwall-node.git</code>

Then use a code sample below.

#Code Samples

##Digital Goods API

####Initializing Paymentwall
<pre><code>
var PaymentwallBase = require('./lib/Paymentwall/Base.js');
PaymentwallBase.setApiType(PaymentwallBase.API_GOODS);
PaymentwallBase.setAppKey('YOUR_APPLICATION_KEY'); // available in your Paymentwall merchant area
PaymentwallBase.setSecretKey('YOUR_SECRET_KEY'); // available in your Paymentwall merchant area
</code></pre>

####Widget Call
[Web API details](http://www.paymentwall.com/en/documentation/Digital-Goods-API/710#paymentwall_widget_call_flexible_widget_call)

The widget is a payment page hosted by Paymentwall that embeds the entire payment flow: selecting the payment method, completing the billing details, and providing customer support via the Help section. You can redirect the users to this page or embed it via iframe. Below is an example that renders an iframe with Paymentwall Widget.

<pre><code>var widget = require('./lib/Paymentwall/Widget.js');
var PaymentwallProduct = require('./lib/Paymentwall/Product.js');

widget.initialize(
  'user40012',
  'p1',
  [
    PaymentwallProduct.initialize(
      'product301',
      9.99,
      'USD',
      'Gold Membership',
      PaymentwallProduct.TYPE_SUBSCRIPTION,
      1,
      PaymentwallProduct.PERIOD_TYPE_MONTH,
      true
    )
  ],
  {'email': 'user@hostname.com'}
);
console.log(widget.getHtmlCode());
</code></pre>

####Pingback Processing

The Pingback is a webhook notifying about a payment being made. Pingbacks are sent via HTTP/HTTPS to your servers. To process pingbacks use the following code:
<pre><code>var pingback = require('./lib/Paymentwall/Pingback.js');
pingback.initialize(queryData, ipAddress);

if (pingback.validate()) {
  var productId = pingback.getProduct().getId();
  if (pingback.isDeliverable()) {
    // deliver the product
  } else if (pingback.isCancelable()) {
    // withdraw the product
  } 
  console.log('OK'); // Paymentwall expects response to be OK, otherwise the pingback will be resent
} else {
  console.log(pingback.getErrorSummary());
}</code></pre>