<script lang="ts">
    interface Props {
        title: string;
        subtitle: string | null;
        image: string | null;
        body: string;
    }

    let { title, subtitle, image, body }: Props = $props();

    const link_regexp = /\[([^\[\]]+)\]\(([^\(\)]+)\)/g;
    const bold_regexp = /\*([^\*\n<>]+)\*/g;
    const emph_regexp = /_([^_\n<>]+)_/g;

    const block_open = ["<ol>", "<ul>"];
    const block_close = ["</ol>", "</ul>"];
    function parse_body(raw: string) {
        const lines = raw.split("\n");
        let result = "";
        let state = 0;

        function check(n: number) {
            if (state == n) return;
            if (state) result += block_close[state - 1];
            result += block_open[n - 1];
            state = n;
        }

        for (const line of lines) {
            if (!line.trim()) {
                if (state) {
                    result += block_close[state - 1];
                    state = 0;
                }
                continue;
            }

            switch (line.charCodeAt(0)) {
                case 43: // +
                    check(1);
                    result += `<li>${line.slice(2)}</li>`;
                    break;
                case 45: // -
                    check(2);
                    result += `<li>${line.slice(2)}</li>`;
                    break;
                case 62: // >
                    if (state) result += block_close[state - 1];
                    result += `<p class="small">${line.slice(1)}</p>`;
                    state = 0;
                    break;
                case 64: {
                    // @
                    if (state) result += block_close[state - 1];

                    let cnt = "";
                    let res: RegExpExecArray | null;
                    while ((res = link_regexp.exec(line))) {
                        cnt += `<a href="${res[2]}" target="_black" rel="noreferrer" class="btn">${res[1]}</a>`;
                    }
                    result += `<div class="actions">${cnt}</div>`;
                    state = 0;
                    break;
                }
                default:
                    if (state) result += block_close[state - 1];
                    result += `<p>${line}</p>`;
                    state = 0;
            }
        }
        if (state) result += block_close[state];

        return result
            .replace(
                link_regexp,
                '<a href="$2" target="_black" rel="noreferrer">$1</a>',
            )
            .replace(bold_regexp, "<b>$1</b>")
            .replace(emph_regexp, "<em>$1</em>");
    }

    const body_html = $derived(parse_body(body));

    // function listify(match: string) {
    //     const lines = match.split("\n");
    //     const N = lines.length - 1; // avoid last newline
    //     let result = `<li>${lines[0].slice(2)}</li>`;
    //     for (let i = 1; i < N; i++) result += `<li>${lines[i].slice(2)}</li>`;
    //     return result;
    // }

    // const body_html = $derived(
    //     body
    //         .replace(par_regexp, "<p>$1</p>")
    //         .replace(btns_regexp, '<div class="btns">$1</div>')
    //         .replace(bold_regexp, "<b>$1</b>")
    //         .replace(emph_regexp, "<em>$1</em>")
    //         .replace(
    //             link_regexp,
    //             '<a href="$2" target="_black" rel="noreferrer">$1</a>',
    //         )
    //         .replace(ol_regexp, (match) => `<ol>${listify(match)}</ol>`)
    //         .replace(ul_regexp, (match) => `<ul>${listify(match)}</ul>`),
    // );
</script>

<div class="card">
    {#if image}
        <img src={image} alt={title} />
    {/if}
    <h3>{title}</h3>
    {#if subtitle}
        <span class="subtitle">{subtitle}</span>
    {/if}
    <div class="body">{@html body_html}</div>
</div>

<style>
    .card {
        padding: 1.3em;
        border-radius: 1.25em;
        min-width: min(100%, 45ch);
        width: 100%;
        box-shadow: 0 0 1em #0001;
        background-color: #fafafa77;
    }
    img {
        width: 100%;
        height: auto;
        display: block;
        border-radius: 7px;
    }
    h3 {
        padding-left: 0.7rem;
        position: relative;
        margin-bottom: 0;
    }
    h3:not(:first-child) {
        margin-top: 1rem;
    }
    h3::before {
        content: "";
        position: absolute;
        left: 0;
        top: 0.1em;
        bottom: 0.1em;
        height: calc(100% - 0.2em);
        width: 0.3rem;
        border-radius: 0.2rem;
        background-color: rgb(var(--giallo));
    }
    .subtitle {
        font-size: var(--fs-500);
        position: relative;
        top: -0.1em;
        padding-left: 0.7rem;
        display: block;
        color: var(--fg-sub);
    }
    .subtitle::before {
        content: "";
        position: absolute;
        left: 0;
        bottom: 0.1em;
        top: -0.5em;
        height: calc(100% + 0.4em);
        border-bottom-left-radius: 0.2rem;
        border-bottom-right-radius: 0.2rem;
        width: 0.3rem;
        background-color: rgb(var(--giallo));
    }

    .body > :global(:is(p, ol, ul)) {
        margin-top: 1rem;
        margin-bottom: 0rem;
    }
    .body > :global(.actions) {
        margin-top: 1.5rem;
        display: flex;
        gap: 1.2rem;
    }
    .body :global(li::marker) {
        color: rgb(var(--blu));
    }
</style>
