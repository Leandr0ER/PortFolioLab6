<!-- Hello from Pie.svelte -->

<script>

export let data = []; // Step 3.1

import * as d3 from 'd3';

let arcGenerator = d3.arc().innerRadius(0).outerRadius(50);

// let arc = arcGenerator({
//     startAngle: 0,
//     endAngle: 2 * Math.PI
// });

// let data = [1, 2];
// let data = [1, 2, 3, 4, 5, 5];
// let colors = ['gold', 'purple'];

// Step 2.1
// let data = [
//     { value: 1, label: "apples" },
//     { value: 2, label: "oranges" },
//     { value: 3, label: "mangos" },
//     { value: 4, label: "pears" },
//     { value: 5, label: "limes" },
//     { value: 5, label: "cherries" }
// ];

// Usando D3
let colors = d3.scaleOrdinal(d3.schemeTableau10);

// let sliceGenerator = d3.pie();
let sliceGenerator = d3.pie().value(d => d.value); // Step2.1

// let arcData = sliceGenerator(data);
// let arcs = arcData.map(d => arcGenerator(d));

// Step 4.4
let arcData;
let arcs;

$: {
        // Reactively calculate arcData and arcs the same way we did before with sliceGenerator and arcGenerator
        arcData = sliceGenerator(data); // TODO: FILL IN
        arcs = arcData.map(d => arcGenerator(d)); // TODO: FILL IN
    }


</script>

<div class="container">  <!--Step 2.4 -->

<svg viewBox="-50 -50 100 100">
    <!-- <path d={arc} fill="red" /> -->
    <!-- {#each arcs as arc}
        <path d={arc} fill="red" />
    {/each} -->    
    {#each arcs as arc, index}
        <path d={arc} fill={colors(index)} />
    {/each}
</svg>

<!-- Step 2.2 -->
<ul class="legend">
    {#each data as d, index}
        <li style="--color: { colors(index) }">
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
}

.container{
    display: flex;
    align-items: center;
    gap: 0.6em;
}

.legend{
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(8em, 1fr));
    border: 1px solid black;
    padding: 1em;
    margin: 1em;
    flex: 1;
}

.legend li{
    display:flex;
    align-items: center;
    gap: 0.6em;
}

.swatch{
    display: inline-block;
    background-color: var(--color);
    width: 1em;
    height: 1em;
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

</style>