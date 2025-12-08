<script lang="ts">
    import {
        Masonry,
        Copyable,
        Card,
        Field,
        Piedmont,
        InPlaceCarousel,
    } from "$lib";
    import donazione from "$lib/assets/5x1000.jpg";
    import partners from "./partners.json";
    import direttivo from "./direttivo.json";
    import activities from "./activities.json";
    import carousel from "./carousel.json";

    let privacy_seen = $state(false);

    let show_past = $state(true);
    let search = $state<string>("");
    const search_regexp = $derived(
        search ? new RegExp(`\\b${search}`, "i") : null,
    );

    const today = new Date().toISOString().slice(0, 10);
    console.log("today:", today);
    const filter = $derived.by(() => {
        const N = activities.length;
        const arr = new Array<boolean>(N).fill(true);
        if (!show_past)
            for (let i = 0; i < N; i++) {
                const exp = activities[i].expire;
                if (exp) arr[i] = today < exp;
            }
        if (search_regexp)
            for (let i = 0; i < N; i++)
                arr[i] &&= search_regexp.test(activities[i].title);
        return arr;
    });
</script>

<svelte:head>
    <title>Gruppo Asperger Piemonte</title>
</svelte:head>

<div class="screen-height">
    <header>
        <div class="header-img">
            <Piedmont>
                <InPlaceCarousel images={carousel} />
            </Piedmont>
        </div>
        <h1>Gruppo Asperger Piemonte</h1>
        <div class="header-body">
            <p>
                L’Associazione Gruppo Asperger Piemonte APS, nata
                dall’esperienza condotta sul territorio dall’associazione <a
                    href="https://www.asperger.it/"
                    target="_blank"
                    rel="noreferrer">Gruppo Asperger Onlus</a
                >
                di cui è socia, ha come scopo la
                <b
                    >tutela dei diritti delle persone nello Spettro Autistico e
                    delle loro famiglie</b
                >. Il Gruppo Asperger Piemonte APS, secondo quanto definito nel
                suo
                <a href="/docs/statuto-GAP-firmato.pdf">statuto</a>, si pone
                come obiettivo quello di sostenere la promozione delle pari
                opportunità e delle iniziative di aiuto reciproco e di
                inclusione sociale delle persone nello Spettro Autistico,
                collaborando con altre realtà impegnate sugli stessi temi.
            </p>
            <p>
                Per l’Associazione, il sostegno dell'autodeterminazione e
                dell'autorappresentanza delle persone nello Spettro Autistico
                passa attraverso l’inclusione sociale, scolastica e lavorativa,
                fondamentale per una migliore qualità di vita.
            </p>
            <p>
                Vorremmo un’associazione partecipata e plurale, capace di
                parlare alle diverse anime, siano esse neurodivergenti o
                neurotipiche: ti aspettiamo!
            </p>
        </div>
    </header>
</div>
<main>
    <section id="conosciamoci">
        <h2>Conosciamoci meglio</h2>
        <div class="cols">
            <div>
                <p>
                    L’Associazione si è costituita nel giugno del 2021, su
                    iniziativa di famiglie residenti in Piemonte che, con il
                    supporto di Gruppo Asperger Onlus, fin dal 2006 hanno
                    costruito:
                </p>
                <ul>
                    <li>
                        Azioni finalizzate all&#39;inclusione sociale di persone
                        nello spettro autistico;
                    </li>
                    <li>
                        Attività di formazione ed informazione &mdash; seminari,
                        convegni, giornate divulgative;
                    </li>
                    <li>
                        Percorsi di supporto alle persone nello spettro
                        autistico e alle loro famiglie;
                    </li>
                    <li>
                        Collaborazione con enti pubblici per la tutela dei
                        diritti delle persone con autismo;
                    </li>
                    <li>
                        Collaborazione con centri universitari, per attività di
                        ricerca e formazione, inclusa partecipazione a ricerche
                        ed indagini cliniche e sociologiche.
                    </li>
                </ul>
            </div>
            <div>
                <p>
                    I volontari impegnati nella nostra associazione sono
                    prevalentemente persone con Autismo e loro familiari. Per il
                    corrente mandato, il Consiglio Direttivo è così composto:
                </p>
                <ul>
                    {#each direttivo as p}
                        <li>{p.name}, <span class="colored">{p.role}</span></li>
                    {/each}
                </ul>
            </div>
        </div>
    </section>
    <section id="attivita" class="activities">
        <h2>Attività ed Eventi</h2>
        <details>
            <summary class="filters">
                <svg
                    width="800px"
                    height="800px"
                    viewBox="0 0 24 24"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        d="M20.058 9.72255C21.0065 9.18858 21.4808 8.9216 21.7404 8.49142C22 8.06124 22 7.54232 22 6.50448V5.81466C22 4.48782 22 3.8244 21.5607 3.4122C21.1213 3 20.4142 3 19 3H5C3.58579 3 2.87868 3 2.43934 3.4122C2 3.8244 2 4.48782 2 5.81466V6.50448C2 7.54232 2 8.06124 2.2596 8.49142C2.5192 8.9216 2.99347 9.18858 3.94202 9.72255L6.85504 11.3624C7.49146 11.7206 7.80967 11.8998 8.03751 12.0976C8.51199 12.5095 8.80408 12.9935 8.93644 13.5872C9 13.8722 9 14.2058 9 14.8729L9 17.5424C9 18.452 9 18.9067 9.25192 19.2613C9.50385 19.6158 9.95128 19.7907 10.8462 20.1406C12.7248 20.875 13.6641 21.2422 14.3321 20.8244C15 20.4066 15 19.4519 15 17.5424V14.8729C15 14.2058 15 13.8722 15.0636 13.5872C15.1959 12.9935 15.488 12.5095 15.9625 12.0976"
                        stroke="#1C274C"
                        stroke-width="1.5"
                        stroke-linecap="round"
                    />
                </svg>
                <span>Filtri</span>
            </summary>
            <label>
                <span>Cerca per nome:</span>
                <input type="text" bind:value={search} />
            </label>
            <label>
                <span>Mostra attività passate</span>
                <input type="checkbox" bind:checked={show_past} />
            </label>
        </details>
        <Masonry items={activities} {filter}>
            {#snippet children(a)}
                <Card {...a} />
            {/snippet}
        </Masonry>
    </section>
    <section id="sostienici">
        <h2>Sostenere il Gruppo</h2>
        <div class="sostieni">
            <div>
                <h3>Perché diventare soci?</h3>
                <p>
                    L’attività del Gruppo Asperger Piemonte APS è resa possibile
                    grazie alle quote di iscrizione, alle donazioni e ai
                    contributi derivanti da privati o enti e dal lavoro
                    volontario dei suoi soci.
                </p>
                <p>
                    Entrando nell’associazione potrai partecipare alle nostre
                    attività di supporto alle persone nello spettro autistico e
                    alle loro famiglie.
                </p>
                <p>
                    Gruppo Asperger Piemonte raccoglie i bisogni dei propri soci
                    co-costruendo percorsi di consapevolezza e autonomia,
                    facilitando l’accesso alle informazioni e alle opportunità
                    della rete, in collaborazione con partner istituzionali e
                    del privato sociale.
                </p>
                <h3>Modalità di iscrizione</h3>
                <ul class="spaced">
                    <li>
                        Prendere visione dello
                        <a href="/docs/Statuto-GAP-firmato.pdf" target="_blank"
                            >Statuto</a
                        >;
                    </li>
                    <li>
                        Compilare la richiesta di iscrizione (<a
                            href="/docs/modulo-richiesta-iscrizione-gruppo-asperger-piemonte.pdf"
                            >pdf modificabile</a
                        >) e inviarla all'indirizzo
                        <a href="mailto:info@gruppoaspergerpiemonte.it"
                            >info@gruppoaspergerpiemonte.it</a
                        >;
                    </li>
                    <li>
                        Versare la quota d'iscrizione di 35 euro a mezzo
                        bonifico bancario a Gruppo Asperger Piemonte APS
                        <br />
                        IBAN: <Copyable text="IT48D0623030870000040820775" />
                        <br />causale: QUOTA ASSOCIATIVA
                    </li>
                </ul>
                <h3>5x1000 al Gruppo Asperger Piemonte</h3>
                <p>
                    Un altro modo concreto per sostenere le nostre attività è la
                    tua donazione al 5x1000 al Gruppo Asperger Piemonte: basterà
                    scrivere il nostro codice fiscale <Copyable
                        text="97870260011"
                    />
                    nella tua dichiarazione dei redditi.
                </p>
            </div>
            <img
                src={donazione}
                alt="Locandina 5x1000"
                width="780"
                height="1036"
                loading="lazy"
                class="i5x1000"
            />
        </div>
    </section>
    <section id="contattaci">
        <h2>Contattaci</h2>
        <ul class="contacts">
            <li>
                Telefonicamente al numero
                <a href="tel:3279057141">327 905 7141</a> il martedì dalle 10 alle
                12 o giovedì dalle 14 alle 16;
            </li>
            <li>
                Via mail all'indirizzo
                <a href="mailto:info@gruppoaspergerpiemonte.it"
                    >info@gruppoaspergerpiemonte.it</a
                >;
            </li>
            <li>Attraverso il seguente form:</li>
        </ul>
        <form action="https://formspree.io/f/xeqnejrv" method="POST">
            <Field name="nome" autocomplete="name" required />
            <Field type="email" name="email" autocomplete="email" required />
            <textarea
                name="messaggio"
                cols="30"
                rows="7"
                placeholder="Scrivi qui il tuo messaggio..."
                required
            ></textarea>
            <div class="privacy-field">
                <input type="checkbox" required bind:checked={privacy_seen} />
                <span
                    >Ho preso visione della
                    <a href="docs/privacy-GAP.pdf">informativa privacy</a></span
                >
            </div>
            <button
                type="submit"
                class="btn"
                disabled={!privacy_seen}
                title={privacy_seen
                    ? "Invia il messaggio"
                    : "È necessario prendere visione della privacy"}
            >
                Invia messaggio
            </button>
        </form>
        <span id="form-feedback"></span>
    </section>
    <section>
        <h2>Partner e amici</h2>
        <ul class="partners">
            {#each partners as p}
                <li class:bg={p.background}>
                    <a href={p.href} target="_blank" rel="noreferrer">
                        <img
                            src="/loghi/{p.img}"
                            alt="Logo de '{p.name}'"
                            loading="lazy"
                        />
                    </a>
                </li>
            {/each}
        </ul>
    </section>
</main>

<style>
    .screen-height {
        min-height: 95vh;
    }
    h1 {
        text-align: center;
        margin-top: 3vh;
        margin-bottom: 6vh;
        font-size: clamp(2.3rem, 5vw, 5rem);
    }
    @media (min-width: 50rem) {
        .screen-height {
            display: flex;
            align-items: center;
            justify-content: center;
        }
        header {
            display: grid;
            grid-template-columns: 1fr 1fr;
            grid-template-rows: auto 1fr;
            column-gap: 3ch;
            row-gap: 5vh;
            max-width: 122ch;
        }
        h1 {
            text-align: left;
            margin-bottom: 0;
            grid-column: 1/3;
            grid-row: 1;
            z-index: 3;
        }
        .header-body {
            grid-row: 2;
            grid-column: 1;
        }
        .header-img {
            grid-column: 2;
            grid-row: 1/3;
            z-index: 2;
            align-self: center;
        }
    }

    .cols {
        column-width: 60ch;
        column-gap: 3.5rem;
        column-rule: 2px dashed hsla(0, 0%, 20%, 0.7);
        max-width: 145ch;
    }
    .cols > * {
        break-inside: avoid;
    }
    .colored {
        color: rgb(var(--blu));
    }

    .activities {
        --gap: 1.3rem;
        --col-width: 42ch;
    }

    details {
        margin-bottom: 2rem;
    }
    summary {
        display: flex;
        align-items: center;
        background-color: #eee;
        padding: 0.3rem 0.8rem 0.3rem 0.5rem;
        border-radius: 2.5rem;
        width: fit-content;
        cursor: pointer;
    }
    summary svg {
        display: block;
        height: 1.3rem;
        width: 1.3rem;
        margin-right: 1ch;
    }
    details::details-content {
        border-left: 2px solid rgb(var(--rosso));
        padding-left: 0.8ch;
        margin-left: 1ch;
    }
    details label {
        display: flex;
        width: fit-content;
        align-items: center;
        margin-top: 0.5rem;
    }
    label > input {
        margin-left: 1ch;
        max-width: 20ch;
    }

    .sostieni img {
        display: block;
        width: 100%;
        height: auto;
        border-radius: 8px;
        box-shadow: 0 0 30px hsla(0, 0%, 20%, 0.15);
    }
    @media (min-width: 60rem) {
        .sostieni {
            display: grid;
            grid-template-columns: 1fr 1fr;
            column-gap: 3.5rem;
            max-width: 120ch;
        }
    }

    .contacts {
        padding-left: 0;
        list-style: none;
    }
    .contacts li + li {
        margin-top: 2.4em;
        position: relative;
    }
    .contacts li + li::before {
        content: "oppure";
        color: #aaa;
        position: absolute;
        top: -1.2em;
        left: 2.5ch;
        transform: translateY(-50%);
    }

    form {
        margin-top: 1rem;
    }
    button {
        font-size: 1em;
    }
    @media (max-width: 110ch) {
        form > * + * {
            margin-top: 1.2rem;
        }
        form button {
            margin-left: auto;
            margin-right: auto;
        }
    }
    @media (min-width: 110ch) {
        form {
            display: grid;
            grid-template-columns: 32ch 60ch;
            gap: 1.7em;
        }
        form .field {
            grid-column: 1;
        }
        form textarea {
            grid-row: 1/5;
            grid-column: 2;
        }
        form button {
            grid-row: 5;
            grid-column: 2;
            justify-self: center;
        }
        .privacy-field {
            grid-row: 3;
            grid-column: 1;
            align-self: start;
        }
    }

    .privacy-field {
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 0.4em;
    }
    .privacy-field input[type="checkbox"] {
        flex-shrink: 0;
        margin-right: 2ch;
    }

    .partners {
        padding: 0;
        list-style: none;
        display: flex;
        flex-wrap: wrap;
        max-width: 100%;
    }
    .partners li {
        max-height: 5rem;
        max-width: calc(100% - 2rem);
        margin: 1rem;
    }
    .partners img {
        display: block;
        max-height: 5rem;
        width: auto;
        max-width: 100%;
        filter: drop-shadow(0 0 1.25rem #0003);
        font-size: var(--fs-500);
    }
    .partners .bg {
        /*background-color: rgb(var(--blu));*/
        background-color: var(--bg-dark);
        border-radius: 7px;
    }
    @media (max-width: 60ch) {
        .partners {
            flex-wrap: nowrap;
            flex-direction: column;
            align-items: center;
        }
    }
</style>
