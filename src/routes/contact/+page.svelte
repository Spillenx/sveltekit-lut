<script>
	import { invalidateAll } from '$app/navigation';
	import { applyAction, deserialize, enhance } from '$app/forms';

	export let form;

	$: console.log('form', form);

	// Standard onSubmit event if not using use:enhance
	async function handleForm(event) {
		//this = form element
		const data = new FormData(this);
		const res = await fetch(this.action, {
			method: 'POST',
			body: data
		});

		// Get data by deserializing it
		const result = deserialize(await res.text());

		// if success rewload all loaded data
		if (result.type === 'success') {
			// Reload all data
			await invalidateAll();
		}

		// populating form
		// will redirect of thrown redirect in action
		// show error page if thrown error
		applyAction(result);
	}
</script>

{#if form?.error_message}
	<p class="message">
		{form.error_message}
	</p>
{/if}

{#if form?.message}
	<p class="message">
		{form.message}
	</p>
{:else}
	<!-- <form
		use:enhance={({ form, data, action, cancel }) => {
			// form -> form element
			// data -> form data
			// action -> url form posts to
			// cancel() -> cancels form
			return ({ result, update }) => {
				update();
				// result -> 'ActionResult'
				// update() -> runs all od the default use:enhance
			};
		}}
		action="/contact?/email"
		method="POST"
	> -->
	<!-- Action = route -> ?/ + action_name-->
	<form on:submit|preventDefault={handleForm} action="/contact?/email">
		<label>
			Name: <input type="text" required name="name" id="name" />
		</label>
		<label>
			Email: <input type="email" required name="email" id="email" />
		</label>
		<label>
			Message: <textarea required name="message" id="message" />
		</label>
		<button type="submit">Send Email</button>
	</form>
{/if}

<style>
	form {
		display: flex;
		flex-direction: column;
		padding: 20px;
		gap: 20px;
	}

	label {
		display: block;
	}
</style>
