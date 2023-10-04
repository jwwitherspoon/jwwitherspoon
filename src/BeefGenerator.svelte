<script>
    import { onMount } from 'svelte';

    let canvas;
    let imageUrl = null;

    onMount(() => {
        const ctx = canvas.getContext('2d');
        const beef = new Image();
        beef.src = '/beef.png';
        beef.onload = () => {
            ctx.drawImage(beef, 0, 0);
        };
    })

    function beefify() {
        // Load canvas context
        const ctx = canvas.getContext('2d');

        // Load and draw beef
        const beef = new Image();
        beef.src = '/beef.png';
        beef.onload = () => {
            // Load and draw overlay when beef is drawn
            ctx.drawImage(beef, 0, 0);
            const overlay = new Image();
            overlay.src = imageUrl ? imageUrl : '/beef.png';
            overlay.onload = () => {
                ctx.drawImage(overlay, 32, 32, 32, 32);
            };
        };
    }
</script>

<h1>Beef Generator</h1>
<div id="input-wrapper">
    <input type="text" bind:value={imageUrl} placeholder="Image Link to Beefify">
    <button type="button" class="btn" on:click="{beefify}">Beefify</button>	
</div>
<p>
    Right-click the emoji and click "Save As" to download it. This is to bypass CORS, so use at your own risk!
</p>
<div>
    <canvas bind:this={canvas} width="64" height="64">Generated Emoji</canvas>
</div>

<style scoped>
    p {
        margin-top: 20px;
        margin-bottom: 20px;
    }

    #input-wrapper {
        display: flex;
        justify-content: center;
        column-gap: 10px;
        margin-top: 2rem;
    }
</style>
