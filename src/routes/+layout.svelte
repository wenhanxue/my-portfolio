<script>
    import { page } from "$app/stores";

    let pages = [
	{url: "./", title: "Home"},
	{url: "./projects", title: "Projects"},
    {url: "./cv", title: "CV"},
    {url: "./contact", title: "Contact"},
    {url: "https://github.com/wenhanxue", title: "Github"}
    ];
</script>
<nav>
    {#each pages as p}
    <a
        href={p.url} 
        class:current={$page.route.id === p.url}
        target={p.url.startsWith("http") ? "_blank" : null}
    >
        {p.title}
    </a>
      
    {/each}
</nav>
<slot />