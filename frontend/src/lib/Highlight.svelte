<script>
    let { importance = 100 } = $props();

    import { onMount } from "svelte";
    let distance = $state(0);
    
    let visibilityByMouse = $state(0);
    let visibilityByScroll = $state(1);
    
    let visibility = $derived.by(() => Math.max(visibilityByScroll, visibilityByMouse));
    
    let bias = .1;
    let el;


    function updateVisibilityByScroll() {
        const rect = el.getBoundingClientRect();
        const topDistance = rect.top;
        const maxDistance = window.innerHeight - rect.height;
        
        // Linear decrease from 1 to 0 as element approaches top of screen
        visibilityByScroll = Math.min(1, topDistance / maxDistance);
        visibilityByScroll = Math.max(bias, visibilityByScroll);

        // Scale by importance
        visibilityByScroll = visibilityByScroll * (importance / 100);

        // Round to 3 decimal places
        visibilityByScroll = visibilityByScroll.toFixed(3);

        // return visibilityByScroll;
    }

    function updateVisibilityByMouse(event) {

        const elY = el.getBoundingClientRect().top;
        console.log(elY);
        const maxDistance = window.innerHeight - el.getBoundingClientRect().height;
        
        const mouseY = event.clientY;
        console.log({mouseY});

        distance = Math.abs(mouseY - elY);
        console.log({distance});
        
        visibilityByMouse = 1 - (distance / maxDistance) ;
        visibilityByMouse = visibilityByMouse.toFixed(3);
    }

    onMount(() => {
        updateVisibilityByScroll();
        window.addEventListener("scroll", (e) => updateVisibilityByScroll(e));
        window.addEventListener("mousemove", (e) => updateVisibilityByMouse(e));
        return () => {
            window.removeEventListener("scroll", (e) => updateVisibilityByScroll(e));
            window.removeEventListener("mousemove", (e) => updateVisibilityByMouse(e));
        };
    });
</script>

<span
    class="highlight"
    style="color: rgba(0,0,0,{visibility});"
    bind:this={el}
>
    <!-- {importance} +
    {visibility} -->
    <slot />
</span>

<style>
    .highlight {
        /* color: rgba(0, 0, 0, 1); */
        /* background-color: aliceblue; */
    }
</style>
