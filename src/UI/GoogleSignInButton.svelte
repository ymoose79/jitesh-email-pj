<script>
  const CLIENT_ID = "ENV_CLIENT_ID";

  const metaTag = document.querySelector(
    'meta[name="google-signin-client_id"]'
  );

  let currentApiRequest;
  let isAuthorized;
  let gBtn;
  metaTag.content = CLIENT_ID;

  // window.onSignIn = (googleUser) => {
  //   const profile = googleUser.getBasicProfile();
  //   console.log("ID: " + profile.getId());
  //   console.log("Name: " + profile.getName());
  //   console.log("Image URL: " + profile.getImageUrl());
  //   console.log("Email: " + profile.getEmail());
  // };

  let GoogleAuth;
  var SCOPE = "https://www.googleapis.com/auth/drive.metadata.readonly";

  // fires when window loads:  
  function handleClientLoad() {
    console.log("client load.... hahahaha");
    gapi.load("client:auth2", initClient);
  }
  
  function initClient() {
    // generates methods the application can use
    var discoveryUrl =
    "https://people.googleapis.com/$discovery/rest?version=v1";
    
    gapi.client.init({
      apiKey: "AIzaSyDlWLse9HDF_8vwgv9B-zt1xPpxK0YLYp8",
      clientId: CLIENT_ID,
      discoveryDocs: [discoveryUrl],
      scope: SCOPE,
    });
  }
  
  // fires on:click
  const handleAuth = () => {
    if (gapi !== undefined) {
      GoogleAuth = gapi.auth2.getAuthInstance();
      GoogleAuth.signIn();
      GoogleAuth.isSignedIn.listen(updateSigninStatus);
      start()
    }
  };

  // makes api call
  function start() {
    try {
      let apiRequest = gapi.client.people.contactGroups.list()
      console.log('apiRequest', apiRequest);
      // let result = JSON.parse(apiRequest.body);
      // console.log(result);
    } catch (e) {
      console.log(e);
    }
  }
  /**---------------> sendAuthorizedApiRequest() currently not doing anything.....
   * Store the request details. Then check to determine whether the user
   * has authorized the application.
   *   - If the user has granted access, make the API request.
   *   - If the user has not granted access, initiate the sign-in flow.
   */
  // function sendAuthorizedApiRequest(requestDetails) {
  //   currentApiRequest = requestDetails;
  //   if (isAuthorized) {
  //     // Make API request
  //     // gapi.client.request(requestDetails)

  //     // Reset currentApiRequest variable.
  //     currentApiRequest = {};
  //   } else {
  //     GoogleAuth.signIn();
  //   }
  // }

  /**
   * Listener called when user completes auth flow. If the currentApiRequest
   * variable is set, then the user was prompted to authorize the application
   * before the request executed. In that case, proceed with that API request.
   */


  //  following function has to exist, but you will never see an 🐔
  function updateSigninStatus(isSignedIn) {
    console.log('🐔')
    if (isSignedIn) {
      console.log('🐔')
      isAuthorized = true;
      if (currentApiRequest) {
        sendAuthorizedApiRequest(currentApiRequest);
      }
    } else {
      console.log('🐔')
      isAuthorized = false;
    }
  }

</script>

<!-- src="https://apis.google.com/js/api.js" -->
<svelte:head>
  <script
    async
    defer
    src="https://apis.google.com/js/platform.js?onload=init"
    on:load={handleClientLoad}>
  </script>
</svelte:head>
<!-- <svelte:window on:load={handleClientLoad} /> -->

<h2 class="alert alert-primary">sign in with google</h2>
<button class="g-signin2x" bind:this={gBtn} on:click={handleAuth}
  >sign in</button
>

<style>
  .--hide {
    display: none;
  }
  .g-signin2x {
    position: absolute;
    top: 50%;
    left: 50%;
  }
</style>
