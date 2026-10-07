<!--
SPDX-FileCopyrightText: 2023 Marlon W (Mawoka)

SPDX-License-Identifier: MPL-2.0
-->
<script lang="ts">
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	interface Props {
		languages?: Array<{
			flag: string;
			name: string;
			code: string;
		}>;
	}

	let {
		languages = [
			{
				code: 'pt_BR',
				flag: '🇧🇷',
				name: 'Português (Brasil)'
			}
		]
	}: Props = $props();
	const get_selected_language = (): string => {
		return localStorage.getItem('language') ?? 'pt_BR';
	};
	let selected_language: string = $state();
	onMount(() => {
		selected_language = get_selected_language();
	});

	const set_language = (code: string): void => {
		if (browser) {
			localStorage.setItem('language', code);
			window.location.reload();
		}
	};
</script>

<div>
	<select
		bind:value={selected_language}
		onchange={() => {
			set_language(selected_language);
		}}
		class="p-2 rounded-lg bg-gray-800 focus:ring-2 ring-blue-600 text-white"
		aria-label="Seletor de idioma"
	>
		{#each languages as lang}
			<option value={lang.code}>{lang.flag} {lang.name} </option>
		{/each}
	</select>
</div>
