<script lang="ts">
	import ColumnHeader from "$lib/components/ColumnHeader.svelte";
import RowLabel from "$lib/components/RowLabel.svelte";
	import RowOptions from "$lib/components/RowOptions.svelte";


    interface Row {
        label: string;
        options: string[];
        selected: string[];
    }

    let columns = [
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

    let rows: Row[] = [
        { 
            label: 'Protein',
            options: proteinOptions,
            selected: [] 
        }
    ]

    function handleRandomize() {

        const randomizeRow = (options: string[]) => {
            const choices = [];
            let remainingOptions = [...options];
            for (let i = 0; i < columns.length; i++) {
                const chosenIndex = Math.floor(Math.random() * remainingOptions.length);
                choices.push(remainingOptions[chosenIndex]);
                remainingOptions.splice(chosenIndex, 1);
            }
            return choices;
        }

        rows.forEach(row => row.selected = randomizeRow(row.options));
        rows = rows;
    }

    function handleClear() {
        rows.forEach(row => row.selected = []);
        rows = rows;
    }

    function handleAddColumn() {
        const newColumn = prompt('New column name');
        columns = [...columns, newColumn ?? 'Unknown'];
    }
    
</script>

<header>
    <h1>Menu Finder</h1>
    <p>Find meal suggestions to help create your weekly menu.</p>
</header>

<main>
    <div class="table">
        <div class="row">
            <div class="cell empty"></div>
            {#each columns as title}
                <div class="cell"><ColumnHeader {title} /></div>
            {/each}
        </div>
        <div class="row">
            {#each rows as {label, selected, options}}
            <div class="cell label"><RowLabel {label} /></div>
                {#each columns as column, index}
                <div class="cell"><RowOptions id="{column}-{label}" selected={selected[index]} {options} /></div>
                {/each}
            {/each}
        </div>
    </div>
    <button class="add" on:click={handleAddColumn}>+</button>
    <button class="add">+</button>
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
        display: grid;
        grid-template-columns: 1fr 30rem;
        grid-template-rows: auto 30rem;
        grid-template-areas: 
            "table" "add-column"
            "add-row" "add-row";
        gap: 10rem;
        max-width: 100%;
        overflow-x: auto;
    }

    button.add {
        font-size: 18px;
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 4px;
        position: relative;
        transition: background-color 100ms cubic-bezier(0.075, 0.82, 0.165, 1);
        border: 1px dashed var(--color-border);
    }
    
    button.add:hover {
        background-color: var(--color-background);
    }

    .table {
        display: table;
        width: 100%;
        grid-area: "table";
    }

    .row {
        display: table-row;
    }

    .cell {
        display: table-cell;
        padding: 10rem;
        border-left: 1px solid var(--color-border);
        border-top: 1px solid var(--color-border);
    }

    .cell:last-child:not(.empty) {
        border-right: 1px solid var(--color-border);
    }

    .row:last-child .cell:not(.empty) {
        padding-bottom: 10rem;
        border-bottom: 1px solid var(--color-border);
    }

    .cell:where(.empty) {
        border: none;
    }

    footer {
        margin-top: 30rem;
    }

    button:where(.main, .secondary) {
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

