<script>
  import GoogleSignIn from "./UI/GoogleSignIn.svelte";
  import SendEmail from "./UI/SendEmail.svelte";
  import DraftId from "./UI/DraftId.svelte";

  //   event.details from GoogleSignIng.svelte
  let fullName;
  let imageUrl;
  let isAuthorized;
  // 	array w/ those key:value pairs stored
  let profileArr;

  // checkbox
  let yes = false;
  let draftSubject="";

  //   assigns variables via GoogleSignIn
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
    <GoogleSignIn on:saveProfileInfo={signInHtml} />
  {:else}
    <img class="image " src={imageUrl} alt="profile pic" />
    <h1 class="fullName ">FULL NAME: {fullName}</h1>
    <div class="labelBox">
      <label
        ><input type="checkbox" id="checkbox" bind:checked={yes} />Do you want
        to pull a template from a draft?</label
      >
      {#if yes}
       <DraftId {draftSubject}/>
      {/if}
    </div>
    <SendEmail {draftSubject} />
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

    /* checkbox */
  .labelBox {
    margin: 2rem;
  }
  #checkbox {
    margin-right: 1rem;
  }
</style>
