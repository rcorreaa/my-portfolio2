<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import Pie from '$lib/Pie.svelte';
    import * as d3 from 'd3';

    let query = "";

    $: filteredProjects = projects.filter(project => {
    if (query) {
        return project.title.includes(query);
    }

    return true;
});

// Make sure the variable definition is *outside* the block
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


//     $: rolledData = d3.rollups(filteredProjects, v => v.length, d => d.year);

//     $: pieData = rolledData.map(([year, count]) => {
//     return { value: count, label: year };
// });
    
</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>

<Pie data={pieData} />

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