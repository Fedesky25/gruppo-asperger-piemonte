<script lang="ts">
    import { onMount } from "svelte";

    interface Props {
        images: Array<{ url: string; title: string }>;
    }
    let { images }: Props = $props();
    let selected = $state(images.length - 1);
    let nextImgRequest = 0;
    const DELTA_TIME = 4000;

    function nextImage() {
        if (selected == 0) selected = images.length - 1;
        else selected--;
    }

    function forceNextImage() {
        clearInterval(nextImgRequest);
        nextImage();
        nextImgRequest = setInterval(nextImage, DELTA_TIME);
    }

    onMount(() => {
        nextImgRequest = setInterval(nextImage, DELTA_TIME);
        return () => clearInterval(nextImgRequest);
    });
</script>

<div>
    <button
        style:z-index={images.length + 1}
        aria-label="Next image"
        onclick={forceNextImage}
    ></button>
    {#each images as image, idx (image.url)}
        <img
            src={image.url}
            alt={image.title}
            style:z-index={idx}
            aria-hidden={idx != selected}
            class:hidden={idx > selected}
        />
    {/each}
</div>

<style>
    div {
        width: 100%;
        height: 100%;
        position: relative;
    }
    img,
    button {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
    button {
        background: transparent;
        border: none;
    }
    img {
        object-fit: cover;
        transition: opacity 0.25s ease;
    }
    img.hidden {
        opacity: 0;
    }
</style>
