<script>
  import GoogleSignInButton from "./UI/GoogleSignInButton.svelte";

  //   put profile vars in arr
  // 	helper f{} for adding/removing classes
  // craete a component & itirate through profileArr to streamline
  let title;

  let fullName;
  let givenName;
  let familyName;
  let imageUrl;
  let email;

  let profileArr=[];

  let img;

  const removeHidden = function (boundElement) {
    boundElement.classList.remove("--hide");
  };
  const addHidden = function (boundElement) {
    boundElement.classList.add("--hide");
  };

    function signInHtml(event) {
  	  const profileDetails = event.detail
  	  profileArr = [profileDetails];

  	  fullName = event.detail.fullName;
  	  givenName = event.detail.givenName;
  	  familyName = event.detail.familyName;
  	  imageUrl = event.detail.imageUrl;
  	  email = event.detail.email;
  	}
  
  // console.log(profileArr, '***', profileArr.familyName)
</script>

<main>
  <h2 class="title" bind:this={title}>sign in with google</h2>
  <GoogleSignInButton on:saveProfileInfo={signInHtml} />
  <img
    class="imageUrl --hide"
    bind:this={img}
    src={imageUrl}
    alt="profile pic"
  />
  <h1 class="fullName --hide" bind:this={fullName}>FULL NAME: {fullName}</h1>
  <h1 class="givenName --hide" bind:this={givenName}>
    GIVEN NAME: {givenName}
  </h1>
  <h1 class="familyName --hide" bind:this={familyName}>
    FAMILY NAME: {familyName}
  </h1>
  <h1 class="email --hide" bind:this={email}>EMAIL {email}</h1>
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }
  .--hide {
    /* display: none; */
    color: red;
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
