<script>
    import Key from "./Key.svelte";
    import { Synth, PolySynth, Gain, now } from 'tone';

	const gainNode = new Gain(0.3).toDestination();
    let synth = new Synth({
        envelope: {
            attack: 0.005,
            decay: 0,
            sustain: 1,
            release: 0.02,
        },
    }).connect(gainNode);
    // get rid of scheduling delay for better response
    synth.context.lookAhead = 0;

    // Change the synth type each time the mono/poly type changes
    $: if (monophonic) {
        synth = new Synth({
            envelope: {
                attack: 0.005,
                decay: 0,
                sustain: 1,
                release: 0.02,
            },
            oscillator: {
                type: selectedWaveType,
            },
        }).connect(gainNode);
        synth.context.lookAhead = 0;
    } else {
        synth = new PolySynth().connect(gainNode);
        synth.set({
            envelope: {
                attack: 0.005,
                decay: 0,
                sustain: 1,
                release: 0.02,
            },
            oscillator: {
                type: selectedWaveType,
            },
        });
        synth.context.lookAhead = 0;
    }

    const waveTypes = [
        'Sine',
        'Square',
        'Sawtooth',
        'Triangle',
    ];

    let selectedWaveType = 'sine';
    let monophonic = true;

    const keycodeNote = new Map([
        ['KeyA', 'C4'],
        ['KeyW', 'Db4'],
        ['KeyS', 'D4'],
        ['KeyE', 'Eb4'],
        ['KeyD', 'E4'],
        ['KeyF', 'F4'],
        ['KeyT', 'Gb4'],
        ['KeyG', 'G4'],
        ['KeyY', 'Ab4'],
        ['KeyH', 'A4'],
        ['KeyU', 'Bb4'],
        ['KeyJ', 'B4'],
        ['KeyK', 'C5'],
        ['KeyO', 'Db5'],
        ['KeyL', 'D5'],
        ['KeyP', 'Eb5'],
        ['Semicolon', 'E5'],
        ['Quote', 'F5'],
        ['BracketRight', 'Gb5'],
        ['Enter', 'G5'],
    ]);

    let isSynthPlaying = false;

    // Change the type of oscillator each time a new wave type is selected
    $: updateWaveType(selectedWaveType);

    // Play logic shared between mono and poly
    function play(event) {
        if (event.repeat) return;
        if (keycodeNote.has(event.code)) {
            if (monophonic) {
                monoPlay(keycodeNote.get(event.code));
            } else {
                polyPlay(keycodeNote.get(event.code));
            }
        }
    }

    // Mono-specific play logic
    function monoPlay(note) {
        if (isSynthPlaying) {
            synth.setNote(note);
        } else {
            synth.triggerAttack(note);
            isSynthPlaying = true;
        }
    }

    // Poly-specific play logic
    function polyPlay(note) {
        synth.triggerAttack(note);
    }

    // Stop logic shared between mono and poly
    function stop(event) {
        // If the key is the one associated with the current note, stop the note
        if (monophonic) {
            monoStop(keycodeNote.get(event.code));
        } else {
            polyStop(keycodeNote.get(event.code));
        }
    }

    // Mono-specific stop logic
    function monoStop(note) {
        if (synth.toFrequency(note) === synth.frequency.getValueAtTime(now())) {
            synth.triggerRelease();
            isSynthPlaying = false;
        }
    }

    // Poly-specific stop logic
    function polyStop(note) {
        synth.triggerRelease(note);
    }

    function updateWaveType(waveType) {
        if (monophonic) {
            synth.oscillator.type = waveType;
        } else {
            synth.set({
                oscillator: {
                    type: waveType
                }
            });
        }
    }
</script>

<svelte:window on:keydown|preventDefault={play} on:keyup={stop} />
<h1 class="text-center">Musical Typing</h1>
<div id="keyboard">
    <div class="octave">
        <div class="key-row">
            <Key char="W" black />
            <Key char="E" black />
            <Key hidden />
            <Key char="T" black />
            <Key char="Y" black />
            <Key char="U" black />
        </div>
        <div class="key-row">
            <Key char="A" />
            <Key char="S" />
            <Key char="D" />
            <Key char="F" />
            <Key char="G" />
            <Key char="H" />
            <Key char="J" />
        </div>
    </div>
    <div class="octave">
        <div class="key-row">
            <Key char="O" black />
            <Key char="P" black />
            <Key hidden />
            <Key char="]" black />
        </div>
        <div class="key-row">
            <Key char="K" />
            <Key char="L" />
            <Key char=";" />
            <Key char="'" />
            <Key char="↵" />
        </div>    
    </div>
</div>
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
    #keyboard {
        display: flex;
        justify-content: center;
        margin: 30px 0;
    }

    .octave {
        margin: 0 8px;
    }

    .key-row {
        display: flex;
        justify-content: center;
    }

    #wave-type-wrapper {
        display: flex;
        justify-content: center;
    }
</style>
