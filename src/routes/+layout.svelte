<script lang="ts">
    import favicon from "$lib/assets/favicon.svg";
    import "$lib/global.css";

    import BackLogo from "$lib/components/BackLogo.svelte";
    import { onDestroy } from "svelte";

    let { children } = $props();

    let AF_request: number | null = null;
    function onscroll() {
        if (AF_request) return;
        AF_request = requestAnimationFrame(parallax);
    }

    let py = $state(0);
    function parallax() {
        const y = window.scrollY;
        py = -y / window.innerHeight;
        AF_request = null;
    }

    onDestroy(() => {
        AF_request && cancelAnimationFrame(AF_request);
    });
</script>

<svelte:head>
    <link rel="icon" href={favicon} />
</svelte:head>

<svelte:window {onscroll} />

<div class="body">
    <div class="behind" style:--y="{10 * py}vh">
        <BackLogo />
    </div>
    <div class="head glass">
        <a href="/" aria-label="Homepage">
            <svg
                class="logo"
                xmlns="http://www.w3.org/2000/svg"
                width="365"
                height="455"
                viewBox="0 0 365 455"
            >
                <g transform="scale(1,-1) translate(0 -410)">
                    <g fill="none" stroke="#e8a92e" stroke-width="17.5">
                        <path d="M181.35,11.07 L310.02,86.05" />
                        <path d="M181.35,11.07 L78.08,35.19" />
                        <path d="M181.35,11.07 L154.63,97.57" />
                        <path d="M310.02,86.05 L154.63,97.57" />
                        <path d="M310.02,86.05 L245.49,140.61" />
                        <path d="M56.25,146.14 L181.35,186.86" />
                        <path d="M56.25,146.14 L78.08,35.19" />
                        <path d="M56.25,146.14 L154.63,97.57" />
                        <path d="M56.25,146.14 L106.39,220.39" />
                        <path d="M181.35,186.86 L154.63,97.57" />
                        <path d="M181.35,186.86 L245.49,140.61" />
                        <path d="M181.35,186.86 L276.28,210.25" />
                        <path d="M181.35,186.86 L190.61,270.14" />
                        <path d="M181.35,186.86 L106.39,220.39" />
                        <path d="M236.07,355.85 L276.28,210.25" />
                        <path d="M236.07,355.85 L190.61,270.14" />
                        <path
                            d="M78.08,35.19L154.63,97.57L245.49,140.61L276.28,210.25L190.61,270.14L106.39,220.39"
                        />
                    </g>
                    <g fill="#851341">
                        <ellipse
                            cx="078.08"
                            cy="035.19"
                            rx="24.13"
                            ry="24.13"
                        />
                        <ellipse
                            cx="154.63"
                            cy="097.57"
                            rx="24.13"
                            ry="24.13"
                        />
                        <ellipse
                            cx="245.49"
                            cy="140.61"
                            rx="24.13"
                            ry="24.13"
                        />
                        <ellipse
                            cx="276.28"
                            cy="210.25"
                            rx="24.13"
                            ry="24.13"
                        />
                        <ellipse
                            cx="106.39"
                            cy="220.39"
                            rx="24.13"
                            ry="24.13"
                        />
                        <ellipse
                            cx="190.61"
                            cy="270.14"
                            rx="24.13"
                            ry="24.13"
                        />
                    </g>
                    <g fill="#3a8e8f">
                        <ellipse cx="181.35" cy="011.07" rx="40.2" ry="40.2" />
                        <ellipse cx="310.02" cy="086.05" rx="40.2" ry="40.2" />
                        <ellipse cx="056.25" cy="146.14" rx="40.2" ry="40.2" />
                        <ellipse cx="181.35" cy="186.86" rx="40.2" ry="40.2" />
                        <ellipse cx="236.07" cy="355.85" rx="40.2" ry="40.2" />
                    </g>
                </g>
            </svg>
        </a>
        <nav>
            <ul class="desktop-nav">
                <li><a href="/#conosciamoci">Chi siamo</a></li>
                <li><a href="/#attivita">Attività</a></li>
                <li><a href="/#sostienici">Sostienici</a></li>
                <li><a href="/#contattaci">Contattaci</a></li>
            </ul>
        </nav>
    </div>
    <div class="content">
        {@render children?.()}
        <svg
            data-nav-closer
            role="banner"
            class="divisor"
            viewBox="0 210 900 250"
            width="900"
            height="250"
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            version="1.1"
        >
            <!-- <rect x="0" y="0" width="900" height="600" fill="var(--bg)"></rect> -->
            <path
                d="M0 243L25 257C50 271 100 299 150 309.3C200 319.7 250 312.3 300 302.8C350 293.3 400 281.7 450 299.7C500 317.7 550 365.3 600 378.7C650 392 700 371 750 374.7C800 378.3 850 406.7 875 420.8L900 435L900 601L875 601C850 601 800 601 750 601C700 601 650 601 600 601C550 601 500 601 450 601C400 601 350 601 300 601C250 601 200 601 150 601C100 601 50 601 25 601L0 601Z"
                fill="var(--bg-dark)"
                stroke-linecap="round"
                stroke-linejoin="miter"
            ></path>
        </svg>
        <footer data-nav-closer>
            <a href="/docs/privacy-GAP.pdf" target="_blank"
                >Informativa sulla privacy - aggiornamento 1/9/2025</a
            >
            <br />
            <br />
            <span>Questo sito non fa uso di cookies</span>
        </footer>
    </div>
</div>

<style>
    .body {
        position: relative;
    }
    .behind {
        position: fixed;
        z-index: 1;
        bottom: -25vh;
        right: 0vw;
        width: 70vw;
        opacity: 0.25;
        filter: blur(20px) grayscale(30%);
        transform: translateY(var(--y));
        will-change: transform;
    }

    .head {
        z-index: 3;
        position: sticky;
        top: 2rem;
        display: flex;
        justify-content: space-between;
        align-items: center;
        width: calc(100% - 4em);
        max-width: 70ch;
        margin: 0 auto;
        padding: 0.5rem 0.7rem;
        border-radius: 7px;
        font-size: var(--fs-400);
    }
    .logo {
        display: block;
        height: 2.2em;
        width: auto;
    }
    .desktop-nav {
        list-style: none;
        display: flex;
    }
    .desktop-nav li {
        margin-right: 1.4ch;
    }
    nav a {
        font-size: 1.1em;
        font-weight: 700;
        text-decoration: none;
        color: #444;
        /*font-variant: small-caps;*/
    }
    nav a:hover {
        color: rgb(var(--rosso));
    }
    nav ul {
        margin-bottom: 0;
    }

    .content {
        position: relative;
        z-index: 2;
    }

    .divisor {
        display: block;
        width: 100%;
        height: auto;
        margin-bottom: -1px;
        margin-top: -1px;
    }

    footer {
        background-color: var(--bg-dark);
        padding: var(--vpp);
        color: var(--fg-ligth);
        font-size: var(--fs-400);
    }
    footer a {
        color: inherit;
        text-decoration-color: #d4d4d4aa;
    }

    @media (min-width: 80ch) {
        footer {
            padding-top: 0;
            text-align: center;
        }
    }
</style>
