<script>
    let firstPlayerTurn = true;
    let currentLargeCellNumber = -1;
    let currentSmallCellNumber;
    let currentCell;
    let gameWon = false;
    // Variable to keep track of whether or not each small game is won or not
    let isGameWon = [false, false, false, false, false, false, false, false, false];

    function makeMove(event) {
        const cell = event.target;
        if (!gameWon) {
            if (!isGameWon[parseInt(cell.id.slice(0, 1))]) {
                currentCell = cell;
                // If first move has not been made or current move is a free choice
                if (currentLargeCellNumber === -1) {
                    // Check if cell is empty
                    if (currentCell.innerHTML == "") {
                        if (firstPlayerTurn) {
                            currentCell.innerHTML = "X";
                            document.getElementById("message").innerHTML = "Player O's Turn";
                            firstPlayerTurn = false;
                        } else {
                            currentCell.innerHTML = "O";
                            document.getElementById("message").innerHTML = "Player X's Turn";
                            firstPlayerTurn = true;
                        }
                        checkSmallGame();
                        if (!gameWon) {
                            if (document.getElementById(currentCell.id.slice(2)).style.backgroundColor != "green" &&
                                document.getElementById(currentCell.id.slice(2)).style.backgroundColor != "red" &&
                                isGamePlayable()) {
                                currentLargeCellNumber = currentCell.id.slice(2);
                            } else {
                                currentLargeCellNumber = -1;
                            }
                        }
                    }
                }
                // If first move has been made or current move follows normal game rules
                else {
                    let smallCellLargeCoordinate = currentCell.id.slice(0, 1);
                    // Check if cell is in the right large cell
                    if (smallCellLargeCoordinate === currentLargeCellNumber) {
                        // Check if cell is empty
                        if (currentCell.innerHTML == "") {
                            if (firstPlayerTurn) {
                                currentCell.innerHTML = "X";
                                document.getElementById("message").innerHTML = "Player O's Turn";
                                firstPlayerTurn = false;
                            } else {
                                currentCell.innerHTML = "O";
                                document.getElementById("message").innerHTML = "Player X's Turn";
                                firstPlayerTurn = true;
                            }
                            document.getElementById(currentLargeCellNumber).style.backgroundColor = "initial";
                            checkSmallGame();
                            if (!gameWon) {
                                if (document.getElementById(currentCell.id.slice(2)).style.backgroundColor != "green" &&
                                    document.getElementById(currentCell.id.slice(2)).style.backgroundColor != "red" &&
                                    isGamePlayable()) {
                                    currentLargeCellNumber = currentCell.id.slice(2);
                                } else {
                                    currentLargeCellNumber = -1;
                                }
                            }
                        }
                    }
                }
                if (!gameWon) {
                    if (document.getElementById(currentCell.id.slice(2)).style.backgroundColor != "green" &&
                        document.getElementById(currentCell.id.slice(2)).style.backgroundColor != "red" &&
                        isGamePlayable()) {
                        document.getElementById(currentLargeCellNumber).style.backgroundColor = "yellow";
                    }
                }
            }
        }
    }

    function checkLargeGame() {
        let currentLargeCell = document.getElementById(currentLargeCellNumber);
        switch (currentLargeCellNumber) {
            case "0":
                // Check for a winning row containing the current cell
                if (document.getElementById("1").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("2").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("3").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("6").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning diagonal containing the current cell
                else if (document.getElementById("4").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("8").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
            case "1":
                // Check for a winning row containing the current cell
                if (document.getElementById("0").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("2").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("4").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("7").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
            case "2":
                // Check for a winning row containing the current cell
                if (document.getElementById("0").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("1").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("5").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("8").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning diagonal containing the current cell
                else if (document.getElementById("4").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("6").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
            case "3":
                // Check for a winning row containing the current cell
                if (document.getElementById("4").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("5").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("0").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("6").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
            case "4":
                // Check for a winning row containing the current cell
                if (document.getElementById("3").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("5").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("1").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("7").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for winning diagonals containing the current cell
                else if (document.getElementById("0").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("8").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                else if (document.getElementById("2").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("6").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
            case "5":
                // Check for a winning row containing the current cell
                if (document.getElementById("3").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("4").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("2").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("8").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
            case "6":
                // Check for a winning row containing the current cell
                if (document.getElementById("7").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("8").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("0").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("3").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning diagonal containing the current cell
                else if (document.getElementById("2").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("4").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
            case "7":
                // Check for a winning row containing the current cell
                if (document.getElementById("6").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("8").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("1").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("4").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
            case "8":
                // Check for a winning row containing the current cell
                if (document.getElementById("6").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("7").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById("2").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("5").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                // Check for a winning diagonal containing the current cell
                else if (document.getElementById("0").style.backgroundColor == currentLargeCell.style.backgroundColor &&
                    document.getElementById("4").style.backgroundColor == currentLargeCell.style.backgroundColor) {
                    gameWon = true;
                    postWinMessage();
                }
                break;
        }
    }

    function checkSmallGame() {
        currentSmallCellNumber = currentCell.id.slice(2);
        currentLargeCellNumber = currentCell.id.slice(0,1);
        switch (currentSmallCellNumber) {
            case "0":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-1").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-2").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-3").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-6").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning diagonal containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-4").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-8").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
            case "1":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-0").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-2").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-4").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-7").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
            case "2":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-0").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-1").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-5").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-8").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning diagonal containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-4").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-6").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
            case "3":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-4").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-5").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-0").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-6").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
            case "4":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-3").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-5").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-1").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-7").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for winning diagonals containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-0").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-8").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                else if (document.getElementById(currentLargeCellNumber + "-2").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-6").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
            case "5":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-3").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-4").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-2").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-8").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
            case "6":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-7").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-8").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-0").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-3").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning diagonal containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-2").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-4").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
            case "7":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-6").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-8").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-1").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-4").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
            case "8":
                // Check for a winning row containing the current cell
                if (document.getElementById(currentLargeCellNumber + "-6").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-7").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning column containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-2").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-5").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                // Check for a winning diagonal containing the current cell
                else if (document.getElementById(currentLargeCellNumber + "-0").innerHTML == currentCell.innerHTML &&
                    document.getElementById(currentLargeCellNumber + "-4").innerHTML == currentCell.innerHTML) {
                    fillInBackgroundColor();
                    checkLargeGame();
                }
                break;
        }
    }

    function isGamePlayable() {
        var nextGame = currentCell.id.slice(2);
        var i;
        for (i=0; i<9; i++) {
            if (document.getElementById(nextGame + "-" + i).innerHTML == "") {
                return true;
            }
        }
        return false;
    }

    function fillInBackgroundColor() {
        isGameWon[currentLargeCellNumber] = true;
        if (currentCell.innerHTML === "X") {
            document.getElementById(currentLargeCellNumber).style.backgroundColor = "green";
        }
        else if (currentCell.innerHTML === "O") {
            document.getElementById(currentLargeCellNumber).style.backgroundColor = "red";
        }
    }

    function postWinMessage() {
        if (document.getElementById(currentLargeCellNumber).style.backgroundColor === "green") {
            document.getElementById("message").innerHTML = "Player X Wins!";
        }
        else if (currentCell.innerHTML === "O") {
            document.getElementById("message").innerHTML = "Player O Wins!";
        }
    }

    function reset() {
        gameWon = false;
        for (let i=0; i<9; i++) {
            document.getElementById(i).style.backgroundColor = "initial";
            isGameWon[i] = false;
            for (let j=0; j<9; j++) {
                document.getElementById(i + "-" + j).innerHTML = "";
            }
        }
        document.getElementById("message").innerHTML = "Player X's Turn";
        currentLargeCellNumber = -1;
        firstPlayerTurn = true;
    }
</script>

<h1>Ultimate Tic-Tac-Toe</h1>
<table>
    <tbody>
        <tr>
            <td id="0" class="outer-cell top-left-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="0-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="0-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="0-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="0-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="0-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="0-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="0-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="0-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="0-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td id="1" class="outer-cell top-center-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="1-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="1-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="1-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="1-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="1-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="1-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="1-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="1-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="1-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td id="2" class="outer-cell top-right-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="2-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="2-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="2-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="2-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="2-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="2-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="2-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="2-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="2-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
        </tr>
        <tr>
            <td id="3" class="outer-cell center-left-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="3-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="3-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="3-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="3-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="3-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="3-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="3-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="3-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="3-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td id="4" class="outer-cell center-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="4-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="4-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="4-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="4-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="4-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="4-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="4-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="4-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="4-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td id="5" class="outer-cell center-right-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="5-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="5-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="5-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="5-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="5-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="5-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="5-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="5-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="5-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
        </tr>
        <tr>
            <td id="6" class="outer-cell bottom-left-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="6-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="6-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="6-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="6-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="6-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="6-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="6-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="6-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="6-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td id="7" class="outer-cell bottom-center-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="7-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="7-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="7-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="7-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="7-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="7-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="7-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="7-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="7-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
            <td id="8" class="outer-cell bottom-right-cell">
                <table>
                    <tbody>
                        <tr>
                            <td id="8-0" class="inner-cell top-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="8-1" class="inner-cell top-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="8-2" class="inner-cell top-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="8-3" class="inner-cell center-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="8-4" class="inner-cell center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="8-5" class="inner-cell center-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                        <tr>
                            <td id="8-6" class="inner-cell bottom-left-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="8-7" class="inner-cell bottom-center-cell" on:click={event => {makeMove(event)}}></td>
                            <td id="8-8" class="inner-cell bottom-right-cell" on:click={event => {makeMove(event)}}></td>
                        </tr>
                    </tbody>
                </table>
            </td>
        </tr>
    </tbody>
</table>
<h3 id="message">Player X's Turn</h3>
<div class="btn-wrapper">
    <button class="btn" on:click={reset}>Reset</button>
</div>

<style scoped>
    table {
        margin: 10px;
    }

    .top-left-cell {
        border-right-style: solid;
        border-bottom-style: solid;
    }

    .top-center-cell {
        border-right-style: solid;
        border-bottom-style: solid;
        border-left-style: solid;
    }

    .top-right-cell {
        border-bottom-style: solid;
        border-left-style: solid;
    }

    .center-left-cell {
        border-top-style: solid;
        border-right-style: solid;
        border-bottom-style: solid;
    }

    .center-cell {
        border-top-style: solid;
        border-right-style: solid;
        border-bottom-style: solid;
        border-left-style: solid;
    }

    .center-right-cell {
        border-top-style: solid;
        border-bottom-style: solid;
        border-left-style: solid;
    }

    .bottom-left-cell {
        border-top-style: solid;
        border-right-style: solid;
    }

    .bottom-center-cell {
        border-top-style: solid;
        border-right-style: solid;
        border-left-style: solid;
    }

    .bottom-right-cell {
        border-top-style: solid;
        border-left-style: solid;
    }

    .outer-cell {
        border-width: thick;
    }

    .inner-cell {
        width: 40px;
        height: 40px;
        border-width: medium;
    }

    .btn-wrapper {
        display: flex;
        justify-content: center;
    }
</style>
