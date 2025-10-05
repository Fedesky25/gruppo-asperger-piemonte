<script>
    import donazione from "$lib/assets/5x1000.jpg";
    import { Masonry, Copyable, Card } from "$lib";
    import partners from "./partners.json";
    import direttivo from "./direttivo.json";
    import activities from "./activities.json";
</script>

<svelte:head>
    <title>Gruppo Asperger Piemonte</title>
</svelte:head>

<header>
    <h1>Gruppo Asperger Piemonte</h1>
    <p>
        Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read
        the documentation
    </p>
</header>
<main>
    <section id="conosciamoci" aria-labelledby="conosciamoci-title">
        <h2 id="conosciamoci-title">Conosciamoci meglio</h2>
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
    <section class="activities">
        <h2>Attività ed Eventi</h2>
        <Masonry items={activities}>
            {#snippet children(a)}
                <Card {...a} />
            {/snippet}
        </Masonry>
    </section>
    <section aria-labelledby="sostieni">
        <h2 id="sostieni">Sostenere il Gruppo</h2>
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
    <section aria-labelledby="partner-title">
        <h2 id="partner-title">Partner e amici</h2>
        <ul class="partners">
            {#each partners as p}
                <li class:bg={p.background}>
                    <a href={p.href} target="_blank" rel="noreferrer">
                        <img src="/loghi/{p.img}" alt={p.name} loading="lazy" />
                    </a>
                </li>
            {/each}
        </ul>
    </section>
</main>

<style>
    header {
        height: 90vh;
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
