---
title: 'Working with Google App Script'
date: 2020-08-04
permalink: /posts/2020/08/working-with-Google-App-Script/
tags:
  - google
  - appscript
  - web
published: true
excerpt: "Google App Script"
---
# [Google App Script](https://developers.google.com/apps-script/overview)
Google Apps Script is a JavaScript development platform, where you can do more with Google products. 

The following examples are taken from [Google App Script's page](https://developers.google.com/apps-script/overview):

* Add custom menus, dialogs, and sidebars to Google Docs, Sheets, and Forms.
Write custom functions and macros for Google Sheets.

* Publish web apps — either standalone or embedded in Google Sites.
Interact with other Google services, including AdSense, Analytics, Calendar, Drive, Gmail, and Maps.

* Build add-ons to extend Google Docs, Sheets, Slides, and Forms, and publish them to the Add-on store.
* Convert an Android app into an Android add-on so that it can exchange data with a user's Google Doc or Sheet on a mobile device.

* and more


Besides the online App Script editor, it is more convienient to develop and manage Apps Script projects from your terminal. This can be achieved by using an open-source tool called [`clasp`](http://g.co/codelabs/clasp).

# Authenticate Clasp to run app locally
https://github.com/google/clasp/blob/master/docs/run.md


# Setup your local environment

* Login globally
{% highlight shell %}
clasp login
{% endhighlight %}

# How to change source
* Make changes locally
* Then
{% highlight shell %} 
clasp push
{% endhighlight %}
to push changes to script.google
* Commit to github

# How run functions
* Make sure you are logged in with `clasp login --status`
* If not login locally, you should ``clasp login --creds creds.json`

The `creds.json` can be obtained from https://console.developers.google.com/apis/credentials?project=ID by downloading OAuth 2.0 Client IDs for application `Clasp`.


* Then
{% highlight shell %}
clasp run
{% endhighlight %}

Open webapp

{% highlight shell %}
clasp open --webapp
{% endhighlight %}

