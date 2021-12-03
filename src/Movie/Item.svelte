<script>
    import { createEventDispatcher } from 'svelte';
    export let id;
    export let name;
    export let status;
    export let image;
    export let like;

    const dispatch = createEventDispatcher();

    function toggleLike() {
        dispatch('onToggleLike', {
            id, name, status, image,
        });
    }

</script>


  <div class="card" >
    <img src={image} class="card-img-top" alt="...">
    <div class="card-body">
      <h5 class="card-title">{name}</h5>
      <p class="card-text">{status}</p>
      <div class="text-center">
            <button class={like ? 'btn btn-secondary' : 'btn btn-primary'}  on:click={toggleLike}>{like ? 'I hate it' : 'I liked it'}</button>
      </div>
    </div>
</div>