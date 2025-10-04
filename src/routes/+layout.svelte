<script lang="ts">
    import favicon from "$lib/assets/favicon.svg";
    import "$lib/global.css";

    let { children } = $props();
</script>

<svelte:head>
    <link rel="icon" href={favicon} />
</svelte:head>

<div class="head glass">
    <a href="/">Gruppo Aspeger Piemonte</a>
    <nav>
        <ul class="desktop-nav">
            <li><a href="#a">Chi siamo</a></li>
            <li><a href="#a">Attività</a></li>
            <li><a href="#a">Sostienici</a></li>
            <li><a href="#a">Contattaci</a></li>
        </ul>
    </nav>
</div>

{@render children?.()}

<style>
    .head {
        position: sticky;
        top: 2rem;
        display: flex;
        justify-content: space-between;
        width: calc(100% - 4em);
        max-width: 70ch;
        margin: 0 auto;
        padding: 0.5rem 0.7rem;
        border-radius: 7px;
        font-size: var(--fs-400);
    }
    .desktop-nav {
        list-style: none;
        display: flex;
    }
    .desktop-nav li {
        margin-left: 1.2ch;
    }
    a {
        text-decoration: none;
        color: inherit;
    }
</style>
