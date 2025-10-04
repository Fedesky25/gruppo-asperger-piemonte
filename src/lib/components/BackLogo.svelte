<script lang="ts">
    import { onMount } from "svelte";
    import { prefersReducedMotion } from "svelte/motion";

    function rnd_window() {
        return 2.0 * Math.random() - 1.0;
    }

    class Point2D {
        public x = $state<number>(0);
        public y = $state<number>(0);
        public readonly str = $derived(`${this.x},${this.y}`);

        private readonly cx: number;
        private readonly cy: number;

        private r: number = 2;
        private a: number = Math.random() * Math.PI * 2;
        private vr: number = 0; // radial velocity
        private va: number = 0; // angle velocity

        constructor(x: number, y: number) {
            this.x = this.cx = x;
            this.y = this.cy = y;
        }

        update(dt: number) {
            this.r += this.vr * dt;
            this.a += this.va * dt;
            this.x = this.cx + this.r * Math.cos(this.a);
            this.y = this.cy + this.r * Math.sin(this.a);

            this.vr -= 4 * this.r * dt;
            this.va *= 1 - 3 * dt;
            if (this.va < 0.1) this.va += rnd_window() * 0.3;
        }

        toString() {
            return `${this.x},${this.y}`;
        }
    }

    const small_dots = [
        new Point2D(78.08, 35.19),
        new Point2D(154.63, 97.57),
        new Point2D(245.49, 140.61),
        new Point2D(276.28, 210.25),
        new Point2D(190.61, 270.14),
        new Point2D(106.39, 220.39),
    ];

    const big_dots = [
        new Point2D(181.35, 11.07),
        new Point2D(310.02, 86.05),
        new Point2D(56.25, 146.14),
        new Point2D(181.35, 186.86),
        new Point2D(236.07, 355.85),
    ];

    let last_time = 0.0;
    function animate(timestamp: number) {
        if (!last_time) last_time = timestamp;
        const dt = Math.min(34, timestamp - last_time) * 1e-3;
        // console.log(dt);
        last_time = timestamp;
        for (let i = 0; i < small_dots.length; i++) small_dots[i].update(dt);
        for (let i = 0; i < big_dots.length; i++) big_dots[i].update(dt);
        requestAnimationFrame(animate);
    }

    onMount(() => requestAnimationFrame(animate));
</script>

<svg
    xmlns="http://www.w3.org/2000/svg"
    width="365"
    height="455"
    viewBox="0 0 365 455"
>
    <!-- <rect x="0" width="365" height="455" fill="black" rx="60" ry="60" /> -->
    <g transform="scale(1,-1) translate(0 -410)">
        <g fill="none" stroke="#e8a92e" stroke-width="17">
            <path d="M{big_dots[0].str} L{big_dots[1].str}" />
            <path d="M{big_dots[0].str} L{small_dots[0].str}" />
            <path d="M{big_dots[0].str} L{small_dots[1].str}" />

            <path d="M{big_dots[1].str} L{small_dots[1].str}" />
            <path d="M{big_dots[1].str} L{small_dots[2].str}" />

            <path d="M{big_dots[2].str} L{big_dots[3].str}" />
            <path d="M{big_dots[2].str} L{small_dots[0].str}" />
            <path d="M{big_dots[2].str} L{small_dots[1].str}" />
            <path d="M{big_dots[2].str} L{small_dots[5].str}" />

            <path d="M{big_dots[3].str} L{small_dots[1].str}" />
            <path d="M{big_dots[3].str} L{small_dots[2].str}" />
            <path d="M{big_dots[3].str} L{small_dots[3].str}" />
            <path d="M{big_dots[3].str} L{small_dots[4].str}" />
            <path d="M{big_dots[3].str} L{small_dots[5].str}" />

            <path d="M{big_dots[4].str} L{small_dots[3].str}" />
            <path d="M{big_dots[4].str} L{small_dots[4].str}" />

            <path
                d="M{small_dots[0].str}L{small_dots[1].str}L{small_dots[2]
                    .str}L{small_dots[3].str}L{small_dots[4].str}L{small_dots[5]
                    .str}"
            />
        </g>
        <g fill="#851341">
            {#each small_dots as p}
                <ellipse cx={p.x} cy={p.y} rx="24.13" ry="24.13" />
            {/each}
        </g>
        <g fill="#3a8e8f">
            {#each big_dots as p}
                <ellipse cx={p.x} cy={p.y} rx="40.2" ry="40.2" />
            {/each}
        </g>
    </g>
</svg>

<style>
    svg {
        max-width: 100%;
        max-height: 100%;
        width: 100%;
        height: auto;
    }
</style>
