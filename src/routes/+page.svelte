<script>
    import ParallaxWindow from "$lib/components/ParallaxWindow.svelte";
    import Cross from "$lib/components/Cross.svelte";

    import img1 from '$lib/assets/philipp-a-urlich-cpncept408c.jpg';
    import img2 from '$lib/assets/philipp-a-urlich-cpncept593c.jpg';
    import img3 from '$lib/assets/philipp-a-urlich-cpncept604-2.jpg';
    import img4 from '$lib/assets/philipp-a-urlich-cpncept572c.jpg';
    import img5 from '$lib/assets/philipp-a-urlich-cpncept558-5-optsmall.jpg';

    let inner_width = $state(0);
    let inner_height = $state(0);

    let half_width = $derived(20 * Math.min(inner_width, inner_height) / 100);

    let track = $state();
    let track_width = $state(0);

    let click_x = $state(0);

    let clicked1 = $state(false),
        clicked2 = $state(false),
        clicked3 = $state(false),
        clicked4 = $state(false),
        clicked5 = $state(false);
    let flush_states = $state(false);
    let window_focus = $derived(clicked1 || clicked2 || clicked3 || clicked4 || clicked5);
    let moving = $state(false);

    $effect(() => {
        if (flush_states) {
            clicked1 = false;
            clicked2 = false;
            clicked3 = false;
            clicked4 = false;
            clicked5 = false;
            flush_states = false;
        }
    })

    let delta = $state(0);
    let prev_delta = $state(0);

    $effect(() => {
        delta = -half_width;
    });

    function handle_mouse_move(event) {
        if (click_x === 0) return;

        let mouse_delta = event.clientX - click_x;
        moving = Math.abs(mouse_delta) > 20;
        delta = Math.min(-half_width, Math.max(2 * mouse_delta + prev_delta, half_width - track_width));
    }

    function handle_mouse_down(event) {
        click_x = event.clientX;
        moving = false;
    }

    function handle_mouse_up() {
        click_x = 0;
        prev_delta = delta;
    }

    $effect(() => {
        if (window_focus) prev_delta = delta;
    })

    let duration = $derived.by(() => {
        return window_focus ? 200 : 1200;
    });

    $effect(() => {
        track.animate([{['transform']: `translate(${delta}px, 0)`}], {
            duration: duration,
            fill: 'forwards',
            easing: 'ease-in-out'
        });
    })
</script>

<svelte:window bind:innerWidth={inner_width} bind:innerHeight={inner_height}/>

<div class="absolute top-0 left-0 text-3xl text-white">{moving} | {window_focus}</div>
<Cross clicked={window_focus}/>
<div role="toolbar" tabindex="0" class="w-screen h-screen flex justify-center items-center select-none text-[#c4c4c4]"
     onmousemove={handle_mouse_move} onmousedown={handle_mouse_down} onmouseup={handle_mouse_up}>
    <div class="absolute left-1/2 flex justify-center items-center gap-x-14" style="transform: translate({-half_width}px, 0)" bind:this={track} bind:clientWidth={track_width}>
        <ParallaxWindow img_src={img1} route={"/project1"} inner_width={inner_width} half_width={half_width} duration={duration} moving={moving} bind:position={delta} bind:clicked={clicked1} bind:flush_states={flush_states}/>
        <ParallaxWindow img_src={img2} route={"/project2"} inner_width={inner_width} half_width={half_width} duration={duration} moving={moving} bind:position={delta} bind:clicked={clicked2} bind:flush_states={flush_states}/>
        <ParallaxWindow img_src={img3} route={"/project3"} inner_width={inner_width} half_width={half_width} duration={duration} moving={moving} bind:position={delta} bind:clicked={clicked3} bind:flush_states={flush_states}/>
        <ParallaxWindow img_src={img4} route={"/project4"} inner_width={inner_width} half_width={half_width} duration={duration} moving={moving} bind:position={delta} bind:clicked={clicked4} bind:flush_states={flush_states}/>
        <ParallaxWindow img_src={img5} route={"/project5"} inner_width={inner_width} half_width={half_width} duration={duration} moving={moving} bind:position={delta} bind:clicked={clicked5} bind:flush_states={flush_states}/>
    </div>
</div>
