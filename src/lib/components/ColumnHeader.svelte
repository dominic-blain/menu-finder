<script lang="ts">
	import { createEventDispatcher } from "svelte";

    export let title: string;
    export let index: number;

    const dispatch = createEventDispatcher();

    function handleInput(event: Event) {
        const value = (event.target as HTMLElement)?.textContent ?? '';
        dispatch('rename', { value, index });
    }

    function handleDelete() {
        dispatch('delete', { index });
    }
</script>

<div class="ctn">
    <button class="delete" on:click={handleDelete}>×</button>
    <div contenteditable="plaintext-only" bind:textContent={title} on:input={handleInput}>{title}</div>
</div>

<style>
    .ctn {
        position: relative;
        display: flex;
        justify-content: center;
    }

    div[contenteditable] {
        font-size: 22px;
        margin: 0;
        font-family: 'Lemon', 'Georgia', serif;
    }

    button.delete {
        position: absolute;
        top: calc(-10rem - 1.3em/2);
        display: block;
        font-size: 20rem;
        width: 1.25em;
        height: 1.25em;
        border-radius: 100%;
        color: var(--color-button-danger);
        border: 1px solid currentColor;
        background-color: white;
        opacity: 0;
        transform: translateY(5rem);
        transition: 90ms cubic-bezier(0.075, 0.82, 0.165, 1);
        transition-property: opacity, transform, background-color;
    }

    .ctn:hover button.delete {
        opacity: 1;
        transform: none;
    }
</style>