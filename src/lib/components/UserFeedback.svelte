<script>
	import { page } from '$app/stores';

	let state = 'closed';

	function submitFeedback(evt) {
		evt.preventDefault();
		const message = document.getElementById('feedback-message').value;
		if (message) {
			fetch('/api/post-feedback', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({ message, page: $page.path })
			});
			state = 'sent';
			setTimeout(() => {
				state = 'closed';
			}, 8000);
		}
	}
</script>

{#if state === 'closed'}
	<button
		on:click={() => (state = 'open')}
		class="text-green-700 border border-green-400 px-4 py-2 rounded hover:bg-green-100"
	>
		Une erreur ? Un oubli ? Contactez-nous !
	</button>
{:else if state === 'open'}
	<form class="flex flex-col items-start gap-y-3" on:submit={submitFeedback}>
		<label for="feedback-message">Votre message :</label>
		<!-- svelte-ignore a11y-autofocus -->
		<textarea
			class="border border-green-300 p-3 w-70 h-30 rounded"
			autofocus
			id="feedback-message"
		/>
		<div class="flex gap-x-4 uppercase">
			<button type="submit" class="bg-green-600 text-white px-4 py-2 rounded">Envoyer</button>
			<button class="text-gray-500" on:click={() => (state = 'closed')}>Fermer</button>
		</div>
		<p>
			→ Ou accédez au <a
				class="hover:underline text-green-600"
				href="https://docs.google.com/spreadsheets/d/12q5-N4tRUyBUL03zQhOjz_DMYwuRzm080vaJhTEHnRc/edit?usp=sharing"
				target="_blank">Google Sheet partagé</a
			>.
		</p>
	</form>
{:else if state === 'sent'}
	<span class="text-xl">Merci pour votre retour ! 😍</span>
{/if}
