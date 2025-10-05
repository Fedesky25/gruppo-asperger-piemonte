<script lang="ts" generics="T extends {}, K extends keyof T">
    import { onDestroy, type Snippet } from "svelte";

    interface Props {
        key?: K | null;
        items: T[];
        children: Snippet<[T, number]>;
    }
    let { key = null, items, children }: Props = $props();

    let wrapper: HTMLDivElement;
    let refresh_request: number | null = null;

    function refresh_layout() {
        console.log("Masonry layout refreshed");

        const ncols =
            getComputedStyle(wrapper).gridTemplateColumns.split(" ").length;

        if (ncols == 1) {
            wrapper.style.gridAutoRows = "auto";
            wrapper.style.paddingBottom = "0";
            for (const child of wrapper.children) {
                (child as HTMLDivElement).style.marginTop = "0px";
            }
        } else {
            const offsets = new Array<number>(ncols).fill(0);
            let idx = 0;
            for (const _child of wrapper.children) {
                const child = _child as HTMLDivElement;
                const height = child.getBoundingClientRect().height;
                const col = idx % ncols;
                // child.style.gridColumn = (1 + col).toString();
                child.style.marginTop = `${offsets[col]}px`;
                offsets[col] += height;
                idx++;
            }
            wrapper.style.gridAutoRows = "0px";
            wrapper.style.paddingBottom = `${Math.max(...offsets)}px`;
        }
        refresh_request = null;
    }

    $effect(() => {
        wrapper && items.length && refresh_layout();
    });

    onDestroy(() => {
        if (refresh_request) clearTimeout(refresh_request);
    });
</script>

<svelte:window
    onresize={() => {
        if (!refresh_request) refresh_request = setTimeout(refresh_layout, 10);
    }}
/>

<div class="wrapper" bind:this={wrapper}>
    {#each items as it, idx (key === null ? idx : it[key])}
        <div class="item" data-index={idx}>
            {@render children(it, idx)}
        </div>
    {/each}
</div>

<style>
    .wrapper {
        display: grid;
        grid-auto-rows: 0px;
        grid-template-columns: repeat(
            auto-fill,
            minmax(var(--col-width, 40ch), 1fr)
        );
        column-gap: var(--gap, 1em);
    }
    .item {
        align-self: start;
        padding-bottom: var(--gap, 1em);
    }
</style>
