<nav>
    {#each pages as p}
    <a href="{p.url}">{p.title}</a>
    {/each}
</nav>
<script>
    let pages = [
	{url: "./", title: "Home"},
	{url: "/projects", title: "Projects"},
    {url: "/cv", title: "CV"},
    {url: "/contact", title: "Contact"},
    {url: "https://github.com/wenhanxue", title: "Github"}
    ];
</script>
<slot />