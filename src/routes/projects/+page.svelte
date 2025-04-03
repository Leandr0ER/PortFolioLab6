<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import Pie from '$lib/Pie.svelte';
    import * as d3 from 'd3';
    
    // let rolledData = d3.rollups(projects, v => v.length, d => d.year);

    // let pieData = rolledData.map(([year, count]) => {
    // return { value: count, label: year };
    // });

    // Step 4.4
    let pieData;

    $: {
        // Initialize to an empty object every time this runs
        pieData = {};
        
        // Calculate rolledData and pieData based on filteredProjects here
        let rolledData = d3.rollups(filteredProjects, v => v.length, d => d.year);

        // We don't need 'let' anymore since we already defined pieData
        pieData = rolledData.map(([year, count]) => {
            return { value: count, label: year };
        });
    }

    let query = "";  // Step 4.1

    // Step 4.3
    // $: filteredProjects = projects.filter(project => {
    // let values = Object.values(project).join("\n").toLowerCase();
    // return values.includes(query.toLowerCase());
    // });

    // Step 4.3
    // $: filteredProjects = projects.filter(project => {
    // if (query) {
    //     return project.title.toLowerCase().includes(query.toLowerCase());
    // }

    // return true;
    // });

    $: filteredProjects = projects.filter(project => {
    let values = Object.values(project).join("\n").toLowerCase();
    return values.includes(query.toLowerCase());
    });

    // Step 4.2
    // $: filteredProjects = projects.filter(project => {
    // if (query) {
    //     return project.title.includes(query);
    // }

    // return true;
    // });


//     let data = [
//     { value: 1, label: "apples" },
//     { value: 2, label: "oranges" },
//     { value: 3, label: "mangos" },
//     { value: 4, label: "pears" },
//     { value: 5, label: "limes" },
//     { value: 5, label: "cherries" }
// ];



</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>

<Pie data={pieData}/>   <!--Step 3.2--> <!--Step 3.1--> 

<!-- Step 4.1 -->
<input type="search" bind:value={query} aria-label="Search projects" placeholder="🔍 Search projects..." />

<h1>{ projects.length } Projects</h1>
<div class="projects">
    {#each filteredProjects as p}
        <Project data={p}/>
      <!-- <article>
        <h2>{p.title}</h2>
        <img src={p.image} alt="" />
        <p>
            {p.description}
        </p>
      </article> -->
    {/each}
</div>

<div class="container">
  <svg viewBox="-50 -50 100 100">
      <!-- ... -->
  </svg>
  <ul class="legend">
      <!-- ... -->
  </ul>
</div>

