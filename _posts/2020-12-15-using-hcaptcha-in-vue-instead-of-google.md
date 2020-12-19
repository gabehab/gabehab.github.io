---
layout: post
title: "Using hCaptcha instead of Google's ReCaptcha with Vue and Express"
date: 2020-12-15 17:10:48 -0800
categories: [development, degoogle]
tags: [vue, captcha, node, web-development, programming, google]
---

For much the same reasons as my last [captcha related post](/development/opinion/2020/03/06/google-captcha-express-js.html), when needing a form of bot deterrence, it makes sense to use a captcha prodiver. To quickly sum it up, using a provider allows for an easy means of stopping most bots abilities to interact with an application. Historically, implementing a useful, and not-too-impossible challenge-test was difficult and chalk-full of issues (accessibility, language, buggy). With that said, there is no excuse into todays world to rely on a privacy-invading, ad-selling organization like Google for such a tool. This is where [hCaptcha](https://hCaptcha.com/?r=1d541b532693) comes in. Please note that I've used my referral link to hCaptcha (although it remains unknown what that actually provides me, but just fyi).

Changing providers couldn't be more straightforward as hCaptcha allows for a direct drop-in replacement for most of [Google ReCaptcha's](https://developers.google.com/recaptcha/) API and features. Furthermore, hCaptcha and the community [provide libraries](https://github.com/hCaptcha) for both frontend and backend environments, meaning its extremely easy to integrate and include as a dependency.

As for how to implement a captcha service effectively, it needs to be implemented _around_ frontend interactions and verified on the backend. That is, gate your expensive interaction (i.e. registering for an account) and shoot a request to your APIs `/verify` endpoint after the user completes the test. From there, the server confirms with hCaptcha that the generated frontend token is valid. This acts as a pass/no-pass middleware before the actual requested endpoint is hit.

For this article, I'll be using [vue-hcaptcha](https://github.com/hCaptcha/vue-hcaptcha) which is a simple wrapper of their JS API with all the necessary vue-ification (e.g. events) that make it easy to drop in a Vue application. As for the backend, I've decided to use a [community package](https://github.com/vastus/node-hcaptcha) for Node. Again, this package is also a simple wrapper, but done well, with zero external dependancies so no point rewriting whats already implemented!

## Integrating Google ReCaptcha V3 with an Express application

The [documentation](https://developers.google.com/recaptcha/docs/v3) provided by Google is reasonably helpful - but an actual example is better!

### The Frontend setup!

First, add the correct ReCaptcha `<script />` to your client-side-facing template (Load it earlier enough to provide enough data for a more accurate response). Something like:

```html
<script src="https://www.google.com/recaptcha/api.js?render=_reCAPTCHA_site_key&onload=onLoad&render=explicit"></script>
```

Next, configure how you want the script to inject itself into the form you wanted guarded. I add the callback function in the script's `src` to inject the captcha value into the form.

```javascript
function onLoad() {
  grecaptcha.ready(function () {
    grecaptcha
      .execute(`${captchaToken}`, { action: "submit" })
      .then(function (token) {
        // Find the hidden input element in the form I wanted validated
        let t = document.querySelector("#reCaptcha");
        // give it a value returned by ReCaptcha
        t.value = token;
      });
  });
}
```

```HTML
    <form>
        <!-- Other form stuff -->
        <input id="reCaptcha" type="hidden" name="g-recaptcha-response">
    </form>
```

All in all, all that's happening is the script loads, calls the function, injects a reCaptcha token into the form that we can validate against on the server. Simple stuff!

### Setting up the Backend

We need to be able to parse the form, grab the token value, and then shoot out a call to the reCaptcha service and see if the submitted form can be trusted.

In Express, a simple middleware that can parse and shootout a request to the service is just what the Doctor ordered. It provides a portable way to pick and choose which routes need to be validated, and allows an easy way to reject requests.

```javascript
function verifyRecaptcha(req, res, next) {
  const captchaURL = `https://www.google.com/recaptcha/api/siteverify`;
  // Get the token from the form
  const key = req.body["g-recaptcha-response"];

  if (!key) throw new Error("Captcha could not be verified. Please try again.");

  // axios works in Node, isn't that nice?
  axios({
    url: captchaURL,
    method: "POST",
    // reCaptcha demands x-www-form-urlencoded requests
    headers: { ContentType: "application/x-www-form-urlencoded" },
    body: `secret=${SECRET}&response=${req.body["g-recaptcha-response"]}`,
  })
    .then((captchaRes) => {
      const data = captchaRes.data;
      // check if successfully requested, and that a score over .5 is met
      if (data.success === true && data.score > 0.5) {
        next();
      }
      throw new Error("Captcha could not be verified. Please try again.");
    })
    .catch((error) => {
      next(error);
    });
}
```

The function grabs the token from the form, shoots a POST request to reCaptcha, and then checks if its successful and returns a high enough score to continue on to the next step.

A route looks very familiar.

```javascript
router.route("/login", verifyRecaptcha).get(handleRoute);
```

And that's it! A really straightforward way to provide some protection and judge user interactions.
