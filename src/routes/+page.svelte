<script>
	import html2canvas from "html2canvas";
	import { onMount } from "svelte";
	import { tweets } from "$lib/tweets.js";

	const download = false; // set to true to download the images
	const params = {
		userinfo: null,
		text: true,
		metrics: null,
		image: true,
	};
	const imgPath = "tweets-images/"; // path in static directory
	const highlightCol = "#1D9BF0";

	function highlightTagMentionLink(text) {
		if (text.includes(highlightCol)) return text;
		// Regex to match hashtags and mentions
		// const regex = /([#@][\w]+)/g;
		const regex = /([#@][\w]+|http[^\s]+)/g;

		// Replace matched strings with HTML span element with specific color
		return text.replace(regex, '<span style="color: #1D9BF0;">$1</span>');
	}

	tweets.forEach((tweet) => {
		tweet.text = highlightTagMentionLink(tweet.text);
		if (tweet.mediaURL != "") {
			tweet.mediaURL = imgPath + tweet.tweetid + ".jpg";
		}
	});

	// tweets = tweets.slice(5);
	console.log(
		tweets.filter((tweet) => tweet.tweetid == "1572335524605566976")[0],
	);

	const examplePost = {
		username: "Jewish Voice for Peace NYC",
		userHandle: "@jvplive",
		time: "1h",
		text: "🚨 HAPPENING NOW AT THE STATUE OF LIBERTY: Hundreds of Jews and allies are holding an emergency sit-in, taking over the island to demand a ceasefire in Gaza. We refuse to allow a genocide to be carried out in our names. Ceasefire now to save lives! Never again for anyone! 🚨",
		retweets: "7.5K",
		quotes: "15K",
		likes: "386K",
		imageAlt: "",
		userImage: "path", // Placeholder path to the user's profile image
		postImage: "images/20231106144341.png", // Placeholder path to the image in the tweet
	};

	async function takeScreenshot(element, filename) {
		const canvas = await html2canvas(element, { scale: 2, userCORS: true });
		const image = canvas.toDataURL("image/jpeg", 0.8);
		downloadImage(image, filename);
	}

	const downloadImage = (dataUrl, filename) => {
		const a = document.createElement("a");
		a.href = dataUrl;
		a.download = filename;
		document.body.appendChild(a);
		a.click();
		document.body.removeChild(a);
	};

	// onMount(() => {
	// 	let elements = document.querySelectorAll(".card");
	// 	elements.forEach((element) => {
	// 		const filename = "tweet-" + element.id;
	// 		console.log(filename);
	// 		if (download) takeScreenshot(element, filename);
	// 	});
	// });

	onMount(() => {
		if (download) {
			const takeScreenshotWithDelay = async () => {
				await new Promise((resolve) => setTimeout(resolve, 1500));
				let elements = document.querySelectorAll(".card");

				for (const element of elements) {
					const filename = "tweet-" + element.id;
					console.log("downloading", filename);

					// Wait for 100 ms before proceeding to the next iteration
					await new Promise((resolve) => setTimeout(resolve, 100));
					takeScreenshot(element, filename);
				}
			};
			takeScreenshotWithDelay();
		}
	});
</script>

<!-- <button on:click={takeScreenshot}>Capture Screenshot</button> -->

{#each tweets as tweet, tweetidx}
	Card {tweetidx + 1}. tweetid:
	<a href={`https://twitter.com/user/status/${tweet.tweetid}`} target="_blank"
		>{tweet.tweetid}</a
	>
	<div class="card-container">
		<article
			class="card"
			id={String(tweetidx).padStart(3, "0") + "-" + tweet.tweetid}
		>
			<header class="card-header">
				{#if params.userinfo}
					<img
						src={tweet.userImage}
						alt={tweet.username}
						class="user-image"
					/>

					<div class="user-info">
						<h2 class="username">{tweet.username}</h2>
						<p class="user-handle">
							{tweet.userHandle} · {tweet.time}
						</p>
					</div>
				{/if}
			</header>

			{#if params.text}
				<p class="post-text">{@html tweet.text}</p>
			{/if}

			{#if params.image}
				{#if tweet.mediaURL != ""}
					<img
						src={tweet.mediaURL}
						alt={tweet.imageAlt}
						class="post-image"
					/>
				{/if}
			{/if}

			{#if params.metrics}
				<footer class="card-footer">
					<span>{tweet.retweets} Retweets</span>
					<span>{tweet.quotes} Quotes</span>
					<span>{tweet.likes} Likes</span>
				</footer>
			{/if}
		</article>
	</div>
{/each}

<style>
	.card-container {
		margin: 21px;
	}
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
		padding: 8px;
		padding-bottom: 5px;
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
		padding-top: 0px;
		margin-top: 0px;
		margin-bottom: 8px;
		font-size: 1.3rem;
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
