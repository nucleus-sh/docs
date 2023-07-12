---
description: >-
  There are few steps you should take to protect and inform the privacy of your
  users.
---

# 📖 Compliance

### GDPR & PECR <a href="#gdpr--pecr" id="gdpr--pecr"></a>

Nucleus by default doesn't use or collect IP adresses or any personal information about your users. You can see the total list of data we collect on [this page](https://www.nucleus.sh/transparency).

We do not use cookies even in the Electron version of our software.&#x20;

The only data we store on user devices is a small cache file in case users go offline for the data to be sent later.

That means you are not required by law to ask consent. _However_, we recommend you to read and follow the practices below.&#x20;

{% hint style="info" %}
Whether you need to ask consent to your users before tracking depends on if you plan to report personal identifiable data (with `setProps` or `setUserId`).
{% endhint %}

### Ask Consent... <a href="#ask-consent" id="ask-consent"></a>

This is in the case where you intend to submit personal identifiable information to our servers (by tying personal data and user ID to events you submit).

Ask before starting the analytics session. We recommend doing that before you even load the modules.

Provide the option to your users to disable/enable tracking as they wish.&#x20;

Nucleus' modules support doing that with the `enableTracking()` and `disableTracking()` modules.

### ... or Anonymise <a href="#or-anonymise" id="or-anonymise"></a>

Else, if you still choose to track individual users and report personal data, you should assign them an ID that doesn't allow to identify the original person.&#x20;

That's why email adresses are bad ideas.&#x20;

The same goes when tying properties to your users, those should be at the best extent not personal identifiable informations.

### Inform <a href="#inform" id="inform"></a>

In both cases, you should inform your users what data will be collected about them.

Although you are not required to if you don't submit any personal information, we recommend you mention us in your privacy policy to better inform your users.

If that helps, you can link to our [data transparency](https://www.nucleus.sh/transparency) page that list all the data we collect and store (that doesn't include IP adresses).

Example:

> To get information about the devices and behavior of our users, we use Nucleus to provide analytics. This service gives us insight about how users interact with our software. It does not store any personal identifiable information. Visit their [transparency](https://nucleus.sh/transparency) page to get the full list of data they collect.
