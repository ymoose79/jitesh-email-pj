<script>
  export let draftSubject;
</script>

<label>
  <input
    type="text"
    name="draftSearch"
    id="draftSearch"
    bind:value={draftSubject}
  />Search your gmail drafts by subject</label
>

<style>
  #draftSearch {
    margin: 1rem;
  }
</style>
