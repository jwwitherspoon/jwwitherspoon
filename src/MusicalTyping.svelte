<script>
    const context = new AudioContext();
    const masterVolume = new GainNode(context, {
        gain: 0.3,
    });
    const activeOscillators = new Map();
    const waveTypes = [
        'Sine',
        'Square',
        'Sawtooth',
        'Triangle',
    ];
    let selectedWaveType = 'sine';
    // Note reference: https://pages.mtu.edu/~suits/notefreqs.html
    const keycodeFrequency = new Map([
        ['KeyA', 261.63], // C4
        ['KeyW', 277.18], // C#4/Db4
        ['KeyS', 293.66], // D4
        ['KeyE', 311.13], // D#4/Eb4
        ['KeyD', 329.63], // E4
        ['KeyF', 349.23], // F4
        ['KeyT', 369.99], // F#4/Gb4
        ['KeyG', 392.00], // G4
        ['KeyY', 415.30], // G#4/Ab4
        ['KeyH', 440.00], // A4
        ['KeyU', 466.16], // A#4/Bb4
        ['KeyJ', 493.88], // B4
        ['KeyK', 523.25], // C5
        ['KeyO', 554.37], // C#5/Db5
        ['KeyL', 587.33], // D4
        ['KeyP', 622.25], // D#5/Eb5
        ['Semicolon', 659.25], // E5
        ['Quote', 698.46], // F5
        ['BracketRight', 739.99], // F#5/Gb5
        ['Enter', 783.99], // G5
    ]);
    let monophonic = true;

    masterVolume.connect(context.destination);

    function play(event) {
        if (event.repeat) return;
        if (keycodeFrequency.has(event.code)) {
            createOscillator(event.code);
        }
    }
1
    function stop(event) {
        if (activeOscillators.has(event.code)) {
            endOscillator(activeOscillators.get(event.code));
            activeOscillators.delete(event.code);
        }
    }

    function endOscillator(oscData) {
        oscData.gainNode.gain.linearRampToValueAtTime(0, context.currentTime + 0.02);
        setTimeout(() => {
            oscData.oscillatorNode.stop();
        }, 100);
    }

    function createOscillator(keyCode) {
        if (monophonic) {
            for (const oscData of activeOscillators.values()) {
                endOscillator(oscData);
            }
            activeOscillators.clear();
        }
        const oscillator = new OscillatorNode(context, {
            frequency: keycodeFrequency.get(keyCode),
            type: selectedWaveType,
        });
        const gain = new GainNode(context);
        oscillator.connect(gain).connect(masterVolume);
        oscillator.start();
        const oscData = {
            'oscillatorNode': oscillator,
            'gainNode': gain,
        };
        activeOscillators.set(keyCode, oscData);
    }
</script>

<svelte:window on:keydown|preventDefault={play} on:keyup={stop} />
<h1 class="text-center">Musical Typing</h1>
<label>
	<input bind:group={monophonic} type="radio" name="phony" value={true} /> Mono
</label>
<label>
	<input bind:group={monophonic} type="radio" name="phony" value={false} /> Poly
</label>
<div id="wave-type-wrapper">
    <select bind:value={selectedWaveType}>
        {#each waveTypes as waveType}
            <option value={waveType.toLowerCase()}>{waveType}</option>
        {/each}
    </select>    
</div>

<style>
    #wave-type-wrapper {
        display: flex;
        justify-content: center;
    }
</style>
