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
	<input type="checkbox" name="botcheck" class="honeypot" bind:checked={formData.botcheck}>

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
			<span class="spinner-container">
				<svg class="spinner" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
					<circle class="spinner-circle" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
					<path class="spinner-path" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
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

<style>/* src/styles/contact-form.css */
.contact-form {
	@apply w-full max-w-2xl mx-auto space-y-6 my-8;
}

.honeypot {
	@apply hidden;
}

.form-group {
	@apply space-y-2;
}

.form-group label {
	@apply block text-sm font-medium text-gray-700;
}

.form-group input,
.form-group textarea {
	@apply w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent disabled:bg-gray-100;
}

button[type="submit"] {
	@apply w-full px-4 py-2 text-sm font-medium text-white bg-blue-600 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 disabled:bg-gray-400 disabled:cursor-not-allowed;
}

.spinner-container {
	@apply inline-flex items-center;
}

.spinner {
	@apply animate-spin -ml-1 mr-3 h-5 w-5 text-white;
}

.spinner-circle {
	@apply opacity-25;
}

.spinner-path {
	@apply opacity-75;
}

.success {
	@apply p-4 bg-green-50 text-green-700 rounded-md text-center;
}

.error {
	@apply p-4 bg-red-50 text-red-700 rounded-md text-center;
}

@media (max-width: 768px) {
	.contact-form {
		@apply p-5;
	}
}
</style>