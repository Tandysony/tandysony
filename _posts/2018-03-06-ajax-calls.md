---
layout: post
title: AJAX - Getting Data without Page Refresh
tags: [Ajax, Django]
---

## Background

Have been working on both back-end and front-end projects for years, more and more circumstances do I face for getting and posting data from/to the server. On traditional Django applications, I just send data from a server to a page in a Django View by rendering a HTML template. This is a so convenient that I can develop a website prototype in a very short amount of time. However, there are four issues when the project get more complex:

- Back-end and front-end are **highly coupled**;
- Templates are rendered at the server side, **adding load** to a server. Remember, computing power of a server, e.g., AWS EC2, Azure cognitive services, is expensive for SMEs (small and medium enterprises);
- If rendering massive data to a page, but pulling from different Django models, client-side users will experience **very slow page loading**;
- Whenever getting data from and post data to a server, mostly I encountered **page refresh**.

To resolve these issues, `Django REST Framework (DRF)` and `Ajax` are coming to save the world.

- **DRF** decouples back-end and front-end. The decoupling makes web development an easier and enjoyable process. Back-end server provides and processes data only, no page rendering involved. Front-end UI consumes and sends data only. In this case, you can enjoy any front-end framework for the development, for example, `React`, `Vue`, and `Angular`.
- **Ajax** make page refresh irrelevant.

However, DRF is beyond the topic of this blog. Let go back to focus on three (personally) mostly used Ajax calls, to get data.

### 1. jQuery `$.ajax`

Many of us are already familiar with jQuery. `$.ajax` is a quick and intuitive solution to make AJAX calls. And, the API is just simple:

```js
loadDataFromServer: function() {
    $.ajax({
      method: 'GET',
      url: 'my_url_endpoint',  // A RESTful URI
      dataType: 'json',
      cache: false,
      success: function(result,status,xhr) {
        // process the result
      },
      error: function(xhr,status,error) {
        // process error
      }
    });
  }
```

This is pretty much everything we need. And, it is recommended if you are already user jQuery library in your front-end development. Of course, there are two short-handed versions to do Ajax calls: `$.get()` and `$.post()`. Please refer to [jQuery AJAX Methods](https://www.w3schools.com/jquery/jquery_ref_ajax.asp) for more information.

### 2. Fetch API

The **Fetch API** is a new, simple and standardized API that aims to unify fetching across the web and replace `XMLHttpRequest`, but the new API provides a more powerful and flexible feature set. It has a [polyfill](https://github.com/github/fetch) for older browsers and should be used in modern web apps. If you are making API calls in Node.js you should also check out [node-fetch](https://github.com/bitinn/node-fetch) which brings fetch to Node.js.

Here is what the modified API call looks like:

```js
loadDataFromServer: function() {
    fetch('my_url_endpoint').then(function(response){
        // process the response
    });
}
```

Please refer to [Mozilla - Fetch_API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) for more information.

### 3. Axios

[Axios](https://github.com/axios/axios) is a promise based HTTP client for Node.js and browser. Like fetch and superagent, it can work on both client and server. It is officially recommended for `Vue.js`.

Here is how you make an API call using Axios:

```js
loadDataFromServer: function() {
    axios.get('my_url_endpoint').then(function(response){
      // process the response

    }).catch(function(error){
      // process the error

    });
}
```

Refer to its GitHub page for many other useful features.

## Conclusion

If you are using jQuery as a library in your font-end code, just use `$.ajax` to get the job done.

If you are using modern vanilla JavaScript (ES6/ES2015+) or `React`, `Fetch API (fetch)` is a good option.

If you are using `Vue.js` like I do, use `axios`.

However, there is not an arbitrary choice. You can use `fetch` or `axios` for either `Vue.js` or `React`, but avoid `$.ajax`.