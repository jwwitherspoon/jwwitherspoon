<script>
    import {link} from 'svelte-spa-router';
</script>

<div id="grid-container">
    <a class="btn" href="/musical-typing" use:link>Musical Typing</a>
    <a class="btn" href="/beef-generator" use:link>Beef Generator</a>
    <a class="btn" href="/jazz-practice" use:link>Jazz Practice Tools</a>
    <a class="btn" href="/sudoku-solver" use:link>Sudoku Solver</a>
    <a class="btn" href="/ultimate-tic-tac-toe" use:link>Ultimate Tic-Tac-Toe</a>
</div>

<style scoped>
    #grid-container {
        display: grid;
        padding: 20px;
        grid-template-columns: 1fr 1fr;
        gap: 30px;
    }

    @media only screen and (max-width: 600px) {
        #grid-container {
            grid-template-columns: 1fr;
        }
    }

    a {
        font-size: 2rem;
    }

    @media only screen and (max-width: 600px) {
        a {
            font-size: 1.5rem;
        }
    }
</style>
