<script>
    import { onMount } from 'svelte';
    let distance = $state(0);
    let visibility = $state(0);
    let el;
    let elRect = $state({});


    function distanceToVisibility(distance) {
        return Number((1/(distance / 50)).toFixed(3));
    }

    function updateVisibilityByMouse(event) {
        let visibilityByMouse = 0;

        const elRect = el.getBoundingClientRect();
        const mouseY = event.clientY;
        const elY = elRect.top;
        distance = Math.abs(mouseY - elY);
        visibilityByMouse = distanceToVisibility(distance);
        visibility = visibilityByMouse;
    }

    
    onMount(() => {
        window.addEventListener('mousemove', updateVisibilityByMouse);
        return () => {
            window.removeEventListener('mousemove', updateVisibilityByMouse);
        };
    });
</script>

<!-- <p style="" bind:this={el}> -->
<p style="color: rgba(0,0,0,{visibility}); background-color: rgba(255,255,255,{visibility});" bind:this={el}>
    <span class="dist">
        <!-- {visibility} -->
    </span>
    <slot />
</p>

<style>
    p {
        position: relative;
        margin-bottom: 2rem;
        line-height: 1.5;
        padding: 1rem 2rem;
        border-radius: 10px;
        }
    p::before {
        content: '';
        position: absolute;
        top: 0;
        left: 10px;
        border-left: 3px solid hsl(252, 15%, 70%);
        height: 100%;
        border-radius: 3px;
    }
    .dist {
        position: absolute;
        top: 0;
        left: -100px;
    }
</style>
