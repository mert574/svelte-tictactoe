<script>
    let board = new Array(9).fill("");
    let highlightedCells = new Array(3);
    let playerTurn = true;
    let lastWinner = null;

    const winningPositions = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
    ];

    function handleCellClick(cell) {
        if (board[cell] !== "" || lastWinner !== null) {
            return;
        }

        board[cell] = playerTurn ? "X" : "O";
        checkForWinners();

        playerTurn = !playerTurn;
    }

    function checkForWinners() {
        winningPositions.forEach(([i, j, k]) => {
            if (allEqualAndNotEmpty(board[i], board[j], board[k])) {
                const winnerSymbol = board[i];
                highlightedCells = [i, j, k];
                finalizeGame(winnerSymbol);
            }
        });

        const isRoundDraw = board.every(cell => cell !== "") && lastWinner === null;
        if (isRoundDraw) {
            finalizeGame();
        }
    }

    function allEqualAndNotEmpty(...values) {
        const first = values[0];
        return values.reduce((acc, curr) => curr !== "" && acc && first === curr);
    }

    async function finalizeGame(winnerSymbol) {
        if (winnerSymbol === "X") {
            lastWinner = "PLAYER";
        } else if (winnerSymbol === "O") {
            lastWinner = "OPPONENT";
        } else {
            lastWinner = "DRAW";
        }

        setTimeout(() => {
            lastWinner = null;
            board = new Array(9).fill("");
            highlightedCells = new Array(3);
        }, 2000);

    }
</script>

<main>
    <h1>Tic Tac Toe</h1>
    <div class="board">
        {#each board as cell, i}
            <div class="cell" class:highlighted={highlightedCells.includes(i)} on:click={() => handleCellClick(i)}>{cell}</div>
        {/each}
    </div>
    {#if lastWinner}
        <h2>{`Game ended. ${lastWinner} ${lastWinner !== "DRAW" ? "won!" : ""}`}</h2>
    {:else}
        <p>Turn: {playerTurn ? "PLAYER" : "OPPONENT"}</p>
    {/if}
</main>

<style>
    main {
        max-width: 422px;
        margin: 0 auto;
        text-align: center;
    }

    .board {
        width: 420px;
        height: 420px;
        border: 1px solid crimson;
        display: flex;
        flex-wrap: wrap;
        box-sizing: content-box;
        user-select: none;
        cursor: pointer;
    }

    .cell {
        width: 140px;
        height: 140px;
        border: 1px solid #ff3e00;
        transition: background-color 200ms ease;
        display: flex;
        align-items: center;
        justify-content: center;
        text-transform: uppercase;
        font-weight: 900;
        font-size: 32px;
    }

    .cell:hover {
        background-color: #ebebeb;
    }

    .cell.highlighted {
        background-color: #eb9a8e;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    h2 {
        color: #ff004e;
        text-transform: uppercase;
        font-size: 1em;
        font-weight: 700;
        background-color: beige;
        width: 100%;
        padding: 0.5em 1em;
        margin: 0;
    }
</style>