<script lang="ts">
	import defaultImage from '$lib/images/image.png';
	import { formatAuthors } from '$lib/utils/authors';
	import { getBeerPriceDisplay } from '$lib/utils/price';

	interface Props {
		title: string;
		description: string;
		rating: number;
		image?: string;
		imageFocusX?: number;
		imageFocusY?: number;
		location: string;
		beerPriceKr?: number;
		isHappyHourPrice?: boolean;
		author?: string;
		coAuthors?: string[] | string;
	}

	let {
		title,
		description,
		rating,
		image = defaultImage,
		imageFocusX = 50,
		imageFocusY = 50,
		location,
		beerPriceKr,
		isHappyHourPrice = false,
		author,
		coAuthors
	}: Props = $props();

	const resolvedImage = $derived.by(() => {
		if (!image) return defaultImage;
		if (image.startsWith('http://') || image.startsWith('https://') || image.startsWith('/')) {
			return image;
		}
		return `/images/${image}`;
	});

	const beerPriceDisplay = $derived(getBeerPriceDisplay(beerPriceKr, isHappyHourPrice));
</script>

<div
	class="group relative aspect-square overflow-hidden rounded-3xl border border-white/90 shadow-[inset_0_1px_0_rgba(255,255,255,0.9),0_14px_30px_-26px_rgba(148,163,184,0.55)]"
>
	<div
		class="absolute inset-0 bg-cover transition duration-500 group-hover:scale-[1.03]"
		style={`background-image: url('${resolvedImage}'); background-position: ${imageFocusX ?? 50}% ${imageFocusY ?? 50}%`}
		role="img"
		aria-label={title}
	></div>

	<div
		class="absolute inset-0 bg-gradient-to-t from-black/85 via-black/35 to-transparent"
	></div>

	<div class="absolute inset-x-0 bottom-0 space-y-1.5 px-5 pb-4 pt-10">
		<div class="flex items-start justify-between gap-4">
			<h2 class="min-w-0 text-xl font-semibold text-white drop-shadow-sm">{title}</h2>
			<div
				class="inline-flex min-w-12 shrink-0 items-baseline justify-center whitespace-nowrap rounded-full border border-white/30 bg-white/15 px-2.5 py-1 text-white backdrop-blur-sm"
			>
				<span class="text-lg font-semibold leading-none">{rating}</span>
				<span class="ml-1 text-[10px] font-semibold uppercase tracking-[0.14em] text-white/70"
					>/3</span
				>
			</div>
		</div>
		<p class="text-[11px] uppercase tracking-[0.24em] text-white/75">{location}</p>
		{#if author}
			<p class="text-xs text-white/75">av {formatAuthors(author, coAuthors)}</p>
		{/if}
		<p class="line-clamp-2 text-sm leading-relaxed text-white/90">
			{description}
		</p>
		{#if beerPriceDisplay}
			<div class="flex items-baseline justify-between gap-3 border-t border-white/25 pt-2">
				<p class="text-[10px] font-semibold uppercase tracking-[0.16em] text-white/75">
					Pris för en stor stark
				</p>
				<p class="whitespace-nowrap text-base font-semibold leading-tight text-white">
					{beerPriceDisplay.text}
				</p>
			</div>
			{#if beerPriceDisplay.note}
				<p class="-mt-1 text-right text-[10px] leading-tight text-white/70">
					{beerPriceDisplay.note}
				</p>
			{/if}
		{/if}
	</div>
</div>
