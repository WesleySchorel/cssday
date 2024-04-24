<script>
	import Logo from '$lib/assets/cssday-black.svg';
	import Logo1 from '$lib/assets/cssday-yellow.svg';
	import Logo2 from '$lib/assets/cssday-lime.svg';
	import Logo3 from '$lib/assets/cssday-blue.svg';
	import { onMount } from 'svelte';

	let logos;

	onMount(() => {
		logos = document.querySelectorAll('.logo');

		logos.forEach((img, index) => {
			img.style.zIndex = `${logos.length - index}`;
		});

		window.addEventListener('mousemove', handleMouseMove);

		return () => {
			window.removeEventListener('mousemove', handleMouseMove);
		};
	});

	function handleMouseMove(event) {
		let x = (window.innerWidth / 2 - event.clientX) / 40;
		let y = (window.innerHeight / 2 - event.clientY) / 40;

		logos.forEach((img, index) => {
			img.style.transform = `translate(${x * index}px, ${y * index}px)`;
		});
	}
</script>

<div id="container">
	<img src={Logo} alt="" class="logo" />
	<img src={Logo1} alt="" class="logo" />
	<img src={Logo2} alt="" class="logo" />
	<img src={Logo3} alt="" class="logo" />
</div>

<style>
	#container {
		height: 12rem;
		width: 10rem;
		aspect-ratio: 1;
		display: flex;
		align-items: center;
		justify-content: center;
		position: relative;
	}

	img {
		width: 277px;
		height: 173px;
	}

	.logo {
		position: absolute;
		transition: 50ms ease;
	}
</style>
