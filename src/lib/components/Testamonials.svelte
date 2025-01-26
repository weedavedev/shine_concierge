<script>
    import { testamonials_data } from "$lib/data/testamonials.js";
    import '$lib/styles/testamonials.css';

    let currentIndex = 0;

    const prevSlide = () => {
        currentIndex = (currentIndex - 1 + testamonials_data.length) % testamonials_data.length;
    };

    const nextSlide = () => {
        currentIndex = (currentIndex + 1) % testamonials_data.length;
    };

    setInterval(nextSlide, 20000);
</script>

<div class="testimonials-container">
    <button class="nav-button prev" on:click={prevSlide}>←</button>

    <div class="testimonials-wrapper">
        {#each testamonials_data as testimonial, i}
            <div class="testimonial-slide {currentIndex === i ? 'active' : ''}"
                 style="transform: translateX({(i - currentIndex) * 100}%);">
                <div class="testimonial-card">
                    <div class="rating-stars">{'★'.repeat(testimonial.stars)}</div>
                    <p class="testimonial-text">{testimonial.review}</p>
                    <div class="testimonial-meta">
                        <span>{testimonial.name}</span>
                        <span>{testimonial.date}</span>
                    </div>
                </div>
            </div>
        {/each}
    </div>

    <button class="nav-button next" on:click={nextSlide}>→</button>
</div>