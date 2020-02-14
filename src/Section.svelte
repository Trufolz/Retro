<script>
    import { createEventDispatcher } from 'svelte';
    import { slide } from 'svelte/transition';

    export let section;
    export let items = [];

    let dispatch = createEventDispatcher();
    function removeItem(event, id) {
        dispatch('removeItem', {
            id
        });
    }
    function removeSection(event, id) {
        dispatch('removeSection', {
            id
        });
    }
</script>

<div class="section">
    <p>
        {section.name} 
        <i on:click={(event, id) => removeSection(event, section.id)} class="fa fa-trash-o" title="Remove section"></i>
    </p>
    {#if items.length === 0 }
        <div class="empty-section">I'm empty!</div>
    {/if}
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
        display: flex;
        justify-content: center;
        align-items: center;
    }

    p .fa-trash-o {
        font-size: 25px;
        margin-left: 10px;
        margin-bottom: 10px;
    }
    div {
        padding: 0.4em;
        margin: 0 0 0.5em 0;
    }
    .section {
        width: 30em;
    }
    .empty-section {
        color: #e1e1e1;
        font-style: italic;
    }
    .fa-trash-o {
        margin-left: 5px;
        cursor: pointer;
    }
</style>
