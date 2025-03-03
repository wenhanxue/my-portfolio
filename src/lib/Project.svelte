<script>
    export let data = {};
    export let hLevel = 2;
</script>

<article>
    <svelte:element this={"h" + hLevel}><a href="assignments/a2/A2_report.html">{data.title}</a></svelte:element>
    <img src={data.image} alt="">
    <p>{data.description}</p>
</article>

<style>
    .projects {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(15em, 1fr));
}

    .projects article {
        grid-template-rows: subgrid;
        grid-row: span 3;
        padding: 0.5em;
    }

    .projects article h3 {
        margin: 0;
    }
</style>