<script>
  export let to = "justin.r.stock@gmail.com";
  export let name = "Justin Stock";
  export let body = ""

  let files = "";

  const toBase64 = (file) =>
    new Promise((res, rej) => {
      const reader = new FileReader();
      reader.readAsDataURL(file);
      reader.onload = () => res(reader.result);
      reader.onerror = (err) => rej(err);
    });


  async function sendEmail() {
    if (!files[1]) {
      const file = files[0];
      const data = await toBase64(file);
      const dataArr = data.split(',')
     const x = dataArr.shift()
     const readyToSendData = dataArr.join();
      formatFile(readyToSendData);
    }
  }

  function formatFile(data) {
    // ---------------- send 1 attachment
    let multipleFiles = "";
    if (!files[1]) {
      multipleFiles += `Content-Type: ${files[0].type} name="${files[0].name}"\n`;
      multipleFiles += `Content-Transfer-Encoding: base64\n`;
      multipleFiles += `Content-Disposition: attachment; filename=${files[0].name}\n\n`;

      multipleFiles += data;

      forwardAttachments(multipleFiles);
    }
    // ---------------- send multiple attachment
    // else {
    //   for (let i = 0, numFiles = files.length; i < numFiles; i++) {
    //     multipleFiles += "--foo_bar_baz\n";
    //     multipleFiles += `Content-type: ${files[i].type} name="${files[i].name}"\n`;
    //     multipleFiles += `Content-Disposition: attachment; filename=${files[i].name}\n\n`;
    //     multipleFiles += `waiting on data....\n\n`;
    //   }
    //   forwardAttachments(multipleFiles);
    // }
  }


  function forwardAttachments(multipleFiles) {
    let base64EncodedEmail = btoa(
      //   `Content-Type:  text/plain; charset="UTF-8"
      `Content-Type: multipart/mixed; boundary="foo_bar_baz"
Content-Transfer-Encoding: message/rfc2822
To: ${to}
From: "${name}" <justin.r.stock@gmail.com>
Subject: Hello world

--foo_bar_baz
Content-Type: text/plain

${body}

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
    request.execute();
  }
</script>

<form on:submit|preventDefault={sendEmail}>
  <input type="file" bind:files multiple />
  <label
    >To:
    <input type="text" placeholder="TO email address" bind:value={to} />
  </label>
  <label
    >From:
    <input type="text" placeholder="FROM name" bind:value={name} />
  </label>
  <label
  >Body:
  <textarea rows="10" cols="50" bind:value={body} />
</label>
  <button>Send Email</button>
</form>
