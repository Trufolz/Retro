<script>
	import Section from './Section.svelte';
	import Toolbar from './Toolbar.svelte';
	import { onMount } from 'svelte';
	import { slide } from 'svelte/transition';

	let sections = [];
	let defaultSections = [
		{
			id: 1,
			name: 'Good'
		},
		{
			id: 2,
			name: 'Bad'
		},
		{
			id: 3,
			name: 'Action'
		}
	];
	let items = [];

	onMount(async () => {
		if (sections.length === 0) {
			getSections();
		}
		if (items.length === 0) {
			getItems();
		}
	});

	const getItems = () => {
		let localStorageItems = [];
		for (let key in localStorage) {
			const data = localStorage.getItem(key);
			if (data && key.startsWith('item')) {
				localStorageItems.push(JSON.parse(data));
			}
		}
		items = localStorageItems;
	}

	const getSections = () => {
		let localStorageSections = [];
		for (let key in localStorage) {
			const data = localStorage.getItem(key);
			if (data && key.startsWith('section')) {
				localStorageSections.push(JSON.parse(data));
			}
		}
		sections = localStorageSections;

		if (sections.length === 0) {
			defaultSections.forEach(s => addSection(s.id, s.name));
		}
	}

	function addItemHandler(event) {
		const newItem = event.detail;
		newItem.id = items.length + 1;
		items = [...items, newItem];
		localStorage.setItem(`item${newItem.id}`, JSON.stringify(newItem));
	}

	function removeItemHandler(event) {
		const { id } = event.detail;
		items = items.filter(i => i.id !== id);
		localStorage.removeItem(`item${id}`);
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
		result.push(`Retro ${getToday()}\n`);
		sections.forEach(section => {
			const sectionItems = items.filter(i => i.sectionId === section.id);
			if (sectionItems.length > 0) {
				result.push(`${section.name}`);
				sectionItems.forEach(i => result.push(`- ${i.value}`));
				result.push('\n');
			}
		});
		navigator.clipboard.writeText(result.join('\n'))
	}

	function addSectionHandler(event) {
		const name = prompt("New section name");
		if (name) {
			const id = sections.length + 1;
			addSection(id, name);
		}
	}

	function addSection(id, name) {
		const newSection = {
			id,
			name
		};
		sections = [...sections, newSection];
		localStorage.setItem(`section${newSection.id}`, JSON.stringify(newSection));
	}

	function removeSectionHandler(event) {
		const { id } = event.detail;
		const sectionToRemove = sections.find(i => i.id === id);
		removeItemsInSection(sectionToRemove.id);
		sections = sections.filter(i => i.id !== id);
		localStorage.removeItem(`section${id}`);
	}

	function removeItemsInSection(sectionId) {
		const itemsToRemove = items.filter(i => i.sectionId === sectionId);
		items = items.filter(i => !itemsToRemove.map(it => it.id).includes(i.id)); // object comparing works?
		itemsToRemove.forEach(i => localStorage.removeItem(`item${i.id}`));
	}

	function getToday() {
		const today = new Date();
		return `${today.getFullYear()}-${today.getMonth() + 1}-${today.getDate()}`;
	}
</script>

<img src="retro-logo.png" alt="Retro"/>
<Toolbar 
	on:addItem={addItemHandler}
	on:clearItems={clearItemsHandler}
	on:copyItems={copyItemsHandler}
	on:addSection={addSectionHandler}
	sections={sections} />
<div class="sections" id="sections">
	{#each sections as section (section.id)}
		<div transition:slide|local>
			<Section
				items={items.filter(i => i ? i.sectionId === section.id : i)}
				on:removeItem={removeItemHandler}
				on:removeSection={removeSectionHandler}
				section={section} />
		</div>
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
		flex-wrap: wrap;
	}
	@media screen and (max-width: 600px) {
		.sections {
			display: block;
		}
	}
</style>
