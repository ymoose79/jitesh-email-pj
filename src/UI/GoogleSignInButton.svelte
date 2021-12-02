<script>
    window.onSignIn = (googleUser) => {
       const profile = googleUser.getBasicProfile();
       console.log('ID: ' + profile.getId());
       console.log('Name: ' + profile.getName());
       console.log('Image URL: ' + profile.getImageUrl());
       console.log('Email: ' + profile.getEmail());
    };
    const metaTag = document.querySelector('meta[name="google-signin-client_id"]')
    metaTag.content = "159286373359-geta9ia939ah6np21elk8ol2j7rgv4tr.apps.googleusercontent.com"

  </script>
<svelte:head>
  <script src="https://apis.google.com/js/platform.js" async defer></script>
</svelte:head>

<div class="g-signin2" data-longtitle="true" data-onsuccess="onSignIn" />