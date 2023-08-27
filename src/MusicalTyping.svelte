<script>
    const context = new AudioContext();
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

    function play(event) {
        if (event.repeat) return;
        if (keycodeFrequency.has(event.code)) {
            createOscillator(event.code);
        }
    }

    function stop(event) {
        if (activeOscillators.has(event.code)) {
            activeOscillators.get(event.code).stop();
            activeOscillators.delete(event.code);
        }
    }

    function createOscillator(keyCode) {
        if (monophonic) {
            for (const osc of activeOscillators.values()) {
                osc.stop();
            }
            activeOscillators.clear();
        }
        const oscillator = new OscillatorNode(context, {
            frequency: keycodeFrequency.get(keyCode),
            type: 'sine',
        });
        oscillator.connect(context.destination);
        oscillator.start();
        activeOscillators.set(keyCode, oscillator);
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
