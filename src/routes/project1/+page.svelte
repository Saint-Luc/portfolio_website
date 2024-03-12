<script lang="ts">
    import img_src from '$lib/assets/anato-finnstark-the-nine.jpg'
    import {cubicInOut, expoIn, expoInOut, expoOut} from "svelte/easing";

    let inner_width: number;
    let inner_height: number;
    let img_height: number;
    let img_width: number;

    function img_init() {
        const img = new Image();
        img.src = img_src;
        img.onload = () => {
            img_height = img.naturalHeight;
            img_width = img.naturalWidth;
        }
    }

    function scale() {
        return {
            duration: 1000,
            easing: expoOut,
            css: t => `width: calc(40vmin + ${t} * (${inner_width}px - 40vmin)); height: calc(56vmin + ${t} * (${inner_height}px - 56vmin));`
        };
    }

    function scale_img() {
        return {
            duration: 1000,
            easing: expoOut,
            css: t => `height: ${(0.25 * t + 0.75) * 1600 * inner_width / 2587}px;`
        }
    }
</script>

<svelte:window bind:innerWidth={inner_width} bind:innerHeight={inner_height}></svelte:window>

<div class="w-screen h-screen flex items-center justify-center">
    <div class="w-screen h-screen flex items-center justify-center overflow-hidden" transition:scale>
        <img class="object-cover" src="{img_src}" style="height: {img_height * inner_width / img_width}px;" alt="project" draggable="false" use:img_init transition:scale_img />
    </div>
</div>
<div class="w-screen h-screen"></div>