<script>
    import { page } from "$app/stores";

    let pages = [
	{url: "./", title: "Home"},
	{url: "./projects", title: "Projects"},
    {url: "./cv", title: "CV"},
    {url: "./contact", title: "Contact"},
    {url: "https://github.com/wenhanxue", title: "Github"}
    ];


    let root = globalThis?.document?.documentElement;
    $: root?.style.setProperty("color-scheme", colorScheme);

    let localStorage = globalThis.localStorage ?? {};
    $: localStorage.colorScheme = colorScheme;

    let colorScheme = localStorage.colorScheme ?? "light dark";

</script>

<nav>
    {#each pages as p}
    <a
        href={p.url} 
        class:current={"." + $page.route.id === p.url}
        target={p.url.startsWith("http") ? "_blank" : null}
    >
        {p.title}
    </a>
      
    {/each}
</nav>

{localStorage.colorScheme}

<label class="color-scheme">
    Theme:
    <select bind:value={ colorScheme }>
        <option value="light dark">Automatic</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
    </select>
</label>

<slot />

<style>
    nav ul {
    display: contents;
    }

    nav ul li {
        display: contents;
    }

    nav a.current {
        border-bottom-width: 0.4em;
        border-bottom-style: solid;
        border-bottom-color: oklch(80% 3% 200);
        padding-bottom: -0.5em;
    }

    nav {
        display: flex;
        margin-bottom: 5ch;
        border-bottom-width: 1px;
        border-bottom-style: solid;
        border-bottom-color: oklch(80% 3% 200);
        text-align: center;

        --border-color: oklch(50% 10% 200 / 40%);
        border-bottom-color: var(--border-color);
    }

    nav a {
        flex: 1;
        text-decoration: none;
        color: inherit;
        padding: 0.5em;
    }

    /* :root {
        --color-accent: oklch(65% 50% 0);
    } */

    nav a:hover {
        border-bottom: 0.4em solid var(--color-accent);
        background-color: oklch(from var(--color-accent) 95% 5% h);
        background-color: color-mix(in oklch, var(--color-accent), canvas 85%);

    }

    .color-scheme {
        position: absolute;
        top: 1rem;
        right: 1rem;
        display: inline-flex;
        gap: 4px;
    }

    select {
        margin-bottom: 0.7em;
        margin-top: 0.25em;
    }
</style>