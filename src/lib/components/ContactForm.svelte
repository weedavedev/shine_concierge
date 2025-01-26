<!-- src/lib/components/ContactForm.svelte -->
<script>
	let formData = {
		name: '',
		email: '',
		message: '',
		botcheck: false
	};

	let isSubmitting = false;
	let submitStatus = '';

	async function handleSubmit(event) {
		event.preventDefault();
		isSubmitting = true;
		submitStatus = '';

		try {
			const response = await fetch('https://api.web3forms.com/submit', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json',
					'Accept': 'application/json'
				},
				body: JSON.stringify({
					access_key: import.meta.env.VITE_W3F_KEY,
					subject: "New Contact Form Submission",
					from_name: "Website Contact Form",
					botcheck: formData.botcheck,
					...formData
				})
			});

			if (response.ok) {
				submitStatus = 'success';
				formData = {
					name: '',
					email: '',
					message: '',
					botcheck: false
				};
			} else {
				throw new Error('Submission failed');
			}
		} catch (error) {
			submitStatus = 'error';
		} finally {
			isSubmitting = false;
		}
	}
</script>

<form on:submit={handleSubmit} class="contact-form">
	<input type="checkbox" name="botcheck" class="hidden" bind:checked={formData.botcheck}>

	<div class="form-group">
		<label for="name">Name</label>
		<input type="text" id="name" bind:value={formData.name} required disabled={isSubmitting} />
	</div>

	<div class="form-group">
		<label for="email">Email</label>
		<input type="email" id="email" bind:value={formData.email} required disabled={isSubmitting} />
	</div>

	<div class="form-group">
		<label for="message">Message</label>
		<textarea id="message" bind:value={formData.message} rows="4" required disabled={isSubmitting}></textarea>
	</div>

	<button type="submit" disabled={isSubmitting}>
		{#if isSubmitting}
   		<span class="flex items-center">
   			<svg class="animate-spin mr-3 h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
   				<circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
   				<path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
   			</svg>
   			Sending...
   		</span>
		{:else}
			Send Message
		{/if}
	</button>

	{#if submitStatus === 'success'}
		<div class="success">Thank you for your message! We'll get back to you soon.</div>
	{:else if submitStatus === 'error'}
		<div class="error">Sorry, there was an error sending your message. Please try again.</div>
	{/if}
</form>

<style>
	@import '../styles/contact-form.css';
</style>