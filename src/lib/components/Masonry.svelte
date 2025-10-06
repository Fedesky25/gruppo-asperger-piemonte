<script lang="ts" generics="T extends {}, K extends keyof T">
    import { onDestroy, type Snippet } from "svelte";

    interface Props {
        key?: K | null;
        items: T[];
        children: Snippet<[T, number]>;
    }
    let { key = null, items, children }: Props = $props();

    let ghost: HTMLDivElement;
    let wrapper: HTMLDivElement;
    let refresh_request: number | null = null;
    let old_ncols = 0;

    function argmin(arr: number[]) {
        const N = arr.length;
        let min = arr[0];
        let idx_min = 0;
        for (let i = 1; i < N; i++) {
            if (arr[i] < min) {
                idx_min = i;
                min = arr[i];
            }
        }
        return idx_min;
    }

    function refresh_layout() {
        console.log("Masonry layout refreshed");

        const ncols =
            getComputedStyle(ghost).gridTemplateColumns.split(" ").length;

        if (ncols > 1) {
            const offsets = new Array<number>(ncols).fill(0);
            for (const _child of wrapper.children) {
                const child = _child as HTMLDivElement;
                const height = child.getBoundingClientRect().height;
                const col = argmin(offsets);
                child.style.gridColumn = (col + 1).toString();
                child.style.marginTop = `${offsets[col]}px`;
                offsets[col] += height;
            }
            wrapper.style.gridAutoRows = "0px";
            wrapper.style.paddingBottom = `${Math.max(...offsets)}px`;
        } else if (old_ncols != 1) {
            wrapper.style.gridAutoRows = "auto";
            wrapper.style.paddingBottom = "0";
            for (const child of wrapper.children) {
                (child as HTMLDivElement).style.marginTop = "0px";
                (child as HTMLDivElement).style.gridColumn = "1";
            }
        }
        old_ncols = ncols;
        refresh_request = null;
    }

    function request_refresh() {
        if (!refresh_request) refresh_request = setTimeout(refresh_layout, 20);
    }

    $effect(() => {
        wrapper && items.length && refresh_layout();
    });

    onDestroy(() => {
        if (refresh_request) clearTimeout(refresh_request);
    });
</script>

<svelte:window onresize={request_refresh} />

<div class="grid" bind:this={ghost}></div>
<div class="grid" bind:this={wrapper} onload={request_refresh}>
    {#each items as it, idx (key === null ? idx : it[key])}
        <div class="item" data-index={idx}>
            {@render children(it, idx)}
        </div>
    {/each}
</div>

<style>
    .grid {
        display: grid;
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
