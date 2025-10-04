<script lang="ts">
    let { text }: { text: string } = $props();

    async function onclick() {
        await navigator.clipboard.writeText(text);
    }
</script>

<button type="button" {onclick} title="Copia negli appunti">{text}</button>

<style>
    button {
        font-size: 1em;
        font-weight: 700;
        font-family: inherit;
        background: transparent;
        color: inherit;
        border: none;
        display: inline;
        cursor: pointer;
        width: fit-content;
    }
</style>
