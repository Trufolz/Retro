<script>
    import Item from './Item.svelte';

    let items = ['good pairing'];
</script>

<div>
    <ul>
        {#each items as item, idx}
            <li>
                <Item text={item} />
            </li>
        {/each}
    </ul>
</div>

<style>
    div {
        border: 1px solid #ccc;
        border-radius: 2px;
        padding: 0.4em;
        margin: 0 0 0.5em 0;
        box-sizing: border-box
    }
</style>
