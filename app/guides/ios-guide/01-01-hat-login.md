---
layout: twoColumn
section: guides
type: iOS Developer's Guide
guide: 
    name: ios-guide
    step: 01-01-hat-login
title: "- Step 1 - Redirecting user to HAT Login"
description: Step 1 to authenticating HAT applications on the iOS platform
---
# Step 1 - Redirect user to HAT Login

To log the user in with their HAT, you need to ask for their HAT address. Depending on the status of your app within the HAT ecosystem you may have also have a specific `application name`, an allowed `success redirect url` and an allowed `fallback redirect url` for the user to be sent to to complete authentication. If you do not have these details, you can put any `application name`, `success redirect url` and `fallback redirect url`, however the authentication token you will receive will not grant you any permissions to do any operations on the HAT but verify that the token really came from the HAT. You can find more details about the step 1 [here](https://developers.hubofallthings.com/guides/hat_login/01-send-hat-login.html).

For an `iOS` application that means asking user to type the `HAT Address`. Something very simple can be like the next image:

![alt text](https://github.com/Hub-of-all-Things/exchange-assets/blob/master/iOS-Guide-Images/Enter-HAT-Address.png?raw=true "type address")

In the above image user is being asked to fill the HAT address, `test`, and select a domain, `.hubofallthings.net`. There can be other domains as well, e.g.: `hat.direct`. We thought that splitting the address and the domain made for a better UX, but this is not a requirement for your app. You could have one `UITextField` that the user will have to type the full address, `test.hubofallthings.net`

Having asked user to fill in the `HAT address`, e.g. `test.hubofallthings.net`, you have to send them to `"https://\(hatAddress)/#/hatlogin?name=\(applicationName)&redirect=\(redirectURL)&fallback=\(fallbackRedirectURL)"` endpoint of the `HAT`, where:

* `hatAddress` is the (fully qualified domain) address of the HAT, e.g. `test.hubofallthings.net`
* `applicationName` is the name of your application on HAT. This is defined once when you complete the form to create a new application. e.g `testing`
* `redirectURL` is the URL where the user should be sent to after completing authentication. ***Optional***. For an `iOS` application that would probably be: `applicationName://success` and has to be added in the `info.plist` file of the project in Xcode as a URL scheme
* `fallbackRedirectURL` is the URL where the user should be sent to in case the authentication has failed. ***Optional***. For an `iOS` application that would probably be: `applicationName://failed` and has to be added in the `info.plist` file of the project in Xcode as a URL scheme

In an `iOS` application, in order to redirect the user to an existing `HAT address` and proceed to the next step, we have to use `Safari` to open the `URL`. This is achieved by using the various ways `iOS` offers, e.g. `SFSafariViewController`.

To achieve this with `SFSafariViewController` you have to initialize an instance of `SFSafariViewController` with the login address described above, `"https://\(hatAddress)/#/hatlogin?name=\(applicationName)&redirect=\(redirectURL)&fallback=\(fallbackRedirectURL)"`, and then via a `UIViewController` present the instance of `SFSafariViewController`. e.g.:

```rawnoselect
import SafariServices

let safariViewController: SFSafariViewController = SFSafariViewController(url: authURL)
viewController.present(safariViewController, animated: animated, completion: completion)
```

That will launch safari within the app and load the url specified.

<nav class="pager-nav">
<a href="" style="display:none;"></a>
<a href="01-02-user-login.html">Next Step: Step 2 - Logging in the User</a>
</nav>