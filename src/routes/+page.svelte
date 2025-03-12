<script>
    import projects from "$lib/projects.json";
    import Project from "$lib/Project.svelte";

    let profileData = fetch("https://api.github.com/users/your-username");
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

    {#await fetch("https://api.github.com/users/wenhanxue") }
    <p>Loading...</p>
    {:then response} {#await response.json()}
    <p>Decoding...</p>
    {:then data}
    <section>
        <h2>My GitHub Stats</h2>
        <dl>
          <dt>Followers:</dt>
          <dd>{data.followers}</dd>
          <dt>Following:</dt>
          <dd>{data.following}</dd>
          <dt>Public Repositories:</dt>
          <dd>{data.public_repos}</dd>
        </dl>
    </section>
    <!-- <p>The data is { JSON.stringify(data) }</p> -->
    {:catch error}
    <p class="error">Something went wrong: {error.message}</p>
    {/await} {:catch error}
    <p class="error">Something went wrong: {error.message}</p>
    {/await}

<style>
    dl {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-template-rows: auto auto;
    gap: 10px;
    background-color: var(--background-color);
    color: var(--text-color);
    padding: 1em;
    border-radius: 8px;
    border: 1px solid var(--border-color);
  }

  dt {
    grid-row: 1;
    font-weight: bold;
    color: var(--dt-color);
  }

  dd {
    grid-row: 2;
    margin: 0;
    color: var(--dd-color);
  }

    /* Optional: Add some styling for the section and error messages */
    section {
    margin: 10px;
    padding: 15px;
    border: 2px dashed var(--border-color);
    background-color: var(--background-color);
    color: var(--text-color);
  }

    .error {
    color: red;
    font-weight: bold;
    }
</style>

    <h2>
        Latest Projects
    </h2>
    <div class="projects highlights">
        {#each projects.slice(0, 3) as p}
            <Project data={p} hLevel="3" />
        {/each}
    </div>