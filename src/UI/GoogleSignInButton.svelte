<script>
  const CLIENT_ID = "ENV_CLIENT_ID";

  const metaTag = document.querySelector(
    'meta[name="google-signin-client_id"]'
  );

  let btnSignInOut;
  let btnRevokeAccess;
  let authStatus;

  

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
        // Load the API's client and auth2 modules.
        // Call the initClient function after the modules load.
        console.log('handle client')
        gapi.load("client:auth2", initClient);
        
      }
      
      function initClient() {
        // In practice, your app can retrieve one or more discovery documents.
        var discoveryUrl =
        "https://www.googleapis.com/discovery/v1/apis/drive/v3/rest";
        
        // Initialize the gapi.client object, which app uses to make API requests.
        // Get API key and client ID from API Console.
        // 'scope' field specifies space-delimited list of access scopes.
        gapi.client
          .init({
            apiKey: "AIzaSyDlWLse9HDF_8vwgv9B-zt1xPpxK0YLYp8",
            clientId: CLIENT_ID,
            discoveryDocs: [discoveryUrl],
            scope: SCOPE,
          })
          .then(function () {
            GoogleAuth = gapi.auth2.getAuthInstance();
            // Listen for sign-in state changes.
            GoogleAuth.isSignedIn.listen(updateSigninStatus);

            // Handle initial sign-in state. (Determine if user is already signed in.)
            var user = GoogleAuth.currentUser.get();
            setSigninStatus();
            console.log(user)
            // Call handleAuthClick function when user clicks on
            //      "Sign In/Authorize" button.
            btnSignInOut.click(function () {
              handleAuthClick();
            });
            btnRevokeAccess.click(function () {
              revokeAccess();
            });
          });
      }

      function handleAuthClick() {
        if (GoogleAuth.isSignedIn.get()) {
          // User is authorized and has clicked "Sign out" button.
          GoogleAuth.signOut();
        } else {
          // User is not signed in. Start Google auth flow.
          GoogleAuth.signIn();
        }
      }

      function revokeAccess() {
        GoogleAuth.disconnect();
      }

      function setSigninStatus() {
        var user = GoogleAuth.currentUser.get();
        console.log("user:******", user)
        var isAuthorized = user.hasGrantedScopes(SCOPE);
        if (isAuthorized) {
          btnSignInOut.innerHTML = "Sign out";
          btnRevokeAccess.classList.remove('hide')
          authStatus.innerHTML = 
            "You are currently signed in and have granted " +
              "access to this app."
          ;
        } else {
          btnSignInOut.innerHTML = "Sign In/Authorize";
          btnRevokeAccess.classList.add('hide');
          authStatus.innerHTML = 
            "You have not authorized this app or you are " + "signed out."
          ;
        }
      }

      function updateSigninStatus() {
        setSigninStatus();
      }
</script>

<!-- src switched -->
<!-- src="https://apis.google.com/js/api.js" -->
<svelte:head>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
  <script
  async
  defer
  src="https://apis.google.com/js/platform.js?onload=init"
  on:load={function(){}}
  onreadystatechange="if (this.readyState === 'complete') this.onload()"
></script>


  <!-- <script
    src="https://apis.google.com/js/platform.js?onload=init"
    async
    defer></script> -->
  <!-- <script src="https://apis.google.com/js/platform.js" async defer></script> -->


</svelte:head>

<button id="signin" bind:this={btnSignInOut} on:click={handleClientLoad}>
  Sign In/Authorize
</button>
<button id="signout" class="hide" bind:this={btnRevokeAccess} on:click={handleClientLoad}>
  Revoke access
</button>

<div id="auth-status" bind:this={authStatus} ></div>
<hr />


<style>
  #signout,
  #signin {
    margin-left: 25px;
  }
  .hide {
    display: none;
  }
  #auth-status{
    display: inline;
    padding-left: 25px;
  }
</style>


