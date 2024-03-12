<script lang="ts">
    import img1 from '$lib/assets/anato-finnstark-the-nine.jpg'
    import img2 from '$lib/assets/anato-finnstark-anato-finnstark-anato-finnstark-web-petit-1.jpg'
    import img3 from '$lib/assets/Harvest Godess.jpg'
    import img4 from '$lib/assets/Spellforce - Conquest of EO - Keyart 01.jpg'
    import img5 from '$lib/assets/Dead Flowers.jpg'
    import img6 from '$lib/assets/The Witch King of Angmar.jpg'
    import cross from "$lib/assets/pixel_cross.svg";
    import ParallaxWindow from "$lib/components/ParallaxWindow.svelte";

    let inner_width: number;
    let track: HTMLElement;
    let image1: HTMLImageElement, image2: HTMLElement, image3: HTMLElement, image4: HTMLElement, image5: HTMLElement, image6: HTMLElement;

    let track_width = $state(0);
    let half_width = $derived(((track_width - 56 * 5) / 12));
    let click_x = $state(0);
    let percentage = $state(8);
    let prev_percentage = $state(0);
    function handle_mouse_move(event: MouseEvent) {
        if(click_x === 0) return;

        let mouse_delta = click_x - event.clientX;
        let maxDelta = inner_width / 1.5;
        percentage = Math.max(-100 + half_width / track_width * 100, Math.min((mouse_delta / maxDelta * -100) + prev_percentage, - half_width / track_width * 100));

        animateElement(track, 'transform', `translate(${percentage}%, -50%)`);

        const image_positions = [
            { element: image1, offset_multiplier: (5 * half_width + 140) },
            { element: image2, offset_multiplier: (3 * half_width + 84) },
            { element: image3, offset_multiplier: (1 * half_width + 28) },
            { element: image4, offset_multiplier: (-1 * half_width + 28) },
            { element: image5, offset_multiplier: (-3 * half_width + 84) },
            { element: image6, offset_multiplier: (-5 * half_width + 140) }
        ];

        image_positions.forEach(({ element, offset_multiplier }) => {
            const position_value = 100 - (-percentage + offset_multiplier / track_width * 100);
            animateElement(element, 'objectPosition', `${position_value}% center`)
        });
    }

    function animateElement(element: HTMLElement, property: string, value: string) {
        element.animate([{ [property]: value }], { duration: 2400, fill: 'forwards', easing: 'cubic-bezier(.48,.28,.31,.71)' });
    }

    function handle_mouse_down(event: MouseEvent) {
        click_x = event.clientX;
    }

    function handle_mouse_up() {
        click_x = 0;
        prev_percentage = percentage;
    }
</script>

<svelte:window bind:innerWidth={inner_width} />

<img class="absolute top-1/2 left-1/2 z-10 -translate-x-1/2 -translate-y-1/2 w-[22px]" alt="cross" src={cross} />
<div role="toolbar" tabindex="0" class="h-screen w-screen text-white" onmousemove={handle_mouse_move} onmousedown={handle_mouse_down} onmouseup={handle_mouse_up}>
    <div class="flex gap-14 absolute left-1/2 top-1/2 select-none" style="transform: translate(-{half_width / track_width * 100}%, -50%)" bind:this={track} bind:clientWidth={track_width}>
        <a class="w-[40vmin] h-[56vmin] flex items-center justify-center overflow-hidden" href="/project1" draggable="false">
            <div>
                <img class="w-screen max-w-[100vw] object-cover" alt="image1" src="{img1}"
                     style="object-position: {100 - (half_width / track_width * 100 + ((5 * half_width + 140) / track_width * 100))}% center" draggable="false" bind:this={image1} />
            </div>
        </a>
        <img class="w-[40vmin] h-[56vmin] object-cover object-center" alt="image2" src="{img2}" style="object-position: {100 - (half_width / track_width * 100 + ((3 * half_width + 84) / track_width * 100))}% center" draggable="false" bind:this={image2} />
        <img class="w-[40vmin] h-[56vmin] object-cover object-center" alt="image3" src="{img3}" style="object-position: {100 - (half_width / track_width * 100 + ((half_width + 28) / track_width * 100))}% center" draggable="false" bind:this={image3} />
        <img class="w-[40vmin] h-[56vmin] object-cover object-center" alt="image4" src="{img4}" style="object-position: {100 - (half_width / track_width * 100 - ((half_width + 28) / track_width * 100))}% center" draggable="false" bind:this={image4} />
        <img class="w-[40vmin] h-[56vmin] object-cover object-center" alt="image5" src="{img5}" style="object-position: {100 - (half_width / track_width * 100 - ((3 * half_width + 84) / track_width * 100))}% center" draggable="false" bind:this={image5} />
        <img class="w-[40vmin] h-[56vmin] object-cover object-center" alt="image6" src="{img6}" style="object-position: {100 - (half_width / track_width * 100 - ((5 * half_width + 140) / track_width * 100))}% center" draggable="false" bind:this={image6} />
    </div>
</div>
