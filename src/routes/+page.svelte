<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";
</script>

<svelte:head>
    <title>Home</title>
</svelte:head>
  
    <h1>Wenhan Xue</h1>
    <!-- <nav>
        <ul>
            <li><a href="index.html">Home</a></li>
            <li><a href="contact/index.html">Contact</a></li>
            <li><a href="projects/index.html">Projects</a></li>
            <li><a href="cv/index.html">CV</a></li>
            <li><a href="https://github.com/wenhanxue" target="_blank">GitHub</a></li>
        </ul>
    </nav> -->
    <p>hi</p>
    <img src="image/cat.jpg" alt="picture of my cat in the kitchen staring up into the camera">
    <h2>
        Latest Projects
    </h2>
    <div class="projects highlights">
        {#each projects.slice(0, 3) as p}
            <Project data={p} hLevel="3" />
        {/each}
    </div>