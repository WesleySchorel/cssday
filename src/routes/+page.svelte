<script>
	import Header from '$lib/components/header.svelte';

	import Reload from '$lib/assets/reload.svg';
	import YouTubeProfile from '$lib/assets/youtubeprofile.jpg';
	import YouTube from '$lib/assets/youtube.svg';

	import { onMount } from 'svelte';

	let talks = [];
	let yearTitle = '';
	let selectedYear = '';

	function getRandomYear() {
		let randomYear;
		do {
			randomYear = Math.floor(Math.random() * (2023 - 2013 + 1)) + 2013;
		} while (randomYear === 2020 || randomYear === 2021);
		return randomYear;
	}

	function roundViews(views) {
		if (views >= 1000) {
			return (views / 1000).toFixed(1) + 'k';
		}
		return views;
	}

	async function fetchTalks(year) {
		try {
			const response = await fetch(`https://cssday.nl/data.json`);
			const data = await response.json();
			const eventData = data[year.toString()];
			if (eventData) {
				const color = eventData.color.hex;
				document.documentElement.style.setProperty('--c-year', color);
				talks = eventData.talks.map((talk) => ({
					title: talk.title,
					speaker: talk.speaker[0].name,
					link: talk.speaker[0].link,
					avatar: talk.speaker[0].avatar,
					youtubeLink: talk.video['youtube-link'],
					thumbnail: talk.video.thumbnail,
					vimeoId: talk.video['vimeo-id'],
					views: roundViews(talk.video.views),
					likes: talk.video.likes,
					color: color
				}));
				yearTitle = eventData.title;
			} else {
				console.error(`No data available for year ${year}`);
			}
		} catch (error) {
			console.error('Error fetching talks data:', error);
		}
	}

	onMount(async () => {
		selectedYear = getRandomYear();
		await fetchTalks(selectedYear);
	});

	function handleYearChange(event) {
		selectedYear = event.target.value;
		fetchTalks(selectedYear);
	}
</script>

<svelte:head>
	<title>{yearTitle}</title>
	<meta name="description" content="Video collection of all speakers who attended CSS Day." />
</svelte:head>

<nav>
	<a href="/" onclick="location.reload();">
		<img class="socials" src={Reload} alt="Reload page" />
	</a>
</nav>

<header>
	<h1>{yearTitle};</h1>
	<Header />
</header>

<div class="speakers">
	<div class="banner">
		<div class="banner-content">
			{#each talks as talk}
				<a href={talk.link} target="_blank">
					<img src={talk.avatar} alt={talk.speaker} />
				</a>
			{/each}
		</div>
		<div class="banner-content">
			{#each talks as talk}
				<a href={talk.link} target="_blank">
					<img src={talk.avatar} alt={talk.speaker} />
				</a>
			{/each}
		</div>
		<div class="banner-content">
			{#each talks as talk}
				<a href={talk.link} target="_blank">
					<img src={talk.avatar} alt={talk.speaker} />
				</a>
			{/each}
		</div>
	</div>
</div>

<div class="content">
	<ul>
		{#each talks as talk}
			<li>
				<a href={talk.youtubeLink} target="_blank">
					<div class="video-wrapper">
						<img class="video" src={talk.thumbnail} alt={talk.title} />
					</div>

					<div class="info-wrapper">
						<div class="title-info-wrapper">
							<img class="avatar" src={talk.avatar} alt={talk.speaker} />
							<h2>{talk.title}</h2>
						</div>
						<div class="info">
							<p>{talk.speaker}</p>
							<p>{talk.views} 👁️</p>
							<p>{talk.likes} 👍</p>
						</div>
					</div>
				</a>
			</li>
		{/each}
	</ul>
</div>

<div class="sub-footer">
	<img
		src={YouTubeProfile}
		alt=""
	/>
	<p>
		We curate and organise deep-diving, single-track, content-focused web conferences, in Amsterdam.
		Most sessions, about 45 minutes each, include a Q&A session with the speaker. We aim to cover
		techniques that can stand the test of time, and we tend to stay away from hypes.
	</p>
	<a href="https://www.youtube.com/@WebConferencesAmsterdam" target="_blank">
		<img class="socials" src={YouTube} alt="Youtube channel link" />
	</a>
</div>

<footer id="selectyear">
	<h3>Select year</h3>
	<ul>
		{#each [2013, 2014, 2015, 2016, 2017, 2018, 2019, 2022, 2023] as year}
			<li>
				<a
					style="color: white;"
					href="/"
					on:click={() => handleYearChange({ target: { value: year } })}
				>
					{year}
				</a>
			</li>
		{/each}
	</ul>
</footer>

<style>
	:root {
		--c-text: #202020;
	}

	nav img {
		margin: 1rem;
		height: 1.75rem;
		width: 1.75rem;
		transition: 0.6s;
	}

	header {
		height: 34rem;
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	h1 {
		font-size: 1.8rem;
		margin-top: 6rem;
		margin-bottom: 3rem;
		text-wrap: balance;
		text-align: center;
	}

	.speakers {
		background-color: var(--c-year);
		height: 11.5rem;
		margin-bottom: -5rem;
		display: flex;
		justify-content: center;
		overflow-x: clip;
	}

	.speakers img {
		border-radius: 8px;
		height: 6rem;
		width: 6rem;
		margin-top: -2.5rem;
	}

	.speakers a:nth-child(even) {
		margin-top: -1rem;
		height: 8rem;
		width: 8rem;
		transform: rotate(2deg);
	}

	.speakers a:nth-child(odd) {
		height: 8rem;
		width: 8rem;
		transform: rotate(-2deg);
	}

	.banner {
		position: relative;
		width: 230%;
		flex-shrink: 0;
		display: flex;
		gap: 0.5rem;
		z-index: 1;
		font-size: 3rem;
		left: 60vw;
	}

	.banner-content {
		position: relative;
		display: flex;
		flex-direction: row;
		flex-shrink: 0;
		animation: scroll 10s linear infinite;
	}

	@keyframes scroll {
		from {
			transform: translateX(0);
		}
		to {
			transform: translateX(calc(-100% - 0.5rem));
		}
	}

	div.content {
		display: flex;
		justify-content: center;
		margin-bottom: 3rem;
	}

	a {
		text-decoration: none;
	}

	.content ul {
		display: grid;
		gap: 2rem;
		column-gap: 3rem;
	}

	.content li {
		list-style: none;
		display: flex;
		flex-direction: column;
		width: 320px;
		z-index: 1;
	}

	.video-wrapper {
		position: relative;
	}

	img.video {
		width: 320px;
		height: 184px;
		border-radius: 6px;
		border-right: 8px solid var(--c-year);
		border-bottom: 8px solid var(--c-year);
		transition: 0.3s;
	}

	.info-wrapper {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
		margin-top: 0.5rem;
	}

	h2 {
		font-size: 18px;
		color: var(--c-text);
		font-weight: 500;
		margin-bottom: 0.75rem;
		width: 18rem;
		text-wrap: balance;
	}

	.title-info-wrapper {
		display: flex;
		align-items: center;
	}

	img.avatar {
		width: 30px;
		height: 30px;
		border-radius: 50%;
		margin-right: 1rem;
	}

	div.info {
		margin-top: -0.5rem;
		display: flex;
		align-items: center;
		margin-left: 46px;
	}

	p {
		font-size: 1rem;
		color: #4d4d4d;
		font-weight: 400;
		margin-right: 0.5rem;
	}

	.sub-footer {
		display: flex;
		flex-direction: column;
		gap: 1rem;
		background-color: #ebebeb;
		padding: 1rem;
		margin: 0 auto 2rem auto;
		width: 320px;
		border-radius: 8px;
	}

	.sub-footer img {
		width: 100%;
		height: auto;
		border-radius: 4px;
	}

	.sub-footer .socials {
		height: 3rem;
		width: 3rem;
	}

	footer {
		background-color: #333333;
		color: #f7f7f7;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 22rem;
		width: 100%;
	}

	footer h3 {
		color: white;
		margin: 1rem;
	}

	footer ul {
		list-style: none;
		display: grid;
		grid-template-columns: repeat(3, 1fr);
		gap: 1rem;
	}

	@media (min-width: 868px) {
		nav img:hover {
			transform: rotate(360deg);
		}

		ul {
			grid-template-columns: repeat(2, 1fr);
		}

		img.video:hover {
			border-width: 0;
			filter: brightness(80%);
		}

		.sub-footer {
			flex-direction: row;
			margin: 6rem auto 4rem auto;
			width: 690px;
		}

		.sub-footer img {
			width: 100px;
			height: 100px;
		}

		.sub-footer p {
			margin-right: 4rem;
		}

		footer {
			height: 12rem;
		}

		footer a:hover {
			opacity: 0.7;
		}
	}

	@media (min-width: 1230px) {
		ul {
			grid-template-columns: repeat(3, 1fr);
			column-gap: 3rem;
		}

		.sub-footer {
			width: 1060px;
		}

		.sub-footer p {
			margin-right: 24rem;
		}
	}
</style>
