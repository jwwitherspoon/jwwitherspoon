<script>
    const context = new AudioContext();
    const masterVolume = new GainNode(context, {
        gain: 0.3,
    });
    const activeOscillators = new Map();
    const keycodeFrequency = new Map([
        ['KeyA', 261.63],
        ['KeyS', 293.66],
        ['KeyD', 329.63],
        ['KeyF', 349.23],
        ['KeyG', 392.00],
        ['KeyH', 440.00],
        ['KeyJ', 493.88],
        ['KeyK', 523.25],
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
            type: 'sine',
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

<svelte:window on:keydown={play} on:keyup={stop} />
<h1 class="text-center">Musical Typing</h1>
<label>
	<input bind:group={monophonic} type="radio" name="phony" value={true} /> Mono
</label>
<label>
	<input bind:group={monophonic} type="radio" name="phony" value={false} /> Poly
</label>

<style>

</style>
