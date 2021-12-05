<script>
  const CLIENT_ID = "ENV_CLIENT_ID";

  const metaTag = document.querySelector(
    'meta[name="google-signin-client_id"]'
  );

  let btnSignIn;
  let btnSignOut;
  let authStatus;

  let gBtn;
  let currentApiRequest;
  let isAuthorized;
  metaTag.content = CLIENT_ID;

  // window.onSignIn = (googleUser) => {
  //   const profile = googleUser.getBasicProfile();
  //   console.log("ID: " + profile.getId());
  //   console.log("Name: " + profile.getName());
  //   console.log("Image URL: " + profile.getImageUrl());
  //   console.log("Email: " + profile.getEmail());
  // };

  var GoogleAuth;
  var SCOPE = "https://www.googleapis.com/auth/drive.metadata.readonly";

  function handleClientLoad() {
    console.log("client load.... hahahaha");
    gapi.load("client:auth2", initClient);
  }

  function initClient() {
    // generates methods the application can use
    var discoveryUrl =
      "https://people.googleapis.com/$discovery/rest?version=v1";

    gapi.client
      .init({
        apiKey: "AIzaSyDlWLse9HDF_8vwgv9B-zt1xPpxK0YLYp8",
        clientId: CLIENT_ID,
        discoveryDocs: [discoveryUrl],
        scope: SCOPE,
      })
      .then(function () {
        GoogleAuth = gapi.auth2.getAuthInstance();
        GoogleAuth.signIn();
        GoogleAuth.isSignedIn.listen(updateSigninStatus);
        start();
      });
  }
  function start() {
    try {
      let apiRequest = gapi.client.discovery.apis.list();
      let result = JSON.parse(apiRequest.body);
      console.log(result);

      console.log("🐯🐯🐯");
    } catch (e) {
      console.log(e);
    }
  }
  /**
   * Store the request details. Then check to determine whether the user
   * has authorized the application.
   *   - If the user has granted access, make the API request.
   *   - If the user has not granted access, initiate the sign-in flow.
   */
  function sendAuthorizedApiRequest(requestDetails) {
    currentApiRequest = requestDetails;
    if (isAuthorized) {
      // Make API request
      // gapi.client.request(requestDetails)

      // Reset currentApiRequest variable.
      currentApiRequest = {};
    } else {
      GoogleAuth.signIn();
    }
  }

  /**
   * Listener called when user completes auth flow. If the currentApiRequest
   * variable is set, then the user was prompted to authorize the application
   * before the request executed. In that case, proceed with that API request.
   */
  function updateSigninStatus(isSignedIn) {
    if (isSignedIn) {
      isAuthorized = true;
      if (currentApiRequest) {
        sendAuthorizedApiRequest(currentApiRequest);
      }
    } else {
      isAuthorized = false;
      GoogleAuth.signOut();
    }
  }

  // let request = gapi.client.request({
  //   'method': 'GET',
  //   'path': '/v1/contactGroups'
  // })

  // function getContacts() {
  //   request.execute(function(res) {
  //     console.log(res)
  //   })
  // }
</script>

<!-- src="https://apis.google.com/js/api.js" -->
<svelte:head>
  <script
    src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
  <script
    async
    defer
    src="https://apis.google.com/js/platform.js?onload=init"
    on:load={function () {
      console.log("nameless function");
    }}>
  </script>

  <script src="https://apis.google.com/js/platform.js" async defer></script>
</svelte:head>
<svelte:window on:load={handleClientLoad} />

<h2 class="alert alert-primary">sign in with google</h2>
<button class="g-signin2 " bind:this={gBtn}>sign in</button>

<!-- <button id="getList" on:click={getContacts}>get contacts</button> -->
<style>
  .--hide {
    display: none;
  }
  .g-signin2 {
    position: absolute;
    top: 50%;
    left: 50%;
  }
</style>
