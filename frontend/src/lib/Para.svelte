<script>
    import { onMount } from 'svelte';
    let distance = $state(0);
    let visibility = $state(0);
    let el;


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
<p style="color: rgba(0,0,0,{visibility});" bind:this={el}>
    <span class="dist">
        <!-- {visibility} -->
    </span>
    <slot />
</p>

<style>
    p {
        position: relative;
        border-left: 3px solid palevioletred;
        padding-left: 10px;
        margin-bottom: 2rem;
        line-height: 1.5;
    }
    .dist {
        position: absolute;
        top: 0;
        left: -100px;
    }
    .highlight {
        background-color: red;
    }
</style>
