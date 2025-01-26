<script>
    import {base} from "$app/paths";

    import Image from '$lib/components/Image.svelte';
    import Testamonials from '$lib/components/Testamonials.svelte';
    import ContactForm from '$lib/components/ContactForm.svelte';

    const logoSource = `${base}/media/shine_logo1_cropped.png`;

</script>

<div class="holding_page_content">
    <Image
            src={logoSource}
            alt="Shine Logo"
            className="center_image mx-auto w-auto"
    />

    <Testamonials	/>

    <ContactForm	/>
</div>

