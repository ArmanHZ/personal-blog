<script lang="ts">
    import { onMount, onDestroy } from "svelte";
    import RowItem from "$lib/components/RowItem.svelte";

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
        <p>Welcome to my part of the internet. I'm <i class="blue-text">Arman</i>.</p>
        <p>
            The purpose of this website is for me to share my interests with the world.
            For me to reflect back, for people to read and learn.
        </p>
        <p>
            My main interests are <i class="blue-text">GNU/Linux</i> <i class="blue-text">Web Security</i> and <i class="blue-text">Tool development</i>.
            So, most of the blog posts will be about these.
        </p>
        <p>However, I also want to write about my more recent interests.</p>
        <p>
            Tech related stuff from CTFs, such as <i class="blue-text">Forensics</i>, <i class="blue-text">OSINT</i> and <i class="blue-text">Reversing</i>.
            <br />
            Non-tech related things like <i class="blue-text">Philosophy</i> and <i class="blue-text">Text analysis</i>.
        </p>
    </div>

    <h1>Links to places</h1>
    <div>
        <RowItem
            title="Blog posts"
            link="/blog"
            content="My posts on various subjects."
        />
        <RowItem
            title="Writeups"
            link="https://github.com/ArmanHZ/writeups"
            content="Writeups for CTFs and HTB boxes. This is where you should go if you're a recruiter and want to see some examples of my documentation or if you're curios and learning about CTFs and boxes."
        />
        <RowItem
            title="Go Detect That Hash!"
            link="https://github.com/ArmanHZ/go-detect-that-hash"
            content="Hash detector written in Go. Inspired by Python hashID."
        />
    </div>

    <br />
    <h1>Friends!</h1>
    <p>You should definitely check out the following landchads as well.</p>
    <RowItem
        title="OpenPunk"
        link="https://openpunk.com/"
        content="A site about making and breaking things."
    />
</div>
