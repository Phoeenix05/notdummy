<script lang="ts">
	import type { Output } from 'valibot'
	import { object, parse, picklist, string } from 'valibot'

	const fieldTypes = ['text', 'uuid', 'name', 'id']

	const FieldSchema = object({
		name: string(),
		type: picklist(fieldTypes)
	})
	type FieldSchemaT = Output<typeof FieldSchema>

	let fields: { id: number; data: FieldSchemaT }[] = []
	$: console.log(fields)

	let fieldName = ''
	let fieldType = fieldTypes[0]

	const addField = () => {
		if (fieldName == '') {
			return
		}

		const data: FieldSchemaT = {
			name: fieldName,
			type: fieldType
		}

		fields = [...fields, { id: fields.length, data: parse(FieldSchema, data) }]
	}
</script>

<div>
	<select bind:value={fieldType} name="type" id="ftype">
		{#each fieldTypes as value}
			<option {value}>{value}</option>
		{/each}
	</select>

	<input bind:value={fieldName} type="text" />
</div>

<button
	on:click={() => {
		addField()
	}}>add</button
>

{#each fields as field}
	<div id={field.id.toString()}>{field.data.name}, {field.data.type}</div>
{/each}
