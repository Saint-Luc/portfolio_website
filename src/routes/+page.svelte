<script>
    import ParallaxWindow from "$lib/components/ParallaxWindow.svelte";
    import img1 from '$lib/assets/anato-finnstark-the-nine.jpg'
    import img2 from '$lib/assets/anato-finnstark-anato-finnstark-anato-finnstark-web-petit-1.jpg'
    import img3 from '$lib/assets/Harvest Godess.jpg'
    import img4 from '$lib/assets/Spellforce - Conquest of EO - Keyart 01.jpg'
    import img5 from '$lib/assets/Dead Flowers.jpg'
    import cross from "$lib/assets/pixel_cross.svg";

    let inner_width = $state(0);

    let track = $state();
    let click_x = $state(0);
    let delta = $state(0);
    let prev_delta = $state(0);

    function handle_mouse_move(event) {
        if(click_x === 0) return;

        let mouse_delta = event.clientX - click_x;
        // delta = Math.min((track.clientWidth - 5 * 56) / -10, Math.max(2 * mouse_delta + prev_delta, -track.clientWidth - (track.clientWidth - 5 * 56) / -10));
        delta = Math.min(0, Math.max(2 * mouse_delta + prev_delta, -track.clientWidth));
    }

    function handle_mouse_down(event) {
        click_x = event.clientX;
    }

    function handle_mouse_up() {
        click_x = 0;
        prev_delta = delta;
    }

    $effect(() => {
        track.animate([{ ['transform']: `translate(${delta}px, 0)` }], { duration: 1200, fill: 'forwards', easing: 'cubic-bezier(.48, .28, .31, .71)' });
    })
</script>

<svelte:window bind:innerWidth={inner_width} />

<img class="absolute top-1/2 left-1/2 z-10 -translate-x-1/2 -translate-y-1/2 w-[22px]" alt="cross" src={cross} />
<div role="toolbar" tabindex="0" class="relative w-screen h-screen flex justify-center items-center select-none" onmousemove={handle_mouse_move} onmousedown={handle_mouse_down} onmouseup={handle_mouse_up}>
    <div class="absolute left-1/2 flex justify-center items-center gap-x-14" bind:this={track}>
        <ParallaxWindow img_src={img1} inner_width={inner_width} bind:position={delta} />
        <ParallaxWindow img_src={img2} inner_width={inner_width} bind:position={delta} />
        <ParallaxWindow img_src={img3} inner_width={inner_width} bind:position={delta} />
        <ParallaxWindow img_src={img4} inner_width={inner_width} bind:position={delta} />
        <ParallaxWindow img_src={img5} inner_width={inner_width} bind:position={delta} />
    </div>
</div>