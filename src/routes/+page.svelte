<script lang="ts">
    import { onMount, onDestroy } from "svelte";

    let mainDiv: HTMLElement | null = null;

    const scrollToMain = () => {
        mainDiv?.scrollIntoView({ behavior: 'smooth' });
    };

    let isAtTop = true;

    const handleWheel = (e: WheelEvent) => {
        if (isAtTop && e.deltaY > 0) {
            e.preventDefault();
            scrollToMain();
        }
    };

    const checkScrollPosition = () => {
        isAtTop = window.scrollY === 0;
    };

    onMount(() => {
        if (typeof window !== 'undefined') {
            window.addEventListener('wheel', handleWheel, { passive: false });
            window.addEventListener('scroll', checkScrollPosition);
        }
    });

    onDestroy(() => {
        if (typeof window !== 'undefined') {
            window.removeEventListener('wheel', handleWheel);
            window.removeEventListener('scroll', checkScrollPosition);
        }
    });
</script>

<div class="hero">
    <div class="hero-top">
        <h1>WARNING</h1>
    </div>
    <div class="hero-center">
        <h1>This website is for people with style, curiosity and passion</h1>
        <h1>Handcrafted with love and attention to detail</h1>
        <h1>Proceed at your own discretion</h1>
    </div>
    <div class="hero-bottom">
        <button on:click={scrollToMain}>ENTER</button>
    </div>
</div>
<div bind:this={mainDiv} class="main-content">
    <div>
        <h1>Home</h1>
        <p><i>Note: This website is best experienced on a PC. Responsive design is limited.</i></p>
        <p>Test</p>
    </div>
</div>
