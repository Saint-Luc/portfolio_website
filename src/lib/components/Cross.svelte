<script>
    let { clicked = false } = $props();

    let top = $state(), btm = $state(), left = $state(), right = $state();

    $effect(() => {
        if (clicked) {
            animate_line(top, 0, -2, 200, 0);
            animate_line(btm, 0, 2, 200, 0);
            animate_line(left, -8, 0, 200, 0);
            animate_line(right, 8, 0, 200, 0);
        }
        else if (!clicked) {
            animate_line(top, 0, 0, 200, 350);
            animate_line(btm, 0, 0, 200, 350);
            animate_line(left, 0, 0, 200, 350);
            animate_line(right, 0, 0, 200, 350);
        }
    });

    function animate_line(line, x_offset, y_offset, duration, delay) {
        line.animate([{['transform']: `translate(${x_offset}vmin, ${y_offset}vmin`}], {
            duration: duration,
            delay: delay,
            fill: 'forwards',
            easing: 'cubic-bezier(.48, .28, .31, .71)'
        });
    }
</script>

<svg class="absolute top-1/2 left-1/2 z-10 -translate-x-1/2 -translate-y-1/2 w-[40vmin] h-[56vmin] pointer-events-none" viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
<!--    <rect x="0" y="0" width="8" height="8" fill="red"/>-->
<!--    <rect x="391" y="291" width="8" height="8" fill="red"/>-->
<!--    <rect x="0" y="291" width="8" height="8" fill="red"/>-->
<!--    <rect x="391" y="0" width="8" height="8" fill="red"/>-->
<!--    <rect x="198" y="148" width="4" height="4" fill="red"/>-->
    <line x1="200" y1="145" x2="200" y2="150" stroke="#c4c4c4" stroke-width="1" bind:this={top}/>
    <line x1="200" y1="150" x2="200" y2="155" stroke="#c4c4c4" stroke-width="1" bind:this={btm}/>
    <line x1="195" y1="150" x2="200" y2="150" stroke="#c4c4c4" stroke-width="1" bind:this={left}/>
    <line x1="200" y1="150" x2="205" y2="150" stroke="#c4c4c4" stroke-width="1" bind:this={right}/>
</svg>
