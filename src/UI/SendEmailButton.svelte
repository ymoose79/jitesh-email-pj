<script>
  export let to = "justin.r.stock@gmail.com";
  export let name = "Justin Stock";

  let files = "";

  function loadGmailApi() {
    gapi.client.load("gmail", "v1", formatFile());
  }

  function sendsEmail(multipleFiles) {
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
${multipleFiles}
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
      console.log(res);
    });
  }

  function formatFile() {
    // ---------------- send 1 attachment
    let multipleFiles = "";
    if (!files[1]) {
      multipleFiles += `Content-type: ${files[0].type} name="${files[0].name}"\n`;
      multipleFiles += `Content-Disposition: attachment; filename=${files[0].name}\n\n`;
      multipleFiles += `waiting on data....`;

      sendsEmail(multipleFiles);
    }
    // ---------------- send multiple attachment
    else {
      for (let i = 0, numFiles = files.length; i < numFiles; i++) {
        multipleFiles += "--foo_bar_baz\n";
        multipleFiles += `Content-type: ${files[i].type} name="${files[i].name}"\n`;
        multipleFiles += `Content-Disposition: attachment; filename=${files[i].name}\n\n`;
        multipleFiles += `waiting on data....`;
      }
      sendsEmail(multipleFiles);
    }
  }
</script>

<form on:submit|preventDefault={loadGmailApi}>
  <input type="file" bind:files multiple />
  <label
    >To:
    <input type="text" placeholder="TO email address" bind:value={to} />
  </label>
  <label
    >From:
    <input type="text" placeholder="FROM name" bind:value={name} />
  </label>
  <button>Send Email</button>
</form>
