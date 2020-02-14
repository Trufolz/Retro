<script>
    import { createEventDispatcher } from 'svelte';
    import { slide } from 'svelte/transition';

    export let sections;

    let inputValue;
    let disableButton = !inputValue;

    const dispatch = createEventDispatcher();
    function handleAddItem(event, sectionId) {
        dispatch('addItem', {
            sectionId,
            id: undefined,
            value: inputValue
        });
        inputValue = '';
    }

    function handleClearItems(event) {
        dispatch('clearItems');
    }

    function handleCopyItems(event) {
        dispatch('copyItems');
    }

    function handleAddSection(event) {
        dispatch('addSection');
    }
</script>

<div class="toolbar">
    <div>
        <button on:click={handleClearItems} class="pill-button pill-button--retro-opposite" title="Clear all">
            <i class="fa fa-trash-o"></i>
        </button>
        <button on:click={handleCopyItems} class="pill-button pill-button--retro-opposite" title="Copy results">
            <i class="fa fa-copy"></i>
        </button>
        <button on:click={handleAddSection} class="pill-button pill-button--retro-opposite" title="Add a new section">
            <i class="fa fa-plus"></i>
        </button>
    </div>
    {#if sections.length !== 0}
        <input transition:slide|local bind:value={inputValue} placeholder="Any ideas?"/>
    {/if}
    <div>
        {#each sections as section (section.id)}
            <button transition:slide|local on:click={(event, id) => handleAddItem(event, section.id)} disabled={!inputValue} class="pill-button pill-button--retro">
                {section.name}
            </button>
        {/each}
    </div>
</div>

<style lang="scss">
    input {
        margin-top: 30px;
        margin-bottom: 30px;
        width: 500px;
        height: 40px;
        font-size: 23px;
        font-family: 'Segoe UI';
        color: #d25577;
        outline: none;
        padding: 5px 0px 5px 5px;
        box-shadow: 0 0 1px #d25577;
        border: 1px solid #d25577;
    }

    @keyframes glow {
        from {
            -webkit-transition: all 3.0s ease;
            -moz-transition: all 3.0s ease;
            -ms-transition: all 3.0s ease;
            -o-transition: all 3.0s ease;
            box-shadow: 0 0 10px #7a1de0;
            border: 1px solid #7a1de0;
            color: #7a1de0;
        }

        to {
            box-shadow: 0 0 10px #d25577;
            border: 1px solid #d25577;
            color: #d25577;
        }
    }

    input:focus {
        box-shadow: 0 0 10px #7a1de0;
        border: 1px solid #7a1de0;
        animation-duration: 2s;
        animation-name: glow;
        animation-iteration-count: infinite;
    }

    ::-webkit-input-placeholder {
        color: #e1e1e1;
        font-style: italic;
    }
    :-moz-placeholder {
        color: #e1e1e1;
        font-style: italic;  
    }
    ::-moz-placeholder {
        color: #e1e1e1;
        font-style: italic;  
    }
    :-ms-input-placeholder {  
        color: #e1e1e1;
        font-style: italic; 
    }

    .toolbar {
        text-align: center;
    }

    .fa {
        cursor: pointer;
    }

    .pill-button {
        font-family: 'Galada';

        &:after {
            opacity: 0;
            transition: all 0.8s
        }

        &--retro {
            min-width: 100px;
            max-height: 50px;
            min-height: 50px;
            color: #fff;
            background-color: #7a1de0;
            border-color: #7a1de0;
            font-size: 25px;

            &:hover {
                background-color: #d25577;
                border-color: #d25577;
            }
        }

        &--retro-opposite {
            min-width: 50px;
            max-width: 50px;
            min-height: 50px;
            max-height: 50px;
            font-size: 25px;
            color: #fff;
            background-color: #d25577;
            border-color: #d25577;

            &:hover {
                background-color: #7a1de0;
                border-color: #7a1de0;
            }
        }
    }
</style>
