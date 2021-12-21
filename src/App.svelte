<script>
  import GoogleSignInButton from "./UI/GoogleSignInButton.svelte";
import SendEmail from "./UI/SendEmail.svelte"
  let title;

  //   event.details from GoogleSignInButtong.svelte
  let fullName;
  let givenName;
  let familyName;
  let imageUrl;
  let email;
  let isAuthorized;
  // 	array w/ those key:value pairs stored
  let profileArr;


//   assigns variables via GoogleSignInButton
  function signInHtml(event) {
    const profileDetails = event.detail;
    profileArr = profileDetails;
    fullName = event.detail.fullName;
    imageUrl = event.detail.imageUrl;

    isAuthorized = event.detail.isAuthorized;
  }

</script>

<main>
  {#if !isAuthorized}
    <h2 class="title">sign in with google</h2>
    <GoogleSignInButton on:saveProfileInfo={signInHtml} />
  {:else}
    <img class="image " src={imageUrl} alt="profile pic" />
    <h1 class="fullName ">FULL NAME: {fullName}</h1>
	<SendEmail/>
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h2 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
