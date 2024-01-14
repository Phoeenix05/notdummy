<script lang="ts">
	// section -- types
	type Exopt = { label: string; ext: string }
	type Action = { text: string; fn: () => void }
	type FieldType = { name: string; description?: string }
	type Field = { name: string; meta: FieldType }

	// section -- ui variables [ stuff that affect the ui ]
	// eg. lists, errors, notifs, and so on.
	let dummy_data: Field[] = []

	let export_option: Exopt[] = [
		{ label: 'Json', ext: 'json' },
		{ label: 'CSV', ext: 'csv' }
	]
	let action: Action[] = [
		{ text: 'export', fn: export_act },
		{ text: 'reset', fn: reset_act }
	]

	// section -- value bindings [ inputs, etc ]
	let new_field_name: string
	let new_field_type: string

	let field_type: FieldType[] = [
		{ name: 'text', description: '`lorem ipsum` placholder string' },
		{ name: 'uuid', description: '36 characters long UUID (universally unique identifier)' },
		{ name: 'id', description: 'differs from `uuid`. `uuid` is 36 characters when `id` is 8.' },
		{ name: 'number', description: 'random number [i8, -128 to 127]' }
	]

	let selected_exopt: string = export_option[0].ext
	// $: console.info(selected_exopt)

	// section -- actions [ on:click, etc ]
	function export_act(): void {
		// const file = new File([JSON.stringify({})], `notdummy.${selected_exopt}`, {
		// 	type: 'application/json'
		// })
	}

	function reset_act(): void {
		alert("not sure how I'm gonna make this work")
	}

	function add_dummy_data(): void {
		dummy_data = [...dummy_data, { name: new_field_name, meta: { name: new_field_type } }]
	}
</script>

{#each dummy_data as dd}
	<input type="text" bind:value={dd.name} />
	<select bind:value={dd.meta.name}>
		{#each field_type as ft}
			<option value={ft.name}>{ft.name}</option>
		{/each}
	</select>
{/each}

<br /><br />
<!--  -->

<input type="text" bind:value={new_field_name} />

<select bind:value={new_field_type}>
	{#each field_type as ft}
		<option value={ft.name}>{ft.name}</option>
	{/each}
</select>

<button on:click={add_dummy_data}>add</button>

<!--  -->

<h3>Export Options</h3>
{#each export_option as opt}
	<input
		type="radio"
		id={`export_opt__${opt.ext}`}
		name="exopt"
		value={opt.ext}
		bind:group={selected_exopt}
	/>
	<label for={`export_opt__${opt.ext}`}>{opt.label}</label>
	<br />
{/each}

<!--  -->
<br />

{#each action as act}
	<button id="action" on:click={() => act.fn()}>{act.text}</button>
{/each}

<!--  -->
<br /><br />

{#each field_type as ft}
	<div>
		<h4>{ft.name}</h4>
		<p>{ft.description}</p>
	</div>
{/each}

<style lang="less">
	button#action {
		margin: 0 0.15em;
	}
</style>
