<script lang="ts">
    import {goto} from "$app/navigation";

    let { img_src = '', inner_width = 0, position = 0 } = $props();

    let img: HTMLImageElement = $state();

    let anchor: HTMLButtonElement = $state();
    let anchor_rect = $derived.by(() => {
        return anchor ? anchor.getBoundingClientRect() : undefined;
    });
    let anchor_relative_center = $derived.by(() => {
        return anchor_rect ? (inner_width / 2) - (anchor_rect.left + anchor_rect.width / 2) : 0;
    });

    $effect(() => {
        img.animate([{ ['objectPosition']: `${0.4 * anchor_relative_center - 0.4 * position}px center` }], { duration: 1200, fill: 'forwards', easing: 'cubic-bezier(.48, .28, .31, .71)' })
    })

    function transition_and_redirect() {
        position = anchor_relative_center;
    }
</script>

<button class="w-[40vmin] h-[56vmin] flex items-center justify-center overflow-hidden bg-blue-600" draggable="false" bind:this={anchor} onclick={transition_and_redirect}>
    <div>
        <img class="w-screen object-cover" alt="image1" src="{img_src}" style="object-position: {0.4 * anchor_relative_center}px" draggable="false" bind:this={img} />
    </div>
</button>