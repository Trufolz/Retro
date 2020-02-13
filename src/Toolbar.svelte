<script>
    import { createEventDispatcher } from 'svelte';

    export let sections;

    let inputValue;
    let disableButton = !inputValue;

    const dispatch = createEventDispatcher();
    function handleAddItem(event) {
        dispatch('addItem', {
            section: event.target.innerText,
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
</script>

<div class="toolbar">
    <div>
        <button on:click={handleClearItems} class="pill-button pill-button--retro-opposite">
            <i class="fa fa-trash-o"></i>
        </button>
        <button on:click={handleCopyItems} class="pill-button pill-button--retro-opposite">
            <i class="fa fa-copy"></i>
        </button>
    </div>
    <input bind:value={inputValue} placeholder="Any ideas?"/>
    <div>
        {#each sections as section}
            <button on:click={handleAddItem} disabled={!inputValue} class="pill-button pill-button--retro">
                {section}
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
        box-shadow: 0 0 10px #d25577;
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
            width: 100px;
            min-width: 100px;
            height: 50px;
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
            width: 50px;
            min-width: 50px;
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