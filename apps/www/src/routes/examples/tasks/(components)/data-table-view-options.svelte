<script lang="ts">
	import { MixerHorizontal } from "radix-icons-svelte";
	import { Button } from "@/registry/new-york/ui/button";
	import * as DropdownMenu from "@/registry/new-york/ui/dropdown-menu";
	import type { AnyPlugins } from "svelte-headless-table/lib/types/TablePlugin";
	import type { Task } from "../(data)/schemas";
	import type { TableViewModel } from "svelte-headless-table/lib/createViewModel";

	export let tableModel: TableViewModel<Task, AnyPlugins>;
	const { pluginStates, flatColumns } = tableModel;
	const { hiddenColumnIds } = pluginStates.hide;

	const ids = flatColumns.map((col: { id: string }) => col.id);

	let hideForId = Object.fromEntries(ids.map((id: string) => [id, true]));

	$: $hiddenColumnIds = Object.entries(hideForId)
		.filter(([, hide]) => !hide)
		.map(([id]) => id);

	const hidableCols = ["title", "status", "priority"];
</script>

<DropdownMenu.Root>
	<DropdownMenu.Trigger asChild let:builder>
		<Button
			variant="outline"
			size="sm"
			class="ml-auto hidden h-8 lg:flex"
			builders={[builder]}
		>
			<MixerHorizontal class="mr-2 h-4 w-4" />
			View
		</Button>
	</DropdownMenu.Trigger>
	<DropdownMenu.Content>
		<DropdownMenu.Label>Toggle columns</DropdownMenu.Label>
		<DropdownMenu.Separator />
		{#each flatColumns as col}
			{#if hidableCols.includes(col.id)}
				<DropdownMenu.CheckboxItem bind:checked={hideForId[col.id]}>
					{col.header}
				</DropdownMenu.CheckboxItem>
			{/if}
		{/each}
	</DropdownMenu.Content>
</DropdownMenu.Root>
