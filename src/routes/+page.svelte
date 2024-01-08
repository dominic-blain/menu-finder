<script lang="ts">
    import WeekDay from '$lib/components/WeekDay.svelte';

    const weekDays = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday'
    ]

    const proteinOptions = [
        'Pork',
        'Beef',
        'Chicken',
        'Turkey',
        'Tuna',
        'Salmon',
        'Shrimp',
        'Tofu',
        'Chickpea',
        'Kidney Beans',
    ];

    let selectedProteins: string[] = [];

    function handleRandomize() {
        const choices = [];
        let remainingProteinOptions = [...proteinOptions];
        for (let i = 0; i < weekDays.length; i++) {
            const chosenIndex = Math.floor(Math.random() * remainingProteinOptions.length);
            choices.push(remainingProteinOptions[chosenIndex]);
            remainingProteinOptions.splice(chosenIndex, 1);
        }
        selectedProteins = choices;
    }

    function handleClear() {
        selectedProteins = [];
    }
</script>

<header>
    <h1>Menu Finder</h1>
    <p>Find meal suggestions to help create your weekly menu.</p>
</header>

<main>
    {#each weekDays as day, index}
        <div class="day">
            <WeekDay name={day} {proteinOptions} selectedProtein={selectedProteins[index] ?? null} />
        </div>
    {/each}
</main>

<footer>
    <button class="main" on:click={handleRandomize}>Randomize!</button>
    <button class="secondary" on:click={handleClear}>Clear</button>
</footer>

<style>
    h1 {
        font-size: 36px;
        font-weight: 800;
        color: rgb(50,50,50);
    }

    main {
        display: flex;
        flex-wrap: wrap;
        gap: 10rem;
    }

    .day {
        width: 200rem;
        border: 1px solid var(--color-border);
    }

    footer {
        margin-top: 30rem;
    }

    button {
        border: 1px solid;
        background: transparent;
        border-radius: 3px;
        
        padding: 10rem 20rem;
        font-size: 16rem;
        cursor: pointer;

        transition: 100ms cubic-bezier(0.075, 0.82, 0.165, 1);
        transition-property: color, border-color, background-color;
    }

    button.main {
        background-color: var(--color-text);
        border-color: var(--color-text);
        color: white;
    }
    button.main:hover {
        background-color: rgb(50,50,50);
        border-color: rgb(50,50,50);
    }

    button.secondary {
        border-color: var(--color-text);
    }
    button.secondary:hover {
        border-color: black;
        color: black;
    }

    
</style>

