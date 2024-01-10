<script lang="ts">
	import ColumnHeader from "$lib/components/ColumnHeader.svelte";
import RowLabel from "$lib/components/RowLabel.svelte";
	import RowOptions from "$lib/components/RowOptions.svelte";
	import type { ComponentEvents } from "svelte";


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

    function handleRenameColumn(event: ComponentEvents<ColumnHeader>['rename']) {
        const { index, value } = event.detail;
        columns[index] = value;
        columns = columns;
    }

    function handleDeleteColumn(event: ComponentEvents<ColumnHeader>['delete']) {
        const { index } = event.detail;
        columns.splice(index, 1);
        columns = columns;
    }

    function handleRenameRow(event: ComponentEvents<RowLabel>['rename']) {
        const { index, value } = event.detail;
        rows[index].label = value;
        rows = rows;
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
            {#each columns as title, index}
            <div class="cell">
                <ColumnHeader {title} {index} on:rename={handleRenameColumn} on:delete={handleDeleteColumn} />
            </div>
            {/each}
        </div>
        <div class="row">
            {#each rows as {label, selected, options}, index}
            <div class="cell label">
                <RowLabel {label} {index} on:rename={handleRenameRow} />
            </div>
            {#each columns as column, index}
            <div class="cell">
                <RowOptions id="{column}-{label}" selected={selected[index]} {options} on:rename={handleRenameRow}/>
            </div>
            {/each}
            {/each}
        </div>
    </div>
    <button class="add add-column" on:click={handleAddColumn}>+</button>
    <button class="add add-row">+</button>
</main>

<footer>
    <button class="main" on:click={handleRandomize}>Randomize!</button>
    <button class="secondary" on:click={handleClear}>Clear</button>
</footer>

<style>
    h1 {
        font-size: 36px;
        font-weight: 800;
    }

    main {
        display: grid;
        grid-template-columns: 1fr 50rem;
        grid-template-rows: auto 48rem;
        grid-template-areas: 
            "table" "add-column"
            "add-row" "add-row";
        
        max-width: 100%;
        overflow-x: auto;
        padding-top: 20rem;
    }

    button.add {
        font-size: 18px;
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        transition: background-color 100ms cubic-bezier(0.075, 0.82, 0.165, 1);
        border: 1px solid var(--borderColor);
        
    }

    button.add-row {
        border-top: none;
    }

    button.add-column {
        border-left: none;
    }
    
    button.add:hover {
        background-color: var(--bgColor-active);
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
        border-left: 1px solid var(--borderColor);
        border-top: 1px solid var(--borderColor);
    }

    .cell:last-child:not(.empty) {
        border-right: 1px solid var(--borderColor);
    }

    .row:last-child .cell:not(.empty) {
        padding-bottom: 10rem;
        border-bottom: 1px solid var(--borderColor);
    }

    .cell:where(.empty) {
        border: none;
        background-color: var(--color-background);
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
        background-color: var(--bgColor-button-main);
        border-color: var(--bgColor-button-main);
        color: var(--color-button-main);
    }
    button.main:hover {
        background-color: var(--bgColor-button-main-hover);
        border-color: var(--borderColor-button-main-hover);
    }

    button.secondary {
        border-color: var(--borderColor-button-secondary);
        color: var(--color-button-secondary);
    }
    button.secondary:hover {
        border-color: var(--borderColor-button-secondary-hover);
        color: var(--color-button-secondary-hover);
    }

    
</style>

