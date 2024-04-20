<h1><a href="/speakers">/speakers</a></h1>

<!-- <script>
    import { onMount } from 'svelte';

    let talks = [];

    onMount(async () => {
        try {
            const response = await fetch('https://cssday.nl/data.json');
            const data = await response.json();
            talks = Object.values(data).flatMap(event => event.talks.map(talk => ({
                title: talk.title,
                speaker: talk.speaker[0].name,
                link: talk.speaker[0].link,
                avatar: talk.speaker[0].avatar,
                country: talk.speaker[0].country,
                slides: talk.slides,
                description: talk.description,
                youtubeLink: talk.video['youtube-link'],
                thumbnail: talk.video.thumbnail,
                vimeoId: talk.video['vimeo-id'], 
                views: talk.video.views,
                likes: talk.video.likes
            })));
        } catch (error) {
            console.error('Error fetching talks data:', error);
        }
    });
</script>

<style>
    iframe {
        display: none;
    }

    img:hover + iframe {
        display: block;
    }
</style>

<ul>
    {#each talks as talk}
        <li>
            <img src="{talk.thumbnail}" alt="{talk.title}">
            <iframe src={"https://player.vimeo.com/video/" + talk.vimeoId} title="{talk.title}" width="720" height="405" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
            <h2>{talk.title}</h2>
            <p>Speaker: {talk.speaker}</p>
            <p>Country: {talk.country}</p>
            <p>Views: {talk.views}</p>
            <p>Likes: {talk.likes}</p>
            <p>Description: {talk.description}</p>
            {#if talk.slides}
                <p>Slides: <a href="{talk.slides}" target="_blank">View Slides</a></p>
            {/if}
            {#if talk.youtubeLink}
                <p>YouTube: <a href="{talk.youtubeLink}" target="_blank">Watch on YouTube</a></p>
            {/if}
        </li>
    {/each}
</ul> -->
