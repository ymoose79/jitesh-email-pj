<script>
  export let to = "justin.r.stock@gmail.com";
  export let name = "Justin Stock";

  let files = [{
    type: "txt/plain",
    name: "xyz"
  },{
    type: "txt/plain",
    name: "abvc"
  }];
  
  let multipleFiles='';
  
  
  // ------------------> contents of file(?)
  let data;
  //   $: {
  //     if (files && files[0]) {
  //         let binfile = files[0];
  //         let reader = new FileReader();
  //         reader.onload = function(evt) {
  //             data = new Uint8Array(evt.target.result);
  //         }
  //         reader.readAsArrayBuffer(binfile);
  //     }
  // }


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

  //   Content-Type: text/markdown; name="new.txt"
  // Content-Disposition: attachment; filename="new.txt"
  function formatFile() {
    files.forEach((file) => {
      multipleFiles += "--foo_bar_baz\n";
      for (let key in file) {
        if (`${[key]}` === "type") {
          multipleFiles += `Content-Type: ${file[key]}\n`;
        } else if (`${[key]}` === "name")
          multipleFiles += `Content-Disposition: attachment; filename="${file[key]}\n\n\n`;
      }
      multipleFiles += `line 1\n`;
      multipleFiles += `line 2\n`;
    });
    sendsEmail(multipleFiles)
    console.log(multipleFiles);
  }
</script>

<form on:submit|preventDefault>
  <input type="file" bind:files multiple />
  <input type="text" placeholder="TO email address" bind:value={to} />
  <input type="text" placeholder="FROM name" bind:value={name} />
  <button on:click={loadGmailApi}>Send Email</button>
</form>
<!-- <textarea cols="25">{data}</textarea> -->
