<script>
    import MusicalTyping from "./musical-typing/MusicalTyping.svelte";
    import BeefGenerator from "./BeefGenerator.svelte";
    import Home from "./Home.svelte";
    import JazzPractice from "./JazzPractice.svelte";
    import Router from 'svelte-spa-router';
    import SudokuSolver from "./SudokuSolver.svelte";
    import UltimateTicTacToe from "./UltimateTicTacToe.svelte";

    const routes = {
        '/': Home,
        '/beef-generator': BeefGenerator,
        '/jazz-practice': JazzPractice,
        '/musical-typing': MusicalTyping,
        '/sudoku-solver': SudokuSolver,
        '/ultimate-tic-tac-toe': UltimateTicTacToe,
    };
</script>

<div id="container">
    <Router {routes}/>
</div>

<style scoped>
    #container {
        text-align: center;
        width: 500px;
        margin: 20px;
        background-color: #f7f7f7;
        opacity: 0.8;
        padding: 20px;
        border-radius: 5px;
    }

    @media only screen and (max-width: 600px) {
        #container {
            width: 280px;
        }
    }
</style>
