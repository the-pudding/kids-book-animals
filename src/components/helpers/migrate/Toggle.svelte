<script>
	let {
		label,
		style = "inner",
		options = [
			{ text: "On", value: "on" },
			{ text: "Off", value: "off" }
		],
		value = $bindable()
	} = $props();

	let checked = $derived(value === options[0].value);

	const id = `toggle-${Math.floor(Math.random() * 1000000)}`;

	const handleClick = (event) => {
		const target = event.target;
		const state = target.getAttribute("aria-checked");
		checked = state === "true" ? false : true;
		value = checked ? options[0].value : options[1].value;
	};
</script>

<div class="toggle toggle--{style}">
	<span class="label" {id}>{label}</span>
	<button
		role="switch"
		aria-checked={checked}
		aria-labelledby={id}
		onclick={handleClick}
	>
		{#if style === "inner"}
			<span>{options[0].text}</span>
			<span>{options[1].text}</span>
		{/if}
	</button>
</div>

<style>
	.toggle button,
	.label {
		font-family: inherit;
		font-size: 1em;
	}

	.toggle--inner [role="switch"][aria-checked="true"] :first-child,
	[role="switch"][aria-checked="false"] :last-child {
		display: inline-block;
		border-radius: 4px;
		background: var(--color-gray-900);
		color: var(--color-gray-100);
	}

	.toggle--inner button {
		padding: 0.5em;
		background-color: var(--color-white);
		border: 4px solid var(--color-gray-900);
	}

	.toggle--inner button span {
		user-select: none;
		pointer-events: none;
		display: inline-block;
		line-height: 1;
		padding: 0.25em;
	}

	.toggle--inner button:focus {
		box-shadow: 0 0 4px 0 var(--color-focus);
	}

	.toggle--slider {
		display: flex;
		align-items: center;
	}

	.toggle--slider button {
		width: 3.5em;
		height: 2em;
		position: relative;
		margin-left: 0.5em;
		background: var(--color-gray-300);
	}

	.toggle--slider button:focus {
		box-shadow: 0 0px 4px var(--color-focus);
	}

	.toggle--slider button::before {
		content: "";
		position: absolute;
		width: 1.5em;
		height: 1.5em;
		background: var(--color-white);
		border-radius: 4px;
		top: 0.25em;
		right: 1.75em;
	}

	.toggle--slider button[aria-checked="true"] {
		background-color: var(--color-gray-900);
	}

	.toggle--slider button[aria-checked="true"]::before {
		transform: translateX(1.5em);
	}

	.toggle--slider button:focus {
		box-shadow: 0 0 4px 0 var(--color-focus);
	}
</style>
