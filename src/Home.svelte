<script>
    import {link} from 'svelte-spa-router';
</script>

<h1 class="text-center">Projects</h1>
<div>
    <a href="/musical-typing" use:link class="btn btn-primary">Musical Typing</a>
</div>
<div>
    <a href="/beef-generator" use:link class="btn btn-primary">Beef Generator</a>
</div>
<div>
    <a href="/jazz-practice" use:link class="btn btn-primary">Jazz Practice Tools</a>
</div>
<div>
    <a href="/sudoku-solver" use:link class="btn btn-primary">Sudoku Solver</a>
</div>
<div>
    <a href="/ultimate-tic-tac-toe" use:link class="btn btn-primary">Ultimate Tic-Tac-Toe</a>
</div>

<style scoped>
    .btn {
        margin: 10px;
    }
</style>
