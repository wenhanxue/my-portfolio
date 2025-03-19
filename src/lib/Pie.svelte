<script>
    import * as d3 from 'd3';

    let arc = d3.arc().innerRadius(0).outerRadius(50)({
	startAngle: 0,
	endAngle: 2 * Math.PI
    });

    export let data = [];

    let colors = d3.scaleOrdinal(d3.schemeTableau10);
    let sliceGenerator = d3.pie().value(d => d.value);
    // Define arcData and arcs outside the reactive block
    let arcData;
    let arcs;

        $: {
            // Reactively calculate arcData and arcs the same way we did before with sliceGenerator and arcGenerator
            arcData = sliceGenerator(data);
            arcs = arcData.map(d => d3.arc().innerRadius(0).outerRadius(50)(d));
        }

    export let selectedIndex = -1;


</script>

<div class="container">
    <svg viewBox="-50 -50 100 100">
        {#each arcs as arc, index}
            <path d={arc} fill={colors(index)}
                class:selected={selectedIndex === index}
                on:click={e => selectedIndex = selectedIndex === index ? -1 : index} />
        {/each}
    </svg>
    
    <ul class="legend">
        {#each data as d, index}
            <li style="--color: { colors(index) }" class:selected={selectedIndex === index}>
                <span class="swatch"></span>
                {d.label} <em>({d.value})</em>
            </li>
        {/each}
    </ul>
</div>

<style>
    svg {
        max-width: 20em;
        margin-block: 2em;

        /* Do not clip shapes outside the viewBox */
        overflow: visible;
    }

    svg:has(path:hover) path:not(:hover) {
	    opacity: 50%;
    }

    path {
        transition: 300ms;
        cursor: pointer;
    }

    /* When a path is selected, make all non-selected paths 50% opacity */
    svg:has(.selected) path:not(.selected) {
        opacity: 50%;
    }

    .selected {
        --color: oklch(60% 45% 0) !important;
        
        &:is(path) {
            fill: var(--color) !important;
        }
        
        &:is(li) {
            color: var(--color);
        }
    }

    ul:has(.selected) li:not(.selected) {
        color: gray;
    }

    path:hover {
        opacity: 100% !important;
    }



    /* Styling the legend container */
    .legend {
        flex: 1;
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(8em, 1fr)); /* Responsive grid layout */
        gap: 1rem; /* Spacing between grid items */
        border: 2px solid black; /* Border to distinguish the legend */
        padding: 1rem; /* Spacing inside the legend container */
        margin: 0; /* Spacing outside the legend container */
        list-style: none; /* Remove default list bullets */
        background-color: rgb(56, 55, 55);
    }

    /* Styling the list items */
    .legend li {
        display: flex;
        align-items: center; /* Vertically center the swatch and text */
        gap: 0.5rem; /* Spacing between swatch and text */
    }

    /* Styling the swatch */
    .legend .swatch {
        display: inline-block; /* Allow width and height to work */
        width: 1rem; /* Square size */
        height: 1rem; /* Square size */
        background-color: var(--color); /* Use the dynamic color variable */
        border-radius: 4px; /* Slightly rounded corners */
    }

    .container {
        display: flex;
        align-items: center;
        gap: 2rem;
        max-width: 100%;
        margin: 0 auto;
        padding: 1rem;
    }

</style>