<script>
    import * as d3 from 'd3';
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
    import Pie from "$lib/Pie.svelte";

    let query = "";

    $: filteredProjects = projects.filter(project => {
        let values = Object.values(project).join("\n").toLowerCase();
	    return values.includes(query.toLowerCase());
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

    let selectedYearIndex = -1;
    let selectedYear;
    $: selectedYear = selectedYearIndex > -1 ? pieData[selectedYearIndex].label : null;
    $: filteredByYear = filteredProjects.filter(project => {
        if (selectedYear) {
            return project.year === selectedYear;
        }

        return true;
    });


</script>

<svelte:head>
  <title>Projects</title>
</svelte:head>
<!-- <pre>{ JSON.stringify(projects, null, "\t") }</pre> -->

<h1>{ projects.length } Projects</h1>
    <!-- <nav>
        <ul>
            <li><a href="../index.html">Home</a></li>
            <li><a href="../contact/index.html">Contact</a></li>
            <li><a  href="index.html">Projects</a></li>
            <li><a href="../cv/index.html">CV</a></li>
            <li><a href="https://github.com/wenhanxue" target="_blank">GitHub</a></li>
        </ul>
    </nav> -->

<Pie data={pieData} bind:selectedIndex={selectedYearIndex} />

<input type="search" bind:value={query}
aria-label="Search projects" placeholder="🔍 Search projects…" />

<div class="projects">
    {#each filteredByYear as p}
        <Project data={p} />

        <!-- <article>
            <h2><a href="assignments/a2/A2_report.html">{p.title}</a></h2>
            <img src={p.image} alt="">
            <p>{p.description}</p>
        </article> -->
    {/each}
</div>

<style>
    input {
        width: 100%;
    }
</style>