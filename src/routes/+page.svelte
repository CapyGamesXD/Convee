<script>
	//@ts-nocheck
	import { Upload } from 'lucide-svelte';
	import { onMount } from 'svelte';

	let files = $state();
	let canvas;
	let downloadURL = $state();
	let format = $state('jpeg');
	let displayImage = $state('');
	let quality = $state(10);
	async function setImage() {
		displayImage = '';
		downloadURL = '';
		if (files.length > 0) {
			let file = files[0];
			console.log('File recieved. :D');

			displayImage = URL.createObjectURL(file);
		}
	}

	function convertImage() {
		downloadURL = '';

		let file = files[0];
		let img = new Image();
		img.src = URL.createObjectURL(file);
		img.onload = () => {
			const ctx = canvas.getContext('2d');
			canvas.width = img.width;
			canvas.height = img.height;
			ctx.drawImage(img, 0, 0);
			downloadURL = canvas.toDataURL(`image/${format}`, quality / 10);
			console.log('Exported as', `image/${format}`);
			if (!downloadURL.startsWith(`data:image/${format}`)) {
				console.log('Export failed!');
				alert(
					'File type is unsupported on this browser. Please switch browsers or try another format.'
				);
				displayImage = '';
				downloadURL = '';
			}
		};
		files = null;
	}
</script>

<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" />
<link
	href="https://fonts.googleapis.com/css2?family=Nunito:ital,wght@0,200..1000;1,200..1000&display=swap"
	rel="stylesheet"
/>

<div class="centerdiv">
	<h1>Convee</h1>
	<a class="info" href="info">Info</a>
	<div class="divider"></div>
	<div class="bubble">
		<label>
			<input class="hidden" type="file" bind:files onchange={(e) => setImage(e)} />
			<Upload class="icon" />
		</label>
		<div class="divider"></div>
		{#if files && files.length >= 1}
			<img src={displayImage} />

			<select bind:value={format}>
				<option value="jpeg">JPEG</option>
				<option value="png">PNG</option>
				<option value="webp">WebP (smallest file)</option>
			</select>

			{#if format !== 'png'}
				<p>Quality: {quality}</p>
				<input class="rangeInput" type="range" min="1" max="10" bind:value={quality} />
			{/if}
			<button onclick={convertImage} class="convert">Convert!</button>
		{/if}
		{#if !displayImage}
			<p>Add an image to get started!</p>
		{/if}
		<canvas bind:this={canvas}></canvas>

		{#if downloadURL}
			<p>Original:</p>
			<img src={displayImage} />
			<p>Converted: (.{format})</p>
			<img src={downloadURL} alt />

			<a class="fancyLink" href={downloadURL} download="ConveeImage">Download Converted Image</a>
		{/if}
	</div>
</div>
