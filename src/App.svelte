<script>
  import GoogleSignInButton from "./UI/GoogleSignInButton.svelte";
import EmailButton from "./UI/SendEmailButton.svelte"
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


//   helper f{}s to manage DOM
  const removeHidden = function (boundElement) {
    boundElement.classList.remove("--hide");
  };
  const addHidden = function (boundElement) {
    boundElement.classList.add("--hide");
  };


//   assigns variables via GoogleSignInButton
  function signInHtml(event) {
    const profileDetails = event.detail;
    profileArr = profileDetails;
    console.log(profileArr);

    fullName = event.detail.fullName;
    givenName = event.detail.givenName;
    familyName = event.detail.familyName;
    imageUrl = event.detail.imageUrl;
    email = event.detail.email;

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
    <h1 class="givenName ">
      GIVEN NAME: {givenName}
    </h1>
    <h1 class="familyName ">
      FAMILY NAME: {familyName}
    </h1>
    <h1 class="email">EMAIL {email}</h1>
	<EmailButton/>
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
