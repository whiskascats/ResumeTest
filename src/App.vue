<script setup>
  function start() {
    // 2. Initialize the JavaScript client library.
    gapi.client.init({
      'apiKey': 'AIzaSyDkBfn-w8Km8h43AsxgI9ia2vT7CIscKQE',
      // Your API key will be automatically added to the Discovery Document URLs.
      'discoveryDocs': ['https://people.googleapis.com/$discovery/rest'],
      // clientId and scope are optional if auth is not required.
      'clientId': '66875778035-qvgp2lv0qrv7o03vmnt4396445qpcp4r.apps.googleusercontent.com',
      'scope': 'profile',
    }).then(function() {
      // 3. Initialize and make the API request.
      return gapi.client.people.people.get({
        'resourceName': 'people/me',
        'requestMask.includeField': 'person.names'
      });
    }).then(function(response) {
      console.log(response.result);
    }, function(reason) {
      console.log('Error: ' + reason.result.error.message);
    });
  };
  // 1. Load the JavaScript client library.
  gapi.load('client', start);
</script>

<template>
  123
</template>
