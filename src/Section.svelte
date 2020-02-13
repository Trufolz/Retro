<script>
    import { createEventDispatcher } from 'svelte';
    import { slide } from 'svelte/transition';

    export let title;
    export let items = [];

    let dispatch = createEventDispatcher();
    function removeItem(event, id) {
        dispatch('removeItem', {
            id,
            section: title
        });
    }
</script>

<div class="section">
    <p>{title}</p>
        {#each items as item (item.id)}
            <div transition:slide|local>
                <span>
                    {item.value}
                </span>
                <span>
                    <i on:click={(event, id) => removeItem(event, item.id)} class="fa fa-trash-o"></i>
                </span>
            </div>
        {/each}
</div>

<style>
    p {
        font-size: 40px;
        font-family: 'Galada';
        color: #7a1de0;
    }
    div {
        padding: 0.4em;
        margin: 0 0 0.5em 0;
    }
    .section {
        padding: 3em;
        flex-basis: 30em;
        flex-grow: 0;
        flex-shrink: 0;
        overflow: hidden;
    }
    .fa-trash-o {
        margin-left: 5px;
        cursor: pointer;
    }
</style>
