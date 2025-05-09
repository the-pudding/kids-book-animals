<script>
	import _ from "lodash";

	let { animal, books } = $props();

	const n = 12;
	let swiperEl = $state();
	let selectedDecade = $state("All");

	let topBooks = $derived(
		books
			.filter((b) => b.decade === selectedDecade || selectedDecade === "All")
			.slice(0, n)
	);

	const formatRatings = (num) => {
		num = Number(num);
		if (num >= 1_000_000)
			return (num / 1_000_000).toFixed(1).replace(/\.0$/, "") + "m";
		if (num >= 1_000) return (num / 1_000).toFixed(1).replace(/\.0$/, "") + "k";
		return num;
	};

	const resetSwiper = () => {
		if (swiperEl && swiperEl.swiper) swiperEl.swiper.slideTo(0);
	};

	$effect(() => resetSwiper(animal));
</script>

<div class="shelf-title">Top {_.startCase(animal)} Books</div>

<figure class="books">
	<button onclick={() => swiperEl.swiper.slidePrev()}>{"<"}</button>
	<swiper-container slides-per-view="auto" bind:this={swiperEl}>
		{#each topBooks as book}
			<swiper-slide>
				<a href={book.goodreads_link} target="_blank">
					<img
						src={book.book_cover_image}
						alt={`Book cover of ${book.title}`}
					/>
				</a>
				<div class="text">
					<strong class="title">{book.title} ({book.pub_year})</strong>
					<div class="author">By {book.author}</div>
					<div class="num-ratings">
						{formatRatings(book.num_ratings)} Goodreads ratings
					</div>
				</div>
			</swiper-slide>
		{/each}
	</swiper-container>
	<button onclick={() => swiperEl.swiper.slideNext()}>{">"}</button>
</figure>

<style>
	figure {
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 1rem;
	}

	swiper-container {
		width: 100%;
		overflow: hidden;
		pointer-events: none;
	}

	swiper-slide {
		width: fit-content;
		pointer-events: auto;
		display: flex;
		flex-direction: column;
		align-items: center;
		margin: 0 0.5rem;
	}

	swiper-slide:hover {
		cursor: pointer;
	}

	.shelf-title {
		font-size: var(--18px);
		font-weight: bold;
		text-align: center;
		margin-bottom: 1rem;
	}

	a {
		height: 140px;
	}

	a:hover img {
		opacity: 0.75;
		transition: opacity 0.1s ease;
	}

	img {
		height: 100%;
	}

	.text {
		font-size: var(--14px);
		text-align: center;
		width: 120px;
	}

	.num-ratings {
		font-size: var(--12px);
		color: var(--color-gray-700);
	}

	@media (max-width: 600px) {
		.shelf-title {
			font-size: var(--16px);
		}

		a {
			height: 120px;
		}
	}
</style>
