<script>
    export let data = {};
    export let hLevel = 2;
</script>

<article>
    <svelte:element this={"h" + hLevel}><a href={data.url}>{data.title}</a></svelte:element>
    <img src={data.image} alt="">
    <p>{data.description}</p>
    <i>{data.year}</i>
</article>

<style>
    article {
        grid-template-rows: subgrid;
        grid-row: span 3;
        padding: 0.5em;
    }

    article h3 {
        margin: 0;
    }
</style>