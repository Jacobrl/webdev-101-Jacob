<script>
	import { pb } from '$lib/pocketbase';
	import { onMount } from 'svelte';

	let posts = [];

	onMount(async () => {
		const resultList = await pb.collection('posts').getList(1, 50, {
			sort: '-created',
			expand: 'user'
		});
		posts = resultList.items;
	});

	$: console.log('posts', posts);
</script>

{#each posts as posts (posts.id)}
	<div>
		<p>{posts.expand.user.username}</p>
		<img src={pb.getFileUrl(posts, posts.photo, { thumb: '100x250' })} />
		<p>{posts.caption}</p>
		<p>{posts.created}</p>
	</div>
{/each}
