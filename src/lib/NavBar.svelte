<!-- src/routes/Navbar.svelte -->
<script>
    import {base} from '$app/paths';
    import {afterUpdate} from "svelte";

    afterUpdate(() => {
        console.log("Base path is:", base);
    }); // For debugging
</script>