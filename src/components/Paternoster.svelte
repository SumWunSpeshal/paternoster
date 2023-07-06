<script>
	import { browser } from '$app/environment';

	/**
	 * @type {HTMLDivElement}
	 */
	let component;

	/**
	 * @type {HTMLDivElement}
	 */
	let paternosterComponent;

	/**
	 * @type {boolean}
	 */
	let simpleSticky = false;

	/**
	 * @type {boolean | null}
	 */
	let scrollDown = null;

	let prevScrollPos = 0;

	let translateY = 0;

	$: roundedTranslateY = Math.floor(translateY);

	$: isOutOfParentBoundaries = () => {
		const { top, bottom } = paternosterComponent.getBoundingClientRect();
		return scrollDown ? bottom - window.innerHeight < 0 : top > 0;
	};

	$: isInView = (/** @type {number} */ top, /** @type {number} */ bottom) => {
		return scrollDown ? bottom - window.innerHeight > 0 : top < 0;
	};

	if (browser) {
		prevScrollPos = window?.scrollY ?? 0;
	}

	/**
	 * @param {number} top
	 * @param {number} bottom
	 * @param {number} height
	 */
	function contentAwareStickyStrategy(top, bottom, height) {
		scrollDown = window.scrollY > prevScrollPos;
		prevScrollPos = window.scrollY;

		if (isOutOfParentBoundaries()) {
			return;
		}

		if (isInView(top, bottom)) {
			return;
		}

		if (height > window.innerHeight) {
			pin(scrollDown ? bottom - window.innerHeight : top);
		}
	}

	/**
	 * @type {import('svelte/elements').UIEventHandler<Window>}
	 */
	function determineStickyStrategy() {
		const { top, bottom, height } = component.getBoundingClientRect();

		if (height < window.innerHeight) {
			simpleSticky = true;
			return;
		}

		contentAwareStickyStrategy(top, bottom, height);
	}

	/**
	 * @param {number} offset
	 */
	function pin(offset) {
		translateY = translateY - offset;
	}
</script>

<svelte:window on:scroll={determineStickyStrategy} />

<div class="paternoster" bind:this={paternosterComponent}>
	<div>
		<div
			class="sidebar"
			bind:this={component}
			style:transform={`translateY(${roundedTranslateY}px)`}
			style:position={simpleSticky ? 'sticky' : ''}
			style:top={simpleSticky ? '0' : ''}
		>
			<slot name="sidebar" />
		</div>
	</div>
	<div>
		<div class="content">
			<slot name="content" />
		</div>
	</div>
</div>

<style>
	.paternoster {
		max-width: 960px;
		margin-inline: auto;
		margin-block: 4rem;

		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(0, 1fr));
		gap: 2rem;
	}

	.content,
	.sidebar {
	}

	.sidebar {
		border: 1px solid red;
	}
</style>
