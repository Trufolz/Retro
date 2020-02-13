<script>
	import Section from './Section.svelte';
	import Toolbar from './Toolbar.svelte';
	import { onMount, tick } from 'svelte';
	import { slide } from 'svelte/transition';

	let sections = ['Good', 'Bad', 'Action'];
	let items = [];

	onMount(async () => {
		if (items.length === 0) {
			items = getItemsFromLocalStorage();
		}
	});

	const getItemsFromLocalStorage = () => {
		let localStorageItems = [];
		for (let key in localStorage) {
			const item = localStorage.getItem(key);
			if (item) {
				localStorageItems.push(JSON.parse(item));
			}
		}
		return localStorageItems.sort(i => i.id);
	}

	function addItemHandler(event) {
		const newItem = event.detail;
		newItem.id = items.length + 1;
		items = [...items, newItem].sort(i => i.id);
		localStorage.setItem(newItem.id, JSON.stringify(newItem));
	}

	function removeItemHandler(event) {
		const { id } = event.detail;
		items = items.filter(i => i.id !== id);
		localStorage.removeItem(id);
	}

	function clearItemsHandler(event) {
		const isConfirmed = confirm('Do you want to clear all items?');
		if (isConfirmed) {
			items = [];
			localStorage.clear();
		}
	}

	function copyItemsHandler(event) {
		const result = [];
		result.push(`Retro ${getToday()}\n\n`);
		sections.forEach(section => {
			const sectionItems = items.filter(i => i.section === section);
			if (sectionItems.length > 0) {
				result.push(`${section}`);
				sectionItems.forEach(i => result.push(`- ${i.value}`));
				result.push('\n');
			}
		});
		navigator.clipboard.writeText(result.join('\n'))
	}

	function getToday() {
		const today = new Date();
		return `${today.getFullYear()}-${today.getMonth() + 1}-${today.getDate()}`;
	}
</script>

<img src="retro-logo.png" alt="Retro"/>
<Toolbar on:addItem={addItemHandler} on:clearItems={clearItemsHandler} on:copyItems={copyItemsHandler} sections={sections} />
<div class="sections" id="sections">
	{#each sections as section (section)}
		<Section on:removeItem={removeItemHandler} title={section} items={items.filter(i => i ? i.section === section : i)} />
	{/each}
</div>

<style>
	img {
		display: block;
		width: 130px;
		margin-left: auto;
  		margin-right: auto;
		margin-top: 20px;
		margin-bottom: 25px;
	}
	.sections {
		display: flex;
		justify-content: center;
		text-align: center;
	}
</style>
