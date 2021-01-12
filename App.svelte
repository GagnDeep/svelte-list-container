<script>
	import List from "./List";

	let items = "a.".repeat(20).split(".");
</script>

<List {items} let:item let:isActive>
	<div>
		{item} {isActive}
	</div>
</List>

<svelte:head>
	<link href="https://unpkg.com/tailwindcss@^1.0/dist/tailwind.min.css" rel="stylesheet">
</svelte:head>