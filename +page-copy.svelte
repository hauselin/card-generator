<script>
	import html2canvas from "html2canvas";
	// Define the data for the card, typically this would be passed as props or fetched from an API
	const post = {
		username: "Jewish Voice for Peace NYC",
		userHandle: "@jvplive",
		time: "1h",
		text: "🚨 HAPPENING NOW AT THE STATUE OF LIBERTY: Hundreds of Jews and allies are holding an emergency sit-in, taking over the island to demand a ceasefire in Gaza. We refuse to allow a genocide to be carried out in our names. Ceasefire now to save lives! Never again for anyone! 🚨",
		retweets: "7.5K",
		quotes: "15K",
		likes: "386K",
		imageAlt: "Protest at the Statue of Liberty",
		userImage: "path", // Placeholder path to the user's profile image
		postImage: "20231106144341.png", // Placeholder path to the image in the tweet
	};

	async function takeScreenshot() {
		const element = document.querySelector(".card");
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

<article class="card">
	<header class="card-header">
		<!-- <img src={post.userImage} alt={post.username} class="user-image" /> -->
		<div class="user-info">
			<h2 class="username">{post.username}</h2>
			<p class="user-handle">{post.userHandle} · {post.time}</p>
		</div>
	</header>
	<p class="post-text">{post.text}</p>
	<img src={post.postImage} alt={post.imageAlt} class="post-image" />
	<footer class="card-footer">
		<span>{post.retweets} Retweets</span>
		<span>{post.quotes} Quotes</span>
		<span>{post.likes} Likes</span>
	</footer>
</article>

<style>
	.card {
		max-width: 600px;
		margin: auto;
		border: 1px solid #ccc;
		border-radius: 16px;
		overflow: hidden;
		font-family: sans-serif;
	}
	.card-header {
		display: flex;
		align-items: center;
		padding: 16px;
	}
	.user-image {
		width: 50px;
		height: 50px;
		border-radius: 50%;
		margin-right: 16px;
	}
	.user-info {
		flex: 1;
	}
	.username {
		margin: 0;
		font-weight: bold;
	}
	.user-handle {
		margin: 0;
		color: #555;
	}
	.post-text {
		padding: 0 16px;
	}
	.post-image {
		width: 100%;
		display: block;
	}
	.card-footer {
		display: flex;
		justify-content: space-around;
		padding: 16px;
		background: #f8f8f8;
	}
</style>
