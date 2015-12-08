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
    var u = "http://api.catchbox.hupi.io/v1/(:account)/hupilytics";
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
