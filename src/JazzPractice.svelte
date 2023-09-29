<script>
    const keys = ['A','A#','Bb','B','C','C#','Db','D','D#','Eb','E','F','F#','Gb','G','G#'];
    const tonalities = [
        ['', 'maj', 'ma', 'M'], // Major
        ['∆', 'm', 'mi', 'min', '-'], // Minor
        ['°', 'dim'], // Fully diminished
        ['+', 'aug', '(#5)'], // Augmented
        ['Ø'], // Half Diminished
    ];
    const extensions = ['','6','7','9','11','13'];

    let message = 'Click a button to generate practice material!';

    function getRandomIndex(arr) {
        return Math.floor(Math.random() * arr.length);
    }

    function generateKey() {
        const key = keys[getRandomIndex(keys)];
        message = key;
    }

    function generateChord() {
        const root = keys[getRandomIndex(keys)];
        const tonalityType = tonalities[getRandomIndex(tonalities)];
        const tonality = tonalityType[getRandomIndex(tonalityType)];
        const extension = extensions[getRandomIndex(extensions)];
        message = root + tonality + extension;
    }
</script>

<h1>Jazz Practice Tools</h1>
<p>{message}</p>
<div class="button-wrapper">
    <button type="button" class="btn" on:click={generateKey}>Random Key</button>
    <button type="button" class="btn" on:click={generateChord}>Random Chord</button>
</div>

<style scoped>
    h1 {
        padding: 0 1rem;
    }

    p {
        display: block;
        margin: 50px 0;
        font-size: 1.25rem;
    }

    .btn {
        margin: 10px;
    }

    .button-wrapper {
        display: flex;
        justify-content: center;
    }
</style>
