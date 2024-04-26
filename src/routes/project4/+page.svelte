<script lang="ts">
    import img_src from '$lib/assets/philipp-a-urlich-cpncept572c.jpg';
    import {cubicInOut, expoIn, expoInOut, expoOut} from "svelte/easing";

    let inner_width: number = $state(0);
    let inner_height: number = $state(0);
    let scroll_value: number = $state(0);

    let img : HTMLImageElement = $state();

    function scale() {
        return {
            duration: 1000,
            easing: expoInOut,
            css: t => `width: calc(40vmin + ${t} * (${inner_width}px - 40vmin)); height: calc(56vmin + ${t * 2} * (${inner_height}px - 56vmin));`
        };
    }

    function scale_img() {
        return {
            duration: 1000,
            easing: expoInOut,
            css: t => `width: ${(0.25 * t + 0.75) * inner_width}px;`
        }
    }

    $effect(() => {
        if (scroll_value != 0)
            img.animate([{['objectPosition']: `center ${0.4 * (scroll_value)}px`}], {
                duration: 0,
                fill: 'forwards',
                easing: 'cubic-bezier(.48, .28, .31, .71)'
            })
    });
</script>

<svelte:window bind:innerWidth={inner_width} bind:innerHeight={inner_height} bind:scrollY={scroll_value}></svelte:window>

<div class="w-screen h-screen flex items-center justify-center">
    <div class="w-screen h-screen flex items-center justify-center overflow-hidden" transition:scale >
        <div>
            <img class="w-screen object-cover" src="{img_src}" alt="project" draggable="false" transition:scale_img bind:this={img} />
        </div>
    </div>
</div>
<div class="w-screen h-screen"></div>