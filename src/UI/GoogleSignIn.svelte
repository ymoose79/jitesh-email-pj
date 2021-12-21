<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  const CLIENT_ID = "ENV_CLIENT_ID";

  const metaTag = document.querySelector(
    'meta[name="google-signin-client_id"]'
  );

  let currentApiRequest;
  let isAuthorized;
  let btnIn;
  let btnOut;

  metaTag.content = CLIENT_ID;

  let GoogleAuth;
  let SCOPES = "https://www.googleapis.com/auth/gmail.send";

  // fires when window loads:
  function handleClientLoad() {
    gapi.load("client:auth2", initClient);
  }

  function initClient() {
    // generates methods the application can use
    var discoveryUrl =
      "https://gmail.googleapis.com/$discovery/rest?version=v1";

    gapi.client.init({
      apiKey: "AIzaSyDlWLse9HDF_8vwgv9B-zt1xPpxK0YLYp8",
      clientId: CLIENT_ID,
      discoveryDocs: [discoveryUrl],
      scope: SCOPES,
      // means that we do not prompt the user with a login/permissions modal if not authenticated
      immediate: true,
    });
  }

  // fires on:click
  const handleAuth = () => {
    if (gapi !== undefined) {
      GoogleAuth = gapi.auth2.getAuthInstance();
      GoogleAuth.signIn();
      GoogleAuth.isSignedIn.listen(updateSigninStatus);
      start();
    }
  };

  const signOut = () => {
    GoogleAuth.signOut();
    btnIn.classList.remove("--hide");
    btnOut.classList.add("--hide");
  };

  // makes api call
  function start() {
    let profile = GoogleAuth.currentUser.get().getBasicProfile();
    dispatch("saveProfileInfo", {
      fullName: profile.getName(),
      imageUrl: profile.getImageUrl(),
      isAuthorized: true,
    });
  }
  
  function updateSigninStatus(isSignedIn) {
    if (isSignedIn) {
      isAuthorized = true;
      console.log("👌");
      if (currentApiRequest) {
        sendAuthorizedApiRequest(currentApiRequest);
      }
    } else {
      isAuthorized = false;
      console.log("⭕️");
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

<button class="sign_in" bind:this={btnIn} on:click={handleAuth}>sign in</button>
<!-- <button class="sign_out" bind:this={btnOut} on:click={signOut}>sign out</button> -->

<style>
/* 
  button {
    position: absolute;
    top: 50%;
    left: 50%;
  } */
</style>
