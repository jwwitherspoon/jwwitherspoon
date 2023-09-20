<script>
    export let char = '';
    export let black = false;
    export let hidden = false;
</script>

<div id="key" class:black={black} class:hidden={hidden}>
    <span id="char">{char}</span>
</div>

<style>
    #key {
        display: flex;
        justify-content: flex-end;
        align-items: flex-end;
        width: 35px;
        height: 55px;
        border: thin solid black;
        border-radius: 5px;
        margin: 0 1px;
    }

    #char {
        padding: 5px;
        text-align: right;
    }

    .black {
        background-color: black;
        color: white;
    }

    .hidden {
        visibility: hidden;
    }
</style>
