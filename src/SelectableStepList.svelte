<script lang="ts">
	import RecipeLeaf from "./RecipeLeaf.svelte";

	export let list: HTMLOListElement | Array<HTMLElement>;
	export let kind: string;
	export let radioName: string;

	function olChildren() {
		return (list as HTMLOListElement).children;
	}

	function olChild(index: number) {
		return olChildren().item(index)! as HTMLElement;
	}

	function pList() {
		return list as Array<HTMLElement>;
	}
</script>

{#if kind == "ol"}
	<!-- means steps is the children of an OL element -->
	<div>
		<ol class="recipe-mutex-select">
			{#each olChildren() as _, i}
				<li>
					<div class="leaf">
						<label>
							<input type="radio" name={radioName} />
							<RecipeLeaf childNodesOf={olChild(i)} asTag="span" />
						</label>
					</div>
				</li>
			{/each}
		</ol>
	</div>
{:else if kind == "p"}
	<!-- means steps is an array of P elements -->
	{#each pList() as p}
		<div class="leaf">
			<label>
				<input type="radio" name={radioName} />
				<RecipeLeaf childNodesOf={p} asTag="span" />
			</label>
		</div>
	{/each}
{/if}

<style>
	li {
		margin-block: var(--p-spacing);
	}

	input[type="radio"] {
		opacity: 0;
		position: absolute;
		height: 100%;
		width: 100%;
		margin: 0;
		padding: 0;
		z-index: -1;
	}

	label {
		position: relative;
	}

	.leaf {
		border-radius: var(--radius-m);
		padding: var(--size-4-2);
		margin: calc(-1 * var(--size-4-2));
	}

	:global(body:not(.is-mobile)) li .leaf {
		/* on desktop, include number in selected step
           -> operates weirdly on mobile, so there just leave default spacing 
		*/
		padding-inline-start: var(--list-indent);
		margin-inline-start: calc(-1 * var(--list-indent));
	}

	.leaf:has(input[type="radio"]:checked) {
		background-color: hsla(
			var(--accent-h),
			var(--accent-s),
			var(--accent-l),
			var(--selected-step-alpha)
		);
	}

	.leaf:has(input[type="radio"]:focus) {
		box-shadow: inset 0px 0px 0px var(--border-width)
			var(--interactive-accent);
	}
</style>
