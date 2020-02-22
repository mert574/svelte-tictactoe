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
        <h2 class="notify">{`Game ended. ${lastWinner} ${lastWinner !== "DRAW" ? "won!" : ""}`}</h2>
    {:else}
        <p class="notify">Turn: {playerTurn ? "PLAYER" : "OPPONENT"}</p>
    {/if}
</main>

<style>
    main {
        width: 100%;
        max-width: 360px;
        height: 100%;
        margin: 0 auto;
        text-align: center;
        display: flex;
        flex-direction: column;
        box-sizing: content-box;
    }

    .board {
        display: grid;
        height: 100%;
        max-height: 360px;
        margin: auto 0;
        grid-template-rows: repeat(3, 1fr);
        grid-template-columns: repeat(3, 1fr);
        box-sizing: content-box;
        user-select: none;
        cursor: pointer;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
        -moz-tap-highlight-color: rgba(0, 0, 0, 0);
    }

    .cell {
        border: 1px solid #ff3e00;
        transition: background-color 200ms ease;
        display: flex;
        align-items: center;
        justify-content: center;
        text-transform: uppercase;
        font-weight: 900;
        font-size: 32px;
        font-family: fantasy;
    }

    .cell:hover {
        background-color: #f9dbbb;
    }

    .cell.highlighted {
        background-color: #f9c3b9;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
        margin: 0;
    }

    @media screen and (max-width: 480px){
        h1 {
            font-size: 3em;
        }
    }

    h2 {
        color: #ff004e;
        text-transform: uppercase;
        font-size: 1rem;
        font-weight: 700;
        background-color: beige;
        width: 100%;
        padding: 0.5em 1em;
        margin: 0;
    }

    p {
        font-size: 1rem;
        font-family: sans-serif;
        text-align: center;
    }

    .notify {
        position: absolute;
        right: 0;
        bottom: 20px;
        left: 0;
    }
</style>