<script lang="ts">
	import Bolt from '$lib/components/icons/Bolt.svelte';
	import { onMount, getContext } from 'svelte';
	import Tooltip from '../../common/Tooltip.svelte';

	const i18n = getContext('i18n');

	export let submitPrompt: Function;
	export let suggestionPrompts = [];

	let prompts = [];

	$: prompts = suggestionPrompts
		.reduce((acc, current) => [...acc, ...[current]], [])
		.sort(() => Math.random() - 0.5);
	// suggestionPrompts.length <= 4
	// 	? suggestionPrompts
	// 	: suggestionPrompts.sort(() => Math.random() - 0.5).slice(0, 4);

	onMount(() => {
		const containerElement = document.getElementById('suggestions-container');

		if (containerElement) {
			containerElement.addEventListener('wheel', function (event) {
				if (event.deltaY !== 0) {
					// If scrolling vertically, prevent default behavior
					event.preventDefault();
					// Adjust horizontal scroll position based on vertical scroll
					containerElement.scrollLeft += event.deltaY;
				}
			});
		}
	});
</script>

{#if prompts.length > 0}
	<div class="mb-2 flex gap-1 text-sm font-medium items-center text-gray-400 dark:text-gray-600">
		<Bolt />
		{$i18n.t('Suggested')}
	</div>
{/if}

<div class="w-full relative">
	<div
		class="relative w-full flex gap-4 snap-x snap-mandatory md:snap-none overflow-x-auto tabs"
		id="suggestions-container"
	>
		{#each prompts as prompt, promptIdx}
			<div class="snap-center shrink-0">
				<button
					class="flex flex-col flex-1 shrink-0 w-64 justify-between h-36 p-5 px-6 bg-gradient-to-b from-[#f4f3fe] to-[#f6f9ff] dark:bg-gray-850 dark:hover:bg-gray-800 rounded transition group"
					on:click={() => {
						submitPrompt(prompt.content);
					}}
				>
					<div class="flex flex-col text-left">
						{#if prompt.title && prompt.title[0] !== ''}
							<div
								class="  font-medium dark:text-gray-300 dark:group-hover:text-gray-200 transition"
							>
								{prompt.title[0]}
							</div>
							<div class="text-sm text-gray-600 font-normal line-clamp-2">{prompt.title[1]}</div>
						{:else}
							<div
								class=" self-center text-sm font-medium dark:text-gray-300 dark:group-hover:text-gray-100 transition line-clamp-2"
							>
								{prompt.content}
							</div>
						{/if}
					</div>

					<div class="w-full flex justify-between">
						<div
							class="text-xs text-gray-400 group-hover:text-gray-500 dark:text-gray-600 dark:group-hover:text-gray-500 transition self-center"
						>
							{$i18n.t('Prompt')}
						</div>

						<div
							class="self-end p-1 rounded-lg text-gray-300 group-hover:text-gray-800 dark:text-gray-700 dark:group-hover:text-gray-100 transition"
						>
							<svg
								xmlns="http://www.w3.org/2000/svg"
								viewBox="0 0 16 16"
								fill="currentColor"
								class="size-4"
							>
								<path
									fill-rule="evenodd"
									d="M8 14a.75.75 0 0 1-.75-.75V4.56L4.03 7.78a.75.75 0 0 1-1.06-1.06l4.5-4.5a.75.75 0 0 1 1.06 0l4.5 4.5a.75.75 0 0 1-1.06 1.06L8.75 4.56v8.69A.75.75 0 0 1 8 14Z"
									clip-rule="evenodd"
								/>
							</svg>
						</div>
					</div>
				</button>
			</div>
		{/each}

		<!-- <div class="snap-center shrink-0">
		<img
			class="shrink-0 w-80 h-40 rounded-lg shadow-xl bg-white"
			src="https://images.unsplash.com/photo-1604999565976-8913ad2ddb7c?ixlib=rb-1.2.1&amp;ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&amp;auto=format&amp;fit=crop&amp;w=320&amp;h=160&amp;q=80"
		/>
	</div> -->
	</div>
	<div class="bg-gradient-to-r from-transparent via-white/90 pl-10 to-white/90 absolute right-0 top-0 h-full w-20 flex items-center justify-center">
		<Tooltip content=滚动鼠标中键可查看更多>
			<svg class="size-6" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg"><path d="M525.056 0a340.8 340.8 0 0 1 339.968 326.272l0.256 14.016v342.592a340.736 340.736 0 0 1-326.208 339.968l-14.08 0.256h-29.312a340.736 340.736 0 0 1-339.968-326.208l-0.256-14.08V340.352A340.736 340.736 0 0 1 481.728 0.256L495.68 0h29.376zM521.6 94.208h-22.592c-144.32 0-261.76 128.256-261.76 285.952v262.848c0 157.632 117.44 285.952 261.76 285.952h22.592c144.32 0 261.76-128.32 261.76-285.952V380.16c-0.064-157.696-117.44-285.952-261.76-285.952zM512 128a64 64 0 0 1 64 64v256a64 64 0 1 1-128 0V192a64 64 0 0 1 64-64z"></path></svg>
		</Tooltip>
	</div>
</div>

<style>
	.tabs::-webkit-scrollbar {
		display: none; /* for Chrome, Safari and Opera */
	}

	.tabs {
		-ms-overflow-style: none; /* IE and Edge */
		scrollbar-width: none; /* Firefox */
	}
</style>
