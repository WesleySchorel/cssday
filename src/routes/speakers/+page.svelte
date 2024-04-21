<script>
    import logo from '$lib/assets/logo.svg';
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

<section>
    <h1>{yearTitle};</h1>
    <img src={logo} alt="" width="277" height="173"/>
</section>

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

<footer>
	<ul>
		{#each [2013, 2014, 2015, 2016, 2017, 2018, 2019, 2022, 2023] as year}
			<li>
				<a style="color: white;" href="#" on:click={() => handleYearChange({ target: { value: year } })}>
					{year}
				</a>
			</li>
		{/each}
	</ul>
</footer>


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
		margin-left: 2rem;
		margin-top: -2.5rem;
	}

	.speakers a:nth-child(even) {
		transform: rotate(2deg);
	}

	.speakers a:nth-child(odd) {
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

	img.video:hover {
        border-width: 0;
		filter: brightness(80%);
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

	p {
		font-size: 1rem;
		color: #4d4d4d;
		font-weight: 400;
		margin-right: 0.5rem;
	}


    footer {
        background-color: #333333;
        color: #f7f7f7;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 10rem;
        width: 100%;
    }

    footer ul {
        display: flex;
        flex-direction: row;
        gap: 2rem;
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
