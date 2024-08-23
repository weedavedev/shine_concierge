<script>
    import Navbar from '$lib/NavBar.svelte';
    import Footer from '$lib/Footer.svelte';
    import '../styles/global.css';

</script>

<div class="layout">
    <Navbar/>
    <slot/>
    <Footer/>
</div>