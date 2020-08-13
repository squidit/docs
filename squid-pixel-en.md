# Squid Pixel

## About

> It's a script integration from squid, that track all registrations and sales

## Behavior

The component squid-pixel behaves like a simple tracking pixel. It use an unique url, but provides a set a variables that can be replaced by componentn into string URL when makes a request.

### Influencer Link 

So that SquidPixel can receive parameters from an influencer. The link from Landing Page should have a querystring in following standard `?squid-share=CODIGO`. This code are stored into access device, and this code is possible check if the conversion of seller or registration was generated through of influencer link

### Influencer Coupon

The SquidPixel also monitors the conversion (generally sales) made from a influencer coupon. The code of this coupon should be sent with the conversion success record (using the variables from Landing Page).
Eg: `cupom_used` in section `2 - Record registered script`

### Value in sales

To register the value in sales from SquidPixe are required send together with the conversion record the value of the cart's success (using Landing Pages variables). Eg: `order_value` in section `2 - Record registered script`

## Integration - List scripts of page or through GTM

### 1 - Initialize Script of SquidPixel:

This script are used to run the SquidPixel in Landing Page and to validate if the origin link is from an influencer.


#### Script:

```
<script>
(function(d,s,id){  chk=function(cb){if(window.squidPixelLoaded){cb();}else{window.setTimeout(function(){chk(cb);},100);}};
  b={};b.init=function(a){chk(function(){TP.init(a);});};
  b.registerSell=function(so){chk(function(){TP.registerSell(so);});};
  b.registerProduct=function(pi){chk(function(){TP.registerProduct(pi);});};
  SquidPixel=b;if(!window.squidPixelLoaded){var js,te=d.getElementsByTagName(s)[0];if(d.getElementById(id)){return;}
  js=d.createElement(s);js.id=id;
  js.src="https://cdn2.squidit.com.br/referral/squid-pixel.js";te.parentNode.insertBefore(js,te);}
  }(document,'script','squid-pixel'));
  SquidPixel.init("[[ID DA ORGANIZAÇAO DO CLIENTE]]");
</script>
```

##### Attributes:

`[[ID DA ORGANIZAÇAO DO CLIENTE]`: Id Organization in squid, its used to validate the link or coupon from influencer

##### Installation:

This script needs to be installed in a way that could be initialized together with landing page. For example, into footer page. And it needs to be accessed from all pages from the current Landing Page, especially in home page and the successful registration or cart page.


##### How to know if works:

To make sure that this step of installation was successful, follow this step by step.

- Open the console in your dev tool browser.

- Put the follow querystring  `?squid-share=testpixel` in URL of landing page.

- Make sure that you have these two records.


`Squid - Pixel initiated - organization: ID DA ORGANIZAÇAO DO CLIENTE`

`Squid - Share code validated`


*Our API will validate if is a test link and will validate as a Squid influencer link*

### 2 - Register conversion script

This script is used to record conversions at Squid base.

#### Script:

```
<script>
  SquidPixel.registerSell({
    order_id: [[__]],
    order_value: [[__]], //linha opcional
    cupom_used: [[__]], //linha opcional
    other_info: {
      [[__]] //linha opcional
    }
  })
</script>
```

##### Requred Attribute

`order_id` ECOMMERCE UNIQUE ID, FOR EXAMPLE `order_id: "ORDERID12345"` - this  id is important for auditing data purpose of conversions with the platform that is integrating squid-pixel.


##### Optionals Attributes

`order_value` Shopping cart value if you will gamificate this value in sales, for example `order_value: 100`

`cupom_used` Coupon code if it will used into your order, for example `cupom_used: "SQUID10"`

`other_info` Any relevant information to keep in conversion, for example `other_info: {emaildecadastro: "email@exemplo.com"}`


#### Installation:

This script should be installed into success registration page or into cart page. Can be in the HTML page or can be called through JS in success registration event or success cart event.


#####  How to know if works::

To make sure that this step of installation was successful, follow these step by step:


- Open the console in your dev tool browser.

- Put the follow querystring  `?squid-share=testpixel` in URL of Landing Page.

- Generate a conversion into your Landing Page

- Make sure that you have these three records.


`Squid - Pre-convertion registered.`

`Squid - Share code exists`

`Squid - Sell registered`

*Our API will validate if is a test link and will validate as a Squid influencer link*