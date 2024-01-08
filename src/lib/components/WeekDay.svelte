<script lang="ts">
    export let name: string;
    export let proteinOptions: string[];
    export let selectedProtein: string | null = null;
</script>

<section>
    <h2>{name}</h2>

    <label for={`${name}-protein`}>Protein</label>
    <select id="{`${name}-protein`}" bind:value={selectedProtein}>
        <option></option>
        {#each proteinOptions as protein}
            <option value={protein}>{protein}</option>
        {/each}
    </select>
</section>

<style>
    section {
        padding: 20rem;
    }

    h2 {
        font-size: 22px;
        margin: 0;
        margin-bottom: 20rem;
        border-bottom: 1px solid var(--color-border);
        padding-bottom: 5rem;
        font-family: 'Lemon', 'Georgia', serif;
    }

    label {
        font-size: 11rem;
        text-transform: uppercase;
        letter-spacing: 0.1em;
        margin-bottom: 4rem;
        display: block;
    }

    select {
        font-size: 16px;
        background-color: whitesmoke;
        border: none;
        padding: 6rem 4rem;
        outline: none;
        width: 100%;
        border-radius: 3px;
    }
</style>
