<script>
	import html2canvas from "html2canvas";

	// You can set props for content or fetch from a store or API
	let tweet = {
		user: "Senator Shehu Sani",
		handle: "@ShehuSani",
		time: "31m",
		content:
			"In the 24 year history of Nigeria’s democracy, it’s only GEJ that has ever congratulated the winner; He is the first and hope he will not be the last.",
		stats: {
			comments: 119,
			retweets: 64,
			likes: 324,
		},
		verified: true,
	};

	async function takeScreenshot() {
		const element = document.querySelector(".tweet-card");
		if (element) {
			const canvas = await html2canvas(element, { scale: 2 });
			const image = canvas.toDataURL("image/png");
			downloadImage(image, "tweet-card.png");
		}
	}

	function downloadImage(dataUrl, filename) {
		const a = document.createElement("a");
		a.href = dataUrl;
		a.download = filename;
		document.body.appendChild(a);
		a.click();
		document.body.removeChild(a);
	}
</script>

<button on:click={takeScreenshot}>Capture Screenshot</button>

<div class="tweet-card">
	<div class="tweet-header">
		<span class="tweet-user">{tweet.user}</span>
		{#if tweet.verified}
			<span class="verified-icon">✓</span>
		{/if}
		<span class="tweet-handle">{tweet.handle}</span>
		<span class="tweet-time">{tweet.time}</span>
	</div>
	<div class="tweet-content">
		{tweet.content}
	</div>
	<!-- <div class="tweet-stats">
		<span>{tweet.stats.comments} Comments</span>
		<span>{tweet.stats.retweets} Retweets</span>
		<span>{tweet.stats.likes} Likes</span>
	</div> -->
</div>

<style>
	.tweet-card {
		border: 1px solid #e1e8ed;
		border-radius: 16px;
		padding: 20px;
		max-width: 500px;
		font-family: "Helvetica", "Arial", sans-serif;
		margin-bottom: 10px;
	}

	.tweet-header {
		display: flex;
		align-items: center;
		font-size: 0.9rem;
		color: #657786;
		margin-bottom: 10px;
	}

	.tweet-user {
		font-weight: bold;
		color: #14171a;
		margin-right: 5px;
	}

	.verified-icon {
		color: #1da1f2;
		font-size: 1rem;
	}

	.tweet-handle {
		margin-right: 5px;
	}

	.tweet-time {
		margin-left: auto;
	}

	.tweet-content {
		font-size: 1rem;
		color: #14171a;
		margin-bottom: 10px;
	}

	.tweet-stats {
		font-size: 0.9rem;
		color: #657786;
		display: flex;
		justify-content: space-between;
	}

	.tweet-stats span {
		cursor: pointer;
	}
</style>
