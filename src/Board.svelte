<script>
  export let boardSize
  export let player1Color
  export let player2Color
  export let points = {player1: 0, player2: 0}
  export let currentPlayer
  export let gameOver

  let boardArr = []
  let canCheckIfGameIsOver = true

  const playerColorSelect = {
    1: player1Color,
    2: player2Color
  }

  for (let i = 0; i < boardSize; i++) {
    boardArr.push([...Array(boardSize).keys()].map(() => ({ checked: false, who: 0 })))
  }

  const changeSquareValue = (x, y) => {
    console.log(x, y)
    // Check if is in border
    if (x > 0 && y > 0 && x < boardSize - 1 && y < boardSize - 1) {
      console.log("Not in border")
    }

    if (!boardArr[x][y].checked) {
      boardArr[x][y] = { checked: true, who: currentPlayer }
      changePlayerTurn()
      checkMove()
      checkIfGameIsOver()
    }
  }

  const changePlayerTurn = () => {
    if (currentPlayer === 1) {
      currentPlayer = 2
    } else {
      currentPlayer = 1
    }
  }

  const checkHorizontalTake = (i, j) => {
    // if doesn't exist an horizontal step just return false cuz it can just check for vertical pieces
    if (j - 1 === -1 || j + 1 === boardSize) {
      return false
    }

    if (boardArr[i][j].who === 1 && boardArr[i][j+1].who === 2 && boardArr[i][j-1].who === 2) {
      return true
    } else if (boardArr[i][j].who === 2 && boardArr[i][j+1].who === 1 && boardArr[i][j-1].who === 1) {
      return true
    }
  }

  const checkVerticalTake = (i, j) => {
    // if doesn't exist an vertical step just return false cuz it can just check for horizontal pieces
    if (i - 1 === -1 || i + 1 === boardSize) {
      return false
    }

    if (boardArr[i][j].who === 1 && boardArr[i+1][j].who === 2 && boardArr[i-1][j].who === 2) {
      return true
    } else if (boardArr[i][j].who === 2 && boardArr[i+1][j].who === 1 && boardArr[i-1][j].who === 1) {
      return true
    }
  }

  const checkIfGameIsOver = () => {
    if (!canCheckIfGameIsOver) {
      return
    }

    const boardValues = Object.values(boardArr)
    const singleLevelValues = boardValues.flat(1)
    const checkedFields = singleLevelValues.map(singleField => singleField.checked)

    if (!checkedFields.includes(false)) {
      gameOver = true
      canCheckIfGameIsOver = false 
    }
  }

  const checkMove = () => {
    points.player1 = 0
    points.player2 = 0

    for (let i = 0; i < boardArr.length; i++) {
      for (let j = 0; j < boardArr.length; j++) {
        if ((checkHorizontalTake(i, j) || checkVerticalTake(i, j))) {
          boardArr[i][j] = { ...boardArr[i][j], who: (currentPlayer === 1 ? 2 : 1) }
          checkIfGameIsOver()
        }

        boardArr[i][j].who === 1 && points.player1 ++ 
        boardArr[i][j].who === 2 && points.player2 ++ 
      }
    }
  }

  
</script>

<main>
  <!-- <button on:click={() => currentPlayer = 2}> Check </button> -->

  <div class="board-wrapper">
    {#each boardArr as row, i}
      <div class="single-row">
        {#each row as square, j}
          <div on:click={() => changeSquareValue(i, j, 'tomato')} style={`background: ${playerColorSelect[square.who]}`} class="single-square" />
        {/each}
      </div>
    {/each}
  </div>
</main>

<style>
  .board-wrapper {
    
  }

  .single-row {
    display: flex;
  }

  .single-square {
    width: 40px;
    height: 40px;
    margin: 4px;
    cursor: pointer;
    background: rgba(0, 0, 0, .2);
  }
</style>