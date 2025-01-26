<!-- Testimonial.svelte -->
<script>
    import { onMount, onDestroy } from 'svelte';
    import { fade, slide } from 'svelte/transition';

    import { testamonials_data } from '$lib/data/testamonials.js';

    export let reviews = testamonials_data;
    export let title = "";
    export let display_seconds = 20;

    let currentIndex = 0;
    let timer;

    const nextReview = () => {
        currentIndex = (currentIndex + 1) % reviews.length;
    };

    const prevReview = () => {
        currentIndex = (currentIndex - 1 + reviews.length) % reviews.length;
    };

    const startTimer = () => {
        clearInterval(timer);
        timer = setInterval(nextReview, display_seconds * 1000);
    };

    onMount(() => {
        startTimer();
    });

    onDestroy(() => {
        clearInterval(timer);
    });
</script>

<div class="testimonials-container">
    {#if title}
        <h2 class="testimonials-title">{title}</h2>
    {/if}

    <div class="testimonials-carousel">
        <button class="nav-button prev" on:click={() => { prevReview(); startTimer(); }}>
            ←
        </button>

        {#key currentIndex}
            <div class="testimonial-card" in:slide={{ duration: 300 }} out:fade>
                <div class="stars">
                    {'★'.repeat(reviews[currentIndex].stars)}
                    {'☆'.repeat(5 - reviews[currentIndex].stars)}
                </div>
                <p class="review">{reviews[currentIndex].review}</p>
                <div class="meta">
                    <span class="name">{reviews[currentIndex].name}</span>
                    <span class="date">{reviews[currentIndex].date}</span>
                </div>
            </div>
        {/key}

        <button class="nav-button next" on:click={() => { nextReview(); startTimer(); }}>
            →
        </button>
    </div>
</div>

<style>
    @import '../styles/testamonials.css';
</style>