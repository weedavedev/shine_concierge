<!-- ContactForm.svelte -->
<script>
	let formData = {
		name: '',
		email: '',
		message: '',
		botcheck: false  // Changed to boolean
	};

	let isSubmitting = false;
	let submitStatus = '';

	async function handleSubmit(event) {
		event.preventDefault();
		isSubmitting = true;
		submitStatus = '';

		try {
			console.log('Submitting form...'); // Debug log

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

			const result = await response.json();
			console.log('Response:', result); // Debug log

			if (response.ok) {
				console.log('Submission successful'); // Debug log
				submitStatus = 'success';
				// Reset form more explicitly
				formData = {
					name: '',
					email: '',
					message: '',
					botcheck: false
				};
				// Force UI update
				formData = {...formData};
			} else {
				console.log('Submission failed:', response.status); // Debug log
				throw new Error('Submission failed');
			}
		} catch (error) {
			console.error('Error details:', error); // Debug log
			submitStatus = 'error';
		} finally {
			isSubmitting = false;
		}
	}
</script>

<form on:submit={handleSubmit} class="w-full max-w-2xl mx-auto space-y-6">
	<!-- Honeypot field - changed to bind:checked -->
	<input
		type="checkbox"
		name="botcheck"
		class="hidden"
		bind:checked={formData.botcheck}
	>

	<div class="space-y-2">
		<label for="name" class="block text-sm font-medium text-gray-700">Name</label>
		<input
			type="text"
			id="name"
			bind:value={formData.name}
			required
			disabled={isSubmitting}
			class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent disabled:bg-gray-100"
		/>
	</div>

	<div class="space-y-2">
		<label for="email" class="block text-sm font-medium text-gray-700">Email</label>
		<input
			type="email"
			id="email"
			bind:value={formData.email}
			required
			disabled={isSubmitting}
			class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent disabled:bg-gray-100"
		/>
	</div>

	<div class="space-y-2">
		<label for="message" class="block text-sm font-medium text-gray-700">Message</label>
		<textarea
			id="message"
			bind:value={formData.message}
			rows="4"
			required
			disabled={isSubmitting}
			class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent disabled:bg-gray-100"
		></textarea>
	</div>

	<button
		type="submit"
		disabled={isSubmitting}
		class="w-full px-4 py-2 text-sm font-medium text-white bg-blue-600 rounded-md hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500 disabled:bg-gray-400 disabled:cursor-not-allowed"
	>
		{#if isSubmitting}
      <span class="inline-flex items-center">
        <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
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
		<div class="p-4 bg-green-50 text-green-700 rounded-md">
			<p class="text-center">Thank you for your message! We'll get back to you soon.</p>
		</div>
	{:else if submitStatus === 'error'}
		<div class="p-4 bg-red-50 text-red-700 rounded-md">
			<p class="text-center">Sorry, there was an error sending your message. Please try again.</p>
		</div>
	{/if}
</form>