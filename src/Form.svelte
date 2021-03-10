<script>
  import materialStore from '../material-store'
  export let id;
  export let name = "";
  export let price;
  $: mode = id ? "edit" : "add";
  $: canSubmit = price >= 0 && name !== "";
  function submit(){
    if(!canSubmit){
      return;
    }
    if (mode == 'add'){
      materialStore.add(name,price);
    }
    if (mode == 'edit'){
      materialStore.edit(id,name,price);
    }
    price = "";
    name = "";
    id = undefined;
  }
  function cancel(){
    price = "";
    name = "";
    id = undefined;
  }
</script>
<style>
  button{
    margin-left: 20px;
  }
  button:disabled{
    cursor:not-allowed;
  }
</style>
<form on:submit|preventDefault={submit}>
  <fieldset>
    <label for="name">Material</label>
    <input bind:value={name} type="text" id="name" placeholder="Wood, Glue, Etc">
    <label for="price">Price</label>
    <input bind:value={price} type="number" min="0" id="price" placeholder="Price" step="any">
  </fieldset>
  <button type="submit" class="float-right" disabled={!canSubmit}>{mode}</button>
  {#if mode === 'edit'}
    <button on:click={cancel} type="button" class="float-right">Cancel</button>
  {/if}
</form>