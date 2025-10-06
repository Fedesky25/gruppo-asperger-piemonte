<script lang="ts">
    import type { FullAutoFill, HTMLInputTypeAttribute } from "svelte/elements";

    interface Props {
        type?: "text" | "email";
        name: string | null;
        required: boolean;
        autocomplete: FullAutoFill | null;
    }

    const {
        type = "text",
        name = null,
        autocomplete = null,
        required = false,
    }: Props = $props();

    const uid = $props.id();

    function oninput(this: HTMLInputElement) {
        this.parentElement?.classList.toggle("filled", !!this.value);
    }
</script>

<div class="field">
    <label for={uid}>Nome</label>
    <input id={uid} {type} {name} {autocomplete} {required} {oninput} />
</div>

<style>
    .field {
        position: relative;
        --h-padding: 0.7em;
        width: 100%;
        max-width: 35ch;
    }
    label {
        position: absolute;
        top: 50%;
        left: calc(0.5 * var(--h-padding));
        transform: translateY(-50%);
        transform-origin: left;
        background-color: var(--bg);
        padding-left: calc(0.5 * var(--h-padding));
        padding-right: calc(0.5 * var(--h-padding));
        cursor: text;
        user-select: none;
        transition:
            top 70ms ease-out,
            color 70ms,
            transform 70ms;
    }
    .field:global(.filled) > label,
    .field:focus-within > label {
        top: -0.1em;
        color: var(--fg-sub);
        transform: translateY(-50%) scale(0.92) translateX(1ch);
    }
</style>
