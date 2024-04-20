<script>
    import logo from '$lib/assets/logo.svg';
	import { onMount } from 'svelte';

	let talks = [];
	let yearTitle = '';

	function getRandomYear() {
		return Math.floor(Math.random() * (2023 - 2013 + 1)) + 2013;
	}

	function roundViews(views) {
		if (views >= 1000) {
			return (views / 1000).toFixed(1) + 'k';
		}
		return views;
	}

	onMount(async () => {
		try {
			let randomYear;
			do {
				randomYear = getRandomYear();
			} while (randomYear === 2020 || randomYear === 2021);

			const response = await fetch(`https://cssday.nl/data.json`);
			const data = await response.json();
			const eventData = data[randomYear.toString()];
			if (eventData) {
				const color = eventData.color.hex;
				document.documentElement.style.setProperty('--c-border', color);
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
				yearTitle = eventData.title; // Hier halen we de titel van het jaar op
			} else {
				console.error(`No data available for year ${randomYear}`);
			}
		} catch (error) {
			console.error('Error fetching talks data:', error);
		}
	});
</script>

<svelte:head>
	<title>{yearTitle}</title>
	<meta name="description" content="Video collection of all speakers who attended {yearTitle}." />
</svelte:head>

<section>
	<h1>{yearTitle};</h1>
    <img src={logo} alt=""/>
</section>

<div class="speakers">
	{#each talks as talk}
		<img src={talk.avatar} alt={talk.speaker} />
	{/each}
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
						    <span>{talk.speaker}</span>
						    <span>{talk.views} 👁️</span>
						    <span>{talk.likes} 👍</span>
					    </div>
				    </div>
                </a>
			</li>
		{/each}
	</ul>
</div>

<style>
section {
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
    background-color: var(--c-border);
    height: 11.5rem;
    margin-bottom: -5rem;
    display: flex;
    justify-content: center;
    overflow-x: clip;
}

.speakers img {
    border-radius: 8px;
    height: 6rem;
    margin-left: 2rem;
    margin-top: -2.5rem;
}

.speakers img:nth-child(even) {
    transform: rotate(2deg);
}

.speakers img:nth-child(odd) {
    transform: rotate(-1deg);
}

div.content {
    display: flex;
    justify-content: center;
    margin-bottom: 3rem;
}

a {
    text-decoration: none;
}

a:focus {
  outline: none;
  border: 2px solid var(--c-border);
}

ul {
    display: grid;
    gap: 4rem;
    column-gap: 2rem;
}

li {
    list-style: none;
    display: flex;
    flex-direction: column;
    width: 320px;
}

.video-wrapper {
    position: relative;
}

img.video {
    height: auto;
    width: 320px;
    border-radius: 6px;
    border-right: 8px solid var(--c-border);
    border-bottom: 8px solid var(--c-border);
    transition: 0.3s;
}

img.video:hover {
    filter: brightness(80%)
}

.info-wrapper {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    margin-top: 0.5rem;
}

h2 {
    font-size: 18px;
    color: #202020;
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

span {
    font-size: 1rem;
    color: #4d4d4d;
    font-weight: 400;
    margin-right: 0.5rem;
}

@media (min-width: 868px) {
    ul {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (min-width: 1230px) {
    ul {
        grid-template-columns: repeat(3, 1fr);
        column-gap: 3rem;
    }
}

</style>
