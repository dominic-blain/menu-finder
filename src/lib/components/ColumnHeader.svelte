<script lang="ts">
	import { createEventDispatcher } from "svelte";

    export let title: string;
    export let index: number;

    const dispatch = createEventDispatcher();

    function handleInput(event: Event) {
        const value = (event.target as HTMLElement)?.textContent ?? '';
        dispatch('rename', { value, index });
    }
</script>

<div contenteditable="true" bind:textContent={title} on:input={handleInput}>{title}</div>

<style>
    div {
        font-size: 22px;
        margin: 0;
        font-family: 'Lemon', 'Georgia', serif;
    }
</style>