<script>
  export let to = "justin.r.stock@gmail.com";
  export let name = "Justin Stock";
  let files;
  $: console.log({files});
  const CLIENT_ID = "ENV_CLIENT_ID";
  let apiKey = "AIzaSyDlWLse9HDF_8vwgv9B-zt1xPpxK0YLYp8";
  let scopes = "https://www.googleapis.com/auth/gmail.send";

  function checkAuth() {
    gapi.auth.authorize(
      {
        client_id: CLIENT_ID,
        scope: scopes,
        immediate: true,
      },
      handleAuthResult
    );
  }

  function handleAuthClick() {
    gapi.auth.authorize(
      {
        client_id: CLIENT_ID,
        scope: scopes,
        immediate: false,
      },
      handleAuthResult
    );
    return false;
  }

  function handleAuthResult(authResult) {
    if (authResult && !authResult.error) {
      console.log(authResult);
      loadGmailApi();
      // $('#authorize-button').remove();
      // $('.table-inbox').removeClass("hidden");
    }
    //   else {
    // $('#authorize-button').removeClass("hidden");
    // $('#authorize-button').on('click', function(){
    //   handleAuthClick();
    // });
    //   }
  }

  function loadGmailApi() {
    gapi.client.load("gmail", "v1", displayInbox());
  }

  function displayInbox() {
    // let request = gapi.client.gmail.users.messages.list({
    //   userId: "me",
    //   labelIds: "INBOX",
    //   maxResults: "10",
    // });

    let base64EncodedEmail = btoa(
    //   `Content-Type:  text/plain; charset="UTF-8"
`Content-Type: multipart/mixed; boundary="foo_bar_baz"
Content-Transfer-Encoding: message/rfc2822
To: ${to}
From: "${name}" <justin.r.stock@gmail.com>
Subject: Hello world

--foo_bar_baz
Content-Type: text/plain

The actual message text goes here
--foo_bar_baz
Content-Type: text/plain; name="test.txt"
Content-Disposition: attachment; filename="test.txt"

line 1
line 2
--foo_bar_baz--
`
    )
      .replace(/\+/g, "-")
      .replace(/\//g, "_");

    let request = gapi.client.gmail.users.messages.send({
      userId: "me",
      resource: {
        raw: base64EncodedEmail,
      },
    });
    request.execute(function (res) {
      console.log(res.messages);
    });
  }
</script>

<input type="file" bind:files={files} multiple>
<input type="text" placeholder="TO email address" bind:value={to} />
<input type="text" placeholder="FROM name" bind:value={name} />
<button on:click={checkAuth}>Send Email</button>
