<script>
	const { src, alt, caption } = $props();
	const video = src.endsWith(".mp4");
</script>

<figure class:video>
	{#if video}
		<video autoplay loop muted playsinline controls>
			<source {src} type="video/mp4" />
		</video>
	{:else}
		<img {src} {alt} />
	{/if}
	{#if caption}
		<figcaption>{@html caption}</figcaption>
	{/if}
</figure>

<style>
	figure {
		display: block;
		width: 100%;
		margin: 2em auto;
		max-width: 20em;
	}

	figure.video {
		max-width: 15em;
	}

	img,
	video {
		max-width: 100%;
		height: auto;
		box-shadow: 0 0 12px 4px rgba(0, 0, 0, 0.2);
		border-radius: 8px;
		border: 12px solid var(--color-black);
	}

	figcaption {
		font-size: var(--14px);
		text-align: center;
		margin-top: 8px;
		line-height: 1.2;
	}
</style>
