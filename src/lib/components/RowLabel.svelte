<script lang="ts">
	import { createEventDispatcher } from "svelte";

    export let label: string;
    export let index: number;

    const dispatch = createEventDispatcher();

    function handleInput(event: Event) {
        const value = (event.target as HTMLElement)?.textContent ?? '';
        dispatch('rename', { value, index });
    }
</script>

<div contenteditable="plaintext-only" bind:textContent={label} on:input={handleInput}>{label}</div>

<style>
    div {
        font-size: 11rem;
        text-transform: uppercase;
        letter-spacing: 0.1em;
        margin-bottom: 4rem;
        display: block;
    }
</style>