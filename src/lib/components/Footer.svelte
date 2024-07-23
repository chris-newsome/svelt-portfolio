<script lang="ts">
	import { type Content, isFilled } from '@prismicio/client';
	import { PrismicLink } from '@prismicio/svelte';
	import IconGithub from '~icons/fa-brands/github';
	import IconLinkedIn from '~icons/fa-brands/linkedin';
	import Bounded from './Bounded.svelte';

	export let settings: Content.SettingsDocument;
</script>

<Bounded class="text-slate-600">
	<div
		class="container mx-auto flex flex-col items-center justify-between gap-5 py-12 sm:flex-row sm:py-8 sm:pb-12"
	>
		<!-- Name Section -->
		<div
			class="name flex flex-col items-center justify-center gap-x-4 gap-y-2 sm:flex-row sm:justify-self-start"
		>
			<a
				href="/"
				class="text-xl font-extrabold tracking-tighter text-slate-950 transition-colors duration-150 hover:text-yellow-400"
			>
				{settings.data.name}
			</a>
			<span
				class="hidden text-5xl font-extralight leading-none text-slate-400 sm:inline"
				aria-hidden="true">/</span
			>
			<p class="text-sm text-slate-950">
				© {new Date().getFullYear()}
				{settings.data.name}
			</p>
		</div>

		<!-- Navigation Section -->
		<div class="navigation" aria-label="Footer Navigation">
			<ul class="flex items-center gap-1">
				{#each settings.data.nav_item as { link, label }, index}
					<li>
						<PrismicLink
							field={link}
							class="block px-3 py-1 text-base font-bold text-slate-950 transition-colors duration-150 hover:text-yellow-400"
						>
							{label}
						</PrismicLink>
					</li>
					{#if index < settings.data.nav_item.length - 1}
						<span class="text-4xl font-thin leading-none text-slate-400" aria-hidden="true">/</span>
					{/if}
				{/each}
			</ul>
		</div>

		<!-- Socials Section -->
		<div class="socials inline-flex justify-center text-slate-950 sm:justify-end">
			{#if isFilled.link(settings.data.gitbuh_link)}
				<PrismicLink
					field={settings.data.gitbuh_link}
					class="p-2 text-2xl transform transition-all duration-150 hover:scale-125 hover:text-yellow-400"
					aria-label={settings.data.name + ' on Github'}
				>
					<IconGithub />
				</PrismicLink>
			{/if}
			{#if isFilled.link(settings.data.linkedin_link)}
				<PrismicLink
					field={settings.data.linkedin_link}
					class="p-2 text-2xl transform transition-all duration-150 hover:scale-125 hover:text-yellow-400"
					aria-label={settings.data.name + ' on LinkedIn'}
				>
					<IconLinkedIn />
				</PrismicLink>
			{/if}
		</div>
	</div>
</Bounded>
