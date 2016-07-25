# Hupilytics
This allow to send analytics data (like google analytics) within hupi's servers.
hupilytics.js is a fork of [piwik.js](https://github.com/piwik/piwik/blob/master/js/piwik.js), there for hupilytics.js is licensed under the same terms as the piwik.js (GPL v3)

## How to integrate hupilytics.js to your website
Add hupilytics.js in the assets of your website and require the script in your website head (see one example below).
To send analytic data to hupi's servers , set the `setTrackerUrl` parameter to `http://api.catchbox.hupi.io/v1/(:account)/hupilytics`.

To tune hupiltyics.js, refer to
 [Piwik's JavaScript Tracking Client Docs](http://developer.piwik.org/api-reference/tracking-javascript), and [Piwik's Custom Variable Docs](http://piwik.org/docs/custom-variables).
To understand variable meaning, refer to: [Piwik's Tracking API Docs](http://developer.piwik.org/api-reference/tracking-api)

### Integration Example

```html
<!-- Hupilytics -->
<script src="hupilytics.js"></script>
<script type="text/javascript">
  var _paq = _paq || [];
  (function()
  {
    var u = "http://api.catchbox.hupi.io/v2/(:account)/hupilytics";
    _paq.push(['setTrackerUrl', u]);  // Required
    _paq.push(['setSiteId', 1]);  // Required: must be an integer

    // If you want to track your users, you can provide a UserID, see http://piwik.org/docs/user-id/
    // _paq.push(['setUserId', '$user_id']);  // must be an int

    // If you have to deal with multiple domains you can use, these :
    // _paq.push(['setDomains', 'www.domain.com']);  // domain wildcard works too: '.domain.com' or '*.domain.com'
    // _paq.push(['setCookieDomains', 'www.domain.com']);  // domain wildcard works too: '.domain.com' or '*.domain.com'

    // ! Required ! Our API needs the current timestamp for the page
    function current_ts()
    {
      // needed for IE8 compat, see http://bit.ly/1NLevPT
      if (!Date.now) {
        Date.now = function() {
          return new Date().getTime();
        }
      }
      return Math.floor(Date.now() / 1000);
    }

    // ! Required ! Our API needs the current timestamp for the page
    _paq.push(['setCustomVariable', 1, 'current_ts', current_ts(), 'page']);

    // all params for 'setCustomVariable' are mandatory,
    // see http://piwik.org/docs/custom-variables/ :
    // _paq.push(['setCustomVariable',
    //            $id (must be unique for this $scope !),
    //            $cvar_name,
    //            $cvar_value,
    //            $scope ('page' or 'visit': scope of the custom_var)
    //           ]);
    // Example:
    // _paq.push(['setCustomVariable', 1, '_your_cvar', '$custom_var', 'visit']);

    _paq.push(['trackPageView']);
    _paq.push(['enableLinkTracking']);
    var d=document, g=d.createElement('script'), s=d.getElementsByTagName('script')[0];
    g.type='text/javascript';
    g.defer=true;
    g.async=true;
    g.src=u;
    s.parentNode.insertBefore(g,s);
  })();
</script>
<!-- End Hupilytics -->
```

## How to follow ecommerce actions with hupilytics

### Ecommerce Orders

To follow the ecommerce orders there are two steps : first, you will add the products :

You can add this code to your

```html
[...]
 // add the first product to the order
 _paq.push(['addEcommerceItem',
 "productId", // (required) SKU: Product unique identifier
 "productName", // (optional)
 "productCategory", // (optional) You can also specify an array of up to 5 categories eg. ["productCategory1", "productCategory2", "productCategory3","productCategory4","productCategory5"]
 productPrice, // (recommended) Must be a float or an integer
 productQuantity // (optional, default to 1) Must be an integer
 ]);
 // You can add others products
[...]
 // Specifiy the ecommerce order
 _paq.push(['trackEcommerceOrder',
 "orderId", // (required) Unique Order ID
 orderRevenuTotal, // (required) Order Revenue grand total (includes tax, shipping, and subtracted discount), must be an integer or a float
 orderSub, // (optional) Order sub total (excludes shipping), must be an integer
 orderTaxAmout, // (optional) Must be an integer or a float
 orderShippingAmount, // (optional) Must be an integer or a float
 orderDiscountOffered // (optional) boolean (set to false for unspecified parameter)
 ]);
 // we recommend to leave the call to trackPageView() on the Order confirmation page
 _paq.push(['trackPageView']);
[...]
```
For this features, we recommend to put it on your order confirmation page.

### Cart tracking

When a user add,delete or modify an item to the cart, hupilytics allows to follow user's actions to the cart.

'''html
[...]
 // add the first product to the order
 __paq.push(['addEcommerceItem',
 "productId", // (required) SKU: Product unique identifier
 "productName", // (optional)
 "productCategory", // (optional) You can also specify an array of up to 5 categories eg. ["productCategory1", "productCategory2", "productCategory3","productCategory4","productCategory5"]
 productPrice, // (recommended) Must be a float or an integer
 productQuantity // (optional, default to 1) Must be an integer
 ]);
 // Here it is important to add all other products found in the cart, even the products not updated by the current "Add to cart" click
[...]
 // Records the cart for this visit
 _paq.push(['trackEcommerceCartUpdate',
 cartAmount]); // (required) Must be an integer or a float
 _paq.push(['trackPageView']);
[...]
