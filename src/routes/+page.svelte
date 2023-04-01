<script>
	import { slides } from './slider.js';
	import { fly } from 'svelte/transition';
	import { expoInOut } from 'svelte/easing';
	import { onMount } from 'svelte';
	import { count, zindex } from './store.js';

	let showScreen = false;
	let sliders = [];
	let counter = 1;
	let duration = 1200;
	let easing = expoInOut;
	let slideImg = [];
	let pillar = [];

	onMount(() => {
		showScreen = true;
		slideSlider(false, 0);
	});

	function slideSlider(value, fresh) {
		if (value) {
			$count  >= 4 ? count.set(1) : count.update(n => n + 1);
		} else {
			if (fresh == 0) {
				count.set(1);
			} else {
				$count  <= 1 ? count.set(4) : count.update(n => n - 1);
			}
		}
		zindex.update(n => n + 1);
	}
	function startOut() {
		document.querySelectorAll(".btn")[0].disabled = true;
		document.querySelectorAll(".btn")[1].disabled = true;
	}
	function endOut() {
		document.querySelectorAll(".btn")[0].disabled = false;
		document.querySelectorAll(".btn")[1].disabled = false;
		zindex.set(1);
	}

	function moveObjects(e) {
		let mouseX = e.clientX;
		slideImg[$count - 1].style.transform = `translateX(${-mouseX/80}px)`;
		pillar[$count - 1].style.transform = `translateX(${-mouseX/80}px)`;
	}
</script>

<svelte:head>
	<title>Slider</title>
	<meta name="description" content="Svelte demo app" />
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
</svelte:head>

<section>
	<div class="slider">
		{#each slides as slide, i}
			{#if $count == (i + 1)}
				<div bind:this={sliders[i]} class="slide slide-{i+1} anim8" style="z-index: {$zindex}" on:mousemove={moveObjects}>
					<div in:fly={{ x: "-100vw", opacity: 1, duration: duration, easing: easing }} out:fly={{ x: "100vw", opacity: 1, duration: duration , easing: easing }} class="slide-text">
						<div class="heading">
							<h1>{slide.title}</h1>
							<h1 class="heading-nr">0{i+1}</h1>
						</div>
						<h2>Collection</h2>
						<p>{slide.info}</p>
						<h3 class="cta">Explore</h3>
					</div>
					<img bind:this={slideImg[i]} in:fly={{ y: "100vh", opacity: 1, duration: duration, easing: easing }} out:fly={{ x: "100vw", opacity: 1, duration: duration, easing: easing }} class="pillar pillar-{i + 1}" src="/img/{slide.pillar}" alt="{slide.title}">
					<img bind:this={pillar[i]} in:fly={{ y: "-100vh", opacity: 1, duration: duration, easing: easing }} out:fly={{ x: "100vw", opacity: 1, duration: duration, easing: easing }} on:outrostart="{startOut}" on:outroend="{endOut}" class="slide-img slide-img-{i + 1}" src="/img/{slide.img}" alt="{slide.title}">
					<div in:fly={{ x: "100vw", opacity: 1, duration: duration, easing: easing }} class="bkg background-{i+1}" />
				</div>
			{/if}
		{/each}
	</div>

	<div class="toggle" in:fly={{y: 300, easing, duration }}>
		<div class="toggle-container">
			<button class="btn" on:click={() => slideSlider(false, 1)}>
				<i class="fa fa-angle-left"></i>
			</button>
			<button class="btn" on:click={() => slideSlider(true, 1)}>
				<i class="fa fa-angle-right"></i>
			</button>
		</div>
	</div>
</section>

<style>
	.show {
		display: flex !important;
	}
	.hide {
		display: none !important;
	}
	.anim8 {
		grid-row: 1;
    	grid-column: 1;
	}
	section {
		height: 100vh;
		width: 100vw;
		overflow: hidden;
		position: relative;
		color: #fff;
		-webkit-font-smoothing: antialiased!important;
	}
	.slider {
		display: grid;
    	place-items: center;
	}
	.slide {
		height: 100vh;
		width: 100vw;
		display: grid;
		align-items: center;
		position: relative;
	}
	.bkg {
		width: 100vw;
		height: 100vh;
		position: absolute;
		top: 0;
		left: 0;
		z-index: -1;
	}
	.background-1 {
		background: linear-gradient(150deg, rgba(73,94,94,1) 0%, rgba(192,163,138,1) 100%);
	}
	.background-2 {
		background: linear-gradient(150deg, rgba(123,108,104,1) 0%, rgba(170,136,101,1) 100%);
	}
	.background-3 {
		background: linear-gradient(150deg, rgba(56,87,114,1) 0%, rgba(178,148,129,1) 100%);
	}
	.background-4 {
		background: linear-gradient(150deg, rgba(56,87,114,1) 0%, rgba(178,148,129,1) 100%);
	}
	.slide-text {
		width: 100%;
		max-width: 1240px;
		transform: translateY(-30px);
		z-index: 10;
		justify-self: center;
	}
	.heading {
		padding: 0 20px;
		display: flex;
		justify-content: space-between;
		margin-bottom: 10px;
	}
	h1 {
		font-size: clamp(40px, calc(2.5rem + ((1vw - 4px) * 12.2222)), 150px);
  		min-height: 0vw;
		margin: 0;
		transition: all 200ms ease-in-out;
	}
	h2 {
		padding: 0 20px;
		text-transform: uppercase;
		margin-bottom: 50px;
		font-size: clamp(16px, calc(1rem + ((1vw - 4px) * 2.2222)), 36px);
  		min-height: 0vw;
		margin-top: 0;
		font-weight: 300;
		transition: all 200ms ease-in-out;
	}
	p {
		padding: 0 20px;
		width: 90%;
		font-size: clamp(14px, calc(0.875rem + ((1vw - 4px) * 0.7778)), 21px);
  		min-height: 0vw;
		line-height: 1.4;
		margin-bottom: 60px;
		transition: all 200ms ease-in-out;
	}
	.heading-nr {
		opacity: 0.2;
	}
	.cta {
		margin: 0 20px;
		text-transform: uppercase;
		font-size: 16px;
		letter-spacing: 1.5px;
		padding-bottom: 12px;
		border-bottom: 1px solid rgba(255, 255, 255, 0.4);
		display: inline;
		cursor: pointer;
		transition: all 400ms;
	}
	.cta:hover {
		border-bottom: 1px solid rgba(255, 255, 255, 1);
	}
	.pillar {
		width: 60vw;
		position: absolute;
		align-self: end;
		transition: all 100ms;
	}
	.pillar-1 {
		right: 18px;
	}
	.pillar-2 {
		right: 20px;
	}
	.pillar-3 {
		right: 16px;
	}
	.pillar-4 {
		right: 18px;
	}
	.slide-img {
		position: absolute;
		width: 60vw;
		transition: all 100ms;
	}
	.slide-img-1 {
		bottom: 20.5vw;
		right: 15px;
	}
	.slide-img-2 {
    	bottom: 23.5vw;
		right: 17px;
	}
	.slide-img-3 {
		bottom: 21.5vw;
		right: 13px;
	}
	.slide-img-4 {
		bottom: 21vw;
		right: 16px;
	}
	.toggle {
		max-width: 1240px;
		width: 100%;
		margin: 0 20px 50px;
		position: absolute;
    	bottom: 0;
		left: 50%;
		transform: translateX(-50%);
		z-index: 200;
	}
	.toggle-container {
		display: flex;
    	gap: 6px;
		transition: all 200ms ease-in-out;
	}
	.toggle button {
		width: clamp(40px, calc(2.5rem + ((1vw - 4px) * 2.2222)), 60px);
    	height: clamp(40px, calc(2.5rem + ((1vw - 4px) * 2.2222)), 60px);
    	border-radius: 100px;
    	border: none;
    	background: rgba(255, 255, 255, 0.3);
		cursor: pointer;
		transition: all 200ms ease-in-out;
	}
	.toggle button:hover {
		background: rgba(255, 255, 255, 0.6);
	}
	.toggle i {
		color: #fff;
	}

	@media screen and (min-width: 600px) {
		h2 {
			margin-bottom: 70px;
		}
		p {
			width: 50%;
		}
		.slide-img {
			width: 50vw;
		}
		.slide-img-1 {
			bottom: 17.5vw;
		}
		.slide-img-2 {
			bottom: 19.5vw;
		}
		.slide-img-3 {
			bottom: 17.5vw;
		}
		.slide-img-4 {
			bottom: 17.5vw;
		}
		.pillar {
			width: 50vw;
		}
		.toggle-container {
			gap: 10px;
		}
	}
	@media screen and (min-width: 1300px) {
		p {
			width: 30%;
		}
		.slide-img {
			width: 700px;
		}
		.slide-img-1, .slide-img-3 {
			right: 19vw;
			bottom: 35px;
		}
		.slide-img-2 {
			bottom: 59px;
    		right: 19.2vw;
		}
		.slide-img-4 {
			right: 19.2vw;
    		bottom: 31px;
		}
		.slide-pillar {
			height: 580px;
			bottom: -100px;
			right: 20vw;
		}
		.pillar {
			width: 700px;
			right: 19.5vw;
			bottom: -213px;
		}
	}
</style>
