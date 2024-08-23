<script>
    import {logoSource} from '$lib/constants';

    let logo_source = logoSource;

    let name = '';
    let email = '';
    let message = '';

    const handleSubmit = (event) => {
        event.preventDefault();
        // Here you can handle the form submission (e.g., send it to a backend or display a message)
        console.log({name, email, message});
        // Reset the form fields
        name = '';
        email = '';
        message = '';
    };
</script>

<style>
    .holding_page_content {
        display: flex;
        align-items: center;
        justify-content: center;
        min-height: 100vh;
        padding: 0 10%;
        box-sizing: border-box;
    }

    .center_image {
        max-width: 750px;
        width: 100%;
        height: auto;
    }

    .contact-form {
        background: white; /* Form background color */
        padding: 20px; /* Padding within the form */
        border-radius: 5px; /* Rounded corners */
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* Subtle shadow */
        width: 100%;
        max-width: 500px; /* Max width of the form */
        display: flex;
        flex-direction: column;
    }

    .form-input {
        margin-bottom: 15px; /* Space between form elements */
    }

    .form-input label {
        margin-bottom: 5px; /* Space between label and input */
    }

    .form-input input,
    .form-input textarea {
        width: 100%; /* Full width of the parent */
        padding: 10px; /* Padding inside the input */
        border: 1px solid #ccc; /* Border for input */
        border-radius: 3px; /* Rounded corners */
        font-size: 16px; /* Input font size */
    }

    .form-input textarea {
        resize: vertical; /* Allow vertical resizing of textarea */
        min-height: 100px; /* Minimum height for textarea */
    }

    .submit-button {
        background-color: #007bff; /* Button background color */
        color: white; /* Button text color */
        border: none; /* Remove border */
        padding: 10px; /* Padding within the button */
        border-radius: 3px; /* Rounded corners */
        cursor: pointer; /* Pointer cursor */
        font-size: 16px; /* Button font size */
    }

    .submit-button:hover {
        background-color: #0056b3; /* Darker background on hover */
    }
</style>

<div class="holding_page_content">
    <img class="center_image" src={logo_source} alt="Shine Logo"/>
    <!--
        <form on:submit={handleSubmit} class="contact-form">
            <div class="form-input">
                <label for="name">Name</label>
                <input type="text" id="name" bind:value={name} required />
            </div>
            <div class="form-input">
                <label for="email">Email</label>
                <input type="email" id="email" bind:value={email} required />
            </div>
            <div class="form-input">
                <label for="message">Message</label>
                <textarea id="message" bind:value={message} required></textarea>
            </div>
            <button type="submit" class="submit-button">Send Message</button>
        </form>-->

</div>

