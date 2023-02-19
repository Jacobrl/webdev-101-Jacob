<script>
	import { currentUser, pb } from '$lib/pocketbase';
	let caption = '';

	const formData = new FormData();
	const handleFileChange = (event) => {
		for (let file of event.target.files) {
			console.log(file);
			formData.append('photo', file);
		}
	};

	const handleCreatePost = async () => {
		formData.append('caption', caption);
		formData.append('user', $currentUser?.id);
		await pb.collection('posts').create(formData);
		caption = '';
	};
</script>

{#if $currentUser}
	<div class="flex justify-center">
		<!-- The button to open modal -->
		<label for="my-modal-4" class="btn">Create Post</label>

		<!-- Put this part before </body> tag -->
		<input type="checkbox" id="my-modal-4" class="modal-toggle" />
		<div class="modal">
			<div class="modal-box relative space-y-4">
				<label for="my-modal-4" class="btn-sm btn-circle btn absolute right-2 top-2">✕</label>
				<h3 class="text-lg font-bold">Create Post</h3>
				<div class="form-control">
					<label class="label">
						<span class="label-text">Your bio</span>
					</label>
					<textarea class="textarea-bordered textarea h-24" bind:value={caption} />
				</div>
				<input
					type="file"
					class="file-input-bordered file-input w-full max-w-xs"
					on:change={handleFileChange}
				/>
				<label for="my-modal-4" class="btn-primary btn-block btn" on:click={handleCreatePost}>
					Create Post</label
				>
			</div>
		</div>
	</div>
{/if}
