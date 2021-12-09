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

  // window.onSignIn = (googleUser) => {
  //   const profile = googleUser.getBasicProfile();
  //   console.log("ID: " + profile.getId());
  //   console.log("Name: " + profile.getName());
  //   console.log("Image URL: " + profile.getImageUrl());
  //   console.log("Email: " + profile.getEmail());
  // };

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
      btnOut.classList.remove("--hide");
      btnIn.classList.add("--hide");
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
      givenName: profile.getGivenName(),
      familyName: profile.getFamilyName(),
      imageUrl: profile.getImageUrl(),
      email: profile.getEmail(),
      isAuthorized: true,
    });
    // try {
    //   let apiRequest = gapi.client.people.contactGroups.list();
    //   console.log("apiRequest", apiRequest);
    //   // let result = JSON.parse(apiRequest.body);
    //   // console.log(result);
    // } catch (e) {
    //   console.log(e);
    // }
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
<button class="sign_out --hide" bind:this={btnOut} on:click={signOut}
  >sign out</button
>

<style>
  .--hide {
    display: none;
  }
  button {
    position: absolute;
    top: 50%;
    left: 50%;
  }
</style>
