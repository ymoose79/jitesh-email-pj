<script>
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  export let draftSubject;
  let narrowYourSearch;

  // function checkLengthOfDraft() {
  //   let messageBody = "";
  //   let rawMessageBody = "";
  //   if (draftId.length === 1) {
  //     return gapi.client.gmail.users.drafts
  //       .get({
  //         userId: "me",
  //         id: draftId,
  //       })
  //       .then(function (res) {
  //         let dataWithAttach = res.result.message.payload;
  //         let data = res.result.message.payload.parts[0];

  //         if (!data.parts) {
  //           rawMessageBody = data.body.data;
  //           messageBody = atob(rawMessageBody);
  //           dispatch("draftIdMessage", messageBody);
  //         } else {
  //           let attachmentIds = [];
  //           rawMessageBody = data.parts[0].body.data;
  //           messageBody = atob(rawMessageBody);

  //           let dataAttachArr = dataWithAttach.parts;

  //           for (let i = 1; i < dataAttachArr.length; i++) {
  //             getAttachments(dataAttachArr[i].body.attachmentId);
  //             let fields = {
  //               type: dataAttachArr[i].mimeType,
  //               name: dataAttachArr[i].filename,
  //             };
  //             attachmentIds = [...attachmentIds, fields];
  //           }
  //           dispatch("draftIdMessage", messageBody);
  //           dispatch("attachmentNameAndTyeps", attachmentIds);
  //         }
  //       });
  //   }
  // }

  function checkLengthOfDraft(draftId) {
    let body = "";
    let rawMessageBody = "";

    return gapi.client.gmail.users.drafts
      .get({
        userId: "me",
        id: draftId,
      })
      .then(function (res) {
        let data = res.result.message.payload.parts[0];
        if (!data.parts) {
          rawMessageBody = data.body.data;
          body = atob(rawMessageBody);
        } else {
          rawMessageBody = data.parts[0].body.data;
          body = atob(rawMessageBody);
        }
        dispatch("draftIdMessage", body);
      });
  }

  function execute() {
    return gapi.client.gmail.users.drafts
      .list({
        userId: "me",
        maxResults: 5,
        q: draftSubject,
      })
      .then(function (res) {
        let draftIds = res.result.drafts;
        if (draftIds.length > 1) {
          narrowYourSearch = true;
        } else {
          narrowYourSearch = false;
          checkLengthOfDraft(draftIds[0].id);
        }
      });
  }
</script>

<label>
  <input
    type="text"
    name="draftSearch"
    id="draftSearch"
    bind:value={draftSubject}
  />Search your gmail drafts by subject</label
>
<button on:click={execute}>get draft</button>
{#if narrowYourSearch}
  <h4>there are too many drafts with that subject line.</h4>
{:else if narrowYourSearch === false}
  <h4>Good to Go!</h4>
{/if}

<style>
  #draftSearch {
    margin: 1rem;
  }
</style>
