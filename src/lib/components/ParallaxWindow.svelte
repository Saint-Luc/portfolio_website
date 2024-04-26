<script lang="ts">
    import {slide} from "svelte/transition";

    let { img_src = '', route ='', inner_width = 0, half_width = 0, duration = 1200, position = 0, moving = false, clicked = false, flush_states = false } = $props();

    let anchor_hover = $state(false);

    let img: HTMLImageElement = $state();

    let div: HTMLDivElement = $state();
    let div_rect = $derived.by(() => {
        return div ? div.getBoundingClientRect() : undefined;
    });
    let div_relative_center = $derived.by(() => {
        return div_rect ? (inner_width / 2) - (div_rect.left + half_width) : 0;
    });

    $effect(() => {
        if (position != 0)
            img.animate([{['objectPosition']: `${0.4 * (div_relative_center - half_width - position)}px center`}], {
                duration: duration,
                fill: 'forwards',
                easing: 'ease-in-out'
            })
    });

    $effect(() => {
        if (moving)
            clicked = false;
    })

    function prepare_travel() {
        if (!moving && !anchor_hover) {
            clicked = !clicked;
            position = div_relative_center - half_width;
        }
    }

    function toggle_flush_states() {
        if (!anchor_hover)
            flush_states = true;
    }

    function toggle_anchor_hover() {
        anchor_hover = !anchor_hover;
    }

    function handle_space_enter(e) {
        if (e.keyCode === 32 || e.keyCode === 13)
            return prepare_travel();
    }
</script>

<div role="toolbar" tabindex="0" class="relative w-[40vmin] h-[56vmin] flex items-center justify-center overflow-hidden" draggable="false" bind:this={div} onmousedown={toggle_flush_states} onclick={prepare_travel} onkeydown={handle_space_enter}>
    <div>
        <img class="w-[75vw] object-cover" alt="image1" src="{img_src}" style="object-position: {0.4 * div_relative_center}px" draggable="false" bind:this={img}/>
    </div>
    {#if clicked}
        <a class="absolute text-[1.5vmin]" href={route} draggable="false" onmouseenter={toggle_anchor_hover} onmouseleave={toggle_anchor_hover} in:slide={{delay: 250, duration: 500}} out:slide={{duration: 400}}>TRAVEL HERE</a>
    {/if}
</div>