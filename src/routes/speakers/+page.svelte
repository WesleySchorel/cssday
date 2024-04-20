<script>
    import { onMount } from 'svelte';

    let talks = [];

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
            } else {
                console.error(`No data available for year ${randomYear}`);
            }
        } catch (error) {
            console.error('Error fetching talks data:', error);
        }
    });
</script>

<div class="content">
    <ul>
        {#each talks as talk}
            <li>
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
                        <span>{talk.views} views</span>
                        <span>{talk.likes} likes</span>
                    </div>
                </div>
            </li>
        {/each}
    </ul>
</div>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');

	* {
        font-family: "Roboto", sans-serif;
		box-sizing: border-box;
		margin: 0;
		padding: 0;
		scroll-behavior: smooth;
	}

	div.content {
		display: flex;
		justify-content: center;
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
        width: 340px;
	}

	.video-wrapper {
		position: relative;
	}

	img.video {
		height: auto;
		width: 340px;
		border-radius: 6px;
		border-right: 8px solid var(--c-border);
		border-bottom: 8px solid var(--c-border);
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
		white-space: nowrap;
		overflow: hidden; 
		text-overflow: ellipsis; 
	}

	.title-info-wrapper {
		display: flex;
		align-items: center;
	}

	img.avatar {
		width: 40px; 
		height: 40px; 
		border-radius: 50%;
		margin-right: 1rem;
	}

	div.info {
        margin-top: -0.5rem;
		display: flex;
		align-items: center;
        margin-left: 56px;
	}

	span {
        font-size: 1rem;
        color: #7F7A7A;
        font-weight: 400;
		margin-right: 0.5rem;
	}
</style>
