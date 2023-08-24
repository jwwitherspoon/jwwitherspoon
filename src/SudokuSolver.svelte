<script>
    class Cell {
        constructor(row, column, zone) {
            this.row = row;
            this.column = column;
            this.zone = zone;
            this.value = 0;
            this.solved = false;
            this.tempSolved = false;
        }
    }

    function createField() {
        var field = [
            [new Cell(0,0,0), new Cell(0,1,0), new Cell(0,2,0), new Cell(0,3,1), new Cell(0,4,1), new Cell(0,5,1), new Cell(0,6,2), new Cell(0,7,2), new Cell(0,8,2)],
            [new Cell(1,0,0), new Cell(1,1,0), new Cell(1,2,0), new Cell(1,3,1), new Cell(1,4,1), new Cell(1,5,1), new Cell(1,6,2), new Cell(1,7,2), new Cell(1,8,2)],
            [new Cell(2,0,0), new Cell(2,1,0), new Cell(2,2,0), new Cell(2,3,1), new Cell(2,4,1), new Cell(2,5,1), new Cell(2,6,2), new Cell(2,7,2), new Cell(2,8,2)],
            [new Cell(3,0,3), new Cell(3,1,3), new Cell(3,2,3), new Cell(3,3,4), new Cell(3,4,4), new Cell(3,5,4), new Cell(3,6,5), new Cell(3,7,5), new Cell(3,8,5)],
            [new Cell(4,0,3), new Cell(4,1,3), new Cell(4,2,3), new Cell(4,3,4), new Cell(4,4,4), new Cell(4,5,4), new Cell(4,6,5), new Cell(4,7,5), new Cell(4,8,5)],
            [new Cell(5,0,3), new Cell(5,1,3), new Cell(5,2,3), new Cell(5,3,4), new Cell(5,4,4), new Cell(5,5,4), new Cell(5,6,5), new Cell(5,7,5), new Cell(5,8,5)],
            [new Cell(6,0,6), new Cell(6,1,6), new Cell(6,2,6), new Cell(6,3,7), new Cell(6,4,7), new Cell(6,5,7), new Cell(6,6,8), new Cell(6,7,8), new Cell(6,8,8)],
            [new Cell(7,0,6), new Cell(7,1,6), new Cell(7,2,6), new Cell(7,3,7), new Cell(7,4,7), new Cell(7,5,7), new Cell(7,6,8), new Cell(7,7,8), new Cell(7,8,8)],
            [new Cell(8,0,6), new Cell(8,1,6), new Cell(8,2,6), new Cell(8,3,7), new Cell(8,4,7), new Cell(8,5,7), new Cell(8,6,8), new Cell(8,7,8), new Cell(8,8,8)]
        ];
        return field;
    }

    function createZone(field) {
        var zone = [
            [field[0][0], field[0][1], field[0][2], field[1][0], field[1][1], field[1][2], field[2][0], field[2][1], field[2][2]],
            [field[0][3], field[0][4], field[0][5], field[1][3], field[1][4], field[1][5], field[2][3], field[2][4], field[2][5]],
            [field[0][6], field[0][7], field[0][8], field[1][6], field[1][7], field[1][8], field[2][6], field[2][7], field[2][8]],
            [field[3][0], field[3][1], field[3][2], field[4][0], field[4][1], field[4][2], field[5][0], field[5][1], field[5][2]],
            [field[3][3], field[3][4], field[3][5], field[4][3], field[4][4], field[4][5], field[5][3], field[5][4], field[5][5]],
            [field[3][6], field[3][7], field[3][8], field[4][6], field[4][7], field[4][8], field[5][6], field[5][7], field[5][8]],
            [field[6][0], field[6][1], field[6][2], field[7][0], field[7][1], field[7][2], field[8][0], field[8][1], field[8][2]],
            [field[6][3], field[6][4], field[6][5], field[7][3], field[7][4], field[7][5], field[8][3], field[8][4], field[8][5]],
            [field[6][6], field[6][7], field[6][8], field[7][6], field[7][7], field[7][8], field[8][6], field[8][7], field[8][8]]
        ];
        return zone;
    }

    function createEnumeration(field) {
        var i, j;
        var enumeration = [];
        for (i=0; i<9; i++) {
            for (j=0; j<9; j++) {
                if (field[i][j].solved == false) {
                    enumeration.push(field[i][j]);
                }
            }
        }
        return enumeration;
    }

    //Returns a string version of the puzzle that should then be set as the innerHTML of an HTML element
    function puzzleToText(field) {
        var i, j;
        var text = "";
        for (i=0; i<9; i++) {
            for (j=0; j<9; j++) {
                text += field[i][j].value + " ";
            }
            text += "<br>";
        }
        return text;
    }

    function solve() {
        var field = createField();
        var zone = createZone(field);
        
        //Test puzzle functionality
        if (document.getElementById("0-0").value == 't' || document.getElementById("0-0").value == 'T') {
            createPuzzle();
            return 0;
        }

        //Reset message board
        document.getElementById("message").innerHTML = "";

        //Add values to cells
        for (var i=0; i<9; i++) {
            for (var j=0; j<9; j++) {
                //Get value of each cell
                var temp = document.getElementById(i + "-" + j).value;

                //If cell input is correct, fill cell with number.
                if (temp.trim() == "") {
                    field[i][j].value = 0;
                }
                else if (temp > 0 && temp < 10) {
                    field[i][j].value = parseInt(temp);
                    field[i][j].solved = true;
                }
                else {
                    document.getElementById("message").innerHTML = "Please enter only values 1-9 in cells.";
                    return 0;
                }
            }
        }

        //Create enumeration of cells in one long line so as to scroll through them better in the solution algorithm
        var enumeration = createEnumeration(field);

        //Solve puzzle using simple solving method
        //Number used to denote which index of enumeration should be used as the current cell
        var currentnum = 0;
        //While the puzzle is not solved and has not been proven impossible, run the solution algorithm
        var puzzleSolved = false, impossible = false;
        while (!puzzleSolved && !impossible) {
            //Determine the current cell
            var current = enumeration[currentnum];
            //Create two vars, one to store the current value and one to store the possible new value (starting with the next untested value)
            var prev = current.value, next = current.value + 1;
            //If the current cell is not tempsolved, try adding numbers into the cell until the entry does not violate the Sudoku condition or all values have been tested
            while (!current.tempSolved && next <= 9 && current.value == prev) {
                if (checkCell(current, next, field, zone)) {
                    current.value = next;
                    current.tempSolved = true;
                }
                next++;
            }
            //If the value does not violate the Sudoku condition
            if (current.tempSolved) {
                //If current cell is last cell, puzzle is solved
                if (currentnum == enumeration.length - 1) {
                    puzzleSolved = true;
                }
                //If current cell is not last cell, continue solution with next cell as new current cell
                else {
                    currentnum++;
                }
            }
            //If all values violate the Sudoku condition
            else {
                //If current cell is first cell, puzzle has no solution
                if (currentnum == 0) {
                    impossible = true;
                }
                //If current cell is not first cell, erase value of current cell and continue solution with previous unsolved cell as new current cell
                else {
                    current.value = 0;
                    currentnum--;
                    enumeration[currentnum].tempSolved = false;
                }
            }
        }

        //Print puzzle
        if (puzzleSolved) {
            for (i=0; i<9; i++) {
                for (j=0; j<9; j++) {
                    //Put number in cell
                    document.getElementById(i + "-" + j).value = field[i][j].value;
                    //Change new numbers to green
                    if (field[i][j].tempSolved) {
                        document.getElementById(i + "-" + j).style.color = "lightgreen";
                    }
                }
            }
        }
        else {
            document.getElementById("message").innerHTML = "This puzzle has no solution.";
        }
    }

    //Function to create a puzzle in the grid for test purposes
    function createPuzzle() {
        document.getElementById("0-0").value = 5; document.getElementById("0-1").value = 3; document.getElementById("0-5").value = 4; document.getElementById("0-7").value = 1;
        document.getElementById("1-1").value = 8; document.getElementById("1-2").value = 6; document.getElementById("1-3").value = 7; document.getElementById("1-7").value = 5;
        document.getElementById("2-1").value = 1; document.getElementById("2-3").value = 3; document.getElementById("2-4").value = 5; document.getElementById("2-6").value = 9; document.getElementById("2-7").value = 8;
        document.getElementById("3-0").value = 8; document.getElementById("3-4").value = 9; document.getElementById("3-6").value = 1; document.getElementById("3-7").value = 4; document.getElementById("3-8").value = 3;
        document.getElementById("5-0").value = 4; document.getElementById("5-1").value = 5; document.getElementById("5-2").value = 1; document.getElementById("5-4").value = 3; document.getElementById("5-8").value = 7;
        document.getElementById("6-1").value = 7; document.getElementById("6-2").value = 8; document.getElementById("6-4").value = 2; document.getElementById("6-5").value = 5; document.getElementById("6-7").value = 3;
        document.getElementById("7-1").value = 4; document.getElementById("7-5").value = 1; document.getElementById("7-6").value = 8; document.getElementById("7-7").value = 6;
        document.getElementById("8-1").value = 6; document.getElementById("8-3").value = 4; document.getElementById("8-7").value = 7; document.getElementById("8-8").value = 1;
    }

    function checkCell(cell, value, field, zone) {
        //Create variables to represent the row and column the cell will be checked against
        var r = cell.row, c = cell.column, z = cell.zone, i = 0;

        //Check value against values of the other row members
        for (i=0; i<9; i++) {
            if ((field[r][i].solved || field[r][i].tempSolved) && field[r][i].value==value)
                return false;
        }

        //Check value against values of the other column members
        for (i=0; i<9; i++) {
            if ((field[i][c].solved || field[i][c].tempSolved) && field[i][c].value==value)
                return false;
        }

        //Check value against values of the other zone members
        for (i=0; i<9; i++) {
            if ((zone[z][i].solved || zone[z][i].tempSolved) && zone[z][i].value==value)
                return false;
        }

        //If the cell's value is not equal to any of the values of the solved cells in the same row, column, and zone, return true
        return true;
    }

    function reset() {
        for (let i=0; i<9; i++) {
            for (let j=0; j<9; j++) {
                document.getElementById(i + "-" + j).value = "";
                document.getElementById(i + "-" + j).style.color = "black";
            }
        }
        document.getElementById("message").innerHTML = "";
    }
</script>

<div class="row">
    <div class="col text-center">
        <h1>Sudoku Solver</h1>
    </div>
</div>
<div id="grid-container">
    <div id="grid">
        {#each [0, 1, 2, 3, 4, 5, 6, 7, 8] as i}
            {#each [0, 1, 2, 3, 4, 5, 6, 7, 8] as j}
                <input type="text" id={i + '-' + j} class="text-center" size="1" maxlength="1">
            {/each}
        {/each}
    </div>
</div>
<div class="row justify-content-center">
    <button type="button" class="btn btn-primary" on:click={solve}>Solve</button>
    <button type="button" class="btn btn-primary" on:click={reset}>Reset</button>
</div>
<div class="row">
    <div class="col text-center">
        <h3 id="message"></h3>
    </div>
</div>

<style scoped>
    #grid-container {
        margin-top: 20px;
        margin-bottom: 20px;
        display: flex;
        justify-content: center;
    }

    #grid {
        display: grid;
        width: min-content;
        grid: repeat(9, 1fr) / repeat(9, 1fr);
        border: 1px solid black;
    }

    input[type="text"] {
        display: table;
        background-color: initial;
        border: 1px solid black;
        -webkit-appearance: none;
    }

    input:focus {
        outline: none;
    }

    .btn {
        margin: 10px;
    }
</style>
