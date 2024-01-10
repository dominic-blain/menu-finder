<script lang="ts">
    export let id: string;
    export let selected: string;
    export let options: string[];
</script>

<select {id} bind:value={selected}>
    <option></option>
    {#each options as option}
        <option value={option}>{option}</option>
    {/each}
</select>

<style>
    select {
        font-size: 16px;
        border: none;
        padding: 6rem 4rem;
        outline: none;
        border-radius: 3px;
        width: 100%;
    }
</style>