<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo de Xadrez</title>
    <!-- Chosen Palette: Serene Earth Tones -->
    <!-- Application Structure Plan: A SPA com layout de duas colunas (adaptável para uma coluna em mobile). A coluna da esquerda contém os controles do jogo e mensagens, enquanto a coluna da direita exibe o tabuleiro de xadrez interativo. Esta estrutura foi escolhida para centralizar a experiência de jogo no tabuleiro, fornecendo feedback claro e conciso ao lado, facilitando a interação e o entendimento do fluxo do jogo. -->
    <!-- Visualization & Content Choices: Tabuleiro de jogo (Objetivo: Interação direta e representação clara do estado; Método: Divs HTML com CSS Grid; Interação: Células clicáveis via JS; Justificativa: Representação visual intuitiva do estado do jogo. Símbolos de peças (Objetivo: Identificação clara; Método: Caracteres Unicode; Justificativa: Leve, escalável e compatível. Mensagens de jogo (Objetivo: Informar estado, movimentos válidos/inválidos; Método: Painel de texto dedicado; Interação: Atualização dinâmica via JS; Justificativa: Feedback em tempo real. Botão de Reiniciar (Objetivo: Controlar o fluxo do jogo; Método: Botão HTML; Interação: Clique para reiniciar; Justificativa: Essencial para iniciar novas partidas. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .board-grid {
            display: grid;
            grid-template-columns: repeat(8, 3rem); /* Tamanho para telas maiores */
            grid-template-rows: repeat(8, 3rem);
            border: 4px solid #4B5563; /* Borda escura para o tabuleiro */
            border-radius: 0.5rem;
            overflow: hidden;
        }
        .board-cell {
            width: 3rem;
            height: 3rem;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2.5rem; /* Tamanho do símbolo da peça */
            cursor: pointer;
            user-select: none; /* Impede seleção de texto na peça */
            transition: background-color 0.1s ease-in-out, transform 0.1s ease-in-out;
        }
        /* Responsividade para telas menores */
        @media (max-width: 768px) {
            .board-grid {
                grid-template-columns: repeat(8, 2.5rem);
                grid-template-rows: repeat(8, 2.5rem);
            }
            .board-cell {
                width: 2.5rem;
                height: 2.5rem;
                font-size: 2rem;
            }
        }
        /* Cores das casas */
        .light-square {
            background-color: #F8D88E; /* Amarelo areia */
        }
        .dark-square {
            background-color: #B5651D; /* Marrom escuro */
        }
        /* Estilos para peças selecionadas */
        .selected {
            box-shadow: inset 0 0 0 4px #3B82F6; /* Anel azul para peça selecionada */
        }
        /* Cores dos símbolos das peças */
        .piece-white {
            color: #F9FAFB; /* Branco quase puro */
            text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
        }
        .piece-black {
            color: #1F2937; /* Cinza escuro */
            text-shadow: 1px 1px 2px rgba(255,255,255,0.3);
        }
    </style>
</head>
<body class="bg-gray-100 text-gray-900 p-4 min-h-screen flex flex-col items-center justify-center">
    <div class="container mx-auto p-6 bg-white shadow-lg rounded-lg max-w-4xl w-full flex flex-col items-center">
        <h1 class="text-4xl font-extrabold text-center mb-6 text-gray-800">Jogo de Xadrez</h1>
        <p class="text-center text-lg mb-8 text-gray-700">Mostre suas técnicas!</p>

        <div class="flex flex-col md:flex-row gap-8 w-full justify-center items-center">
            <!-- Painel de Mensagens e Controles do Jogo -->
            <div class="md:w-1/3 p-4 bg-gray-50 rounded-lg shadow-inner flex flex-col justify-between h-48 md:h-80">
                <div>
                    <h2 class="text-2xl font-semibold mb-3 text-gray-700">Estado do Jogo</h2>
                    <p id="message-display" class="text-xl font-bold text-gray-800 mb-4">Turno das Brancas</p>
                </div>
                <button id="reset-game-button" class="w-full bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-4 rounded-xl shadow-md transition duration-300 ease-in-out transform hover:scale-105">
                    Reiniciar Jogo
                </button>
            </div>

            <!-- Tabuleiro de Xadrez Interativo -->
            <div class="md:w-2/3 flex justify-center items-center p-4 bg-gray-50 rounded-lg shadow-xl">
                <div id="chess-board" class="board-grid">
                    <!-- O tabuleiro será renderizado e manipulado via JavaScript -->
                </div>
            </div>
        </div>
        <p class="text-sm mt-8 text-gray-500">Desenvolvido para Iniciante</p>
    </div>

    <script>
        const BOARD_SIZE = 8;
        const PIECE_SYMBOLS = {
            'wp': '♙', 'wr': '♖', 'wn': '♘', 'wb': '♗', 'wq': '♕', 'wk': '♔',
            'bp': '♟', 'br': '♜', 'bn': '♞', 'bb': '♝', 'bq': '♛', 'bk': '♚',
        };
        const INITIAL_BOARD = [
            ['br', 'bn', 'bb', 'bq', 'bk', 'bb', 'bn', 'br'],
            ['bp', 'bp', 'bp', 'bp', 'bp', 'bp', 'bp', 'bp'],
            [null, null, null, null, null, null, null, null],
            [null, null, null, null, null, null, null, null],
            [null, null, null, null, null, null, null, null],
            [null, null, null, null, null, null, null, null],
            ['wp', 'wp', 'wp', 'wp', 'wp', 'wp', 'wp', 'wp'],
            ['wr', 'wn', 'wb', 'wq', 'wk', 'wb', 'wn', 'wr'],
        ];

        let board = INITIAL_BOARD.map(row => [...row]);
        let selectedPiece = null;
        let turn = 'w';
        let message = 'Turno das Brancas';

        const messageDisplay = document.getElementById('message-display');
        const chessBoardDiv = document.getElementById('chess-board');
        const resetButton = document.getElementById('reset-game-button');

        function getPieceColor(piece) {
            return piece ? piece[0] : null;
        }

        function isValidPosition(r, c) {
            return r >= 0 && r < BOARD_SIZE && c >= 0 && c < BOARD_SIZE;
        }

        function canPawnMove(startRow, startCol, endRow, endCol, pieceColor, boardState) {
            const direction = pieceColor === 'w' ? -1 : 1;
            const startRank = pieceColor === 'w' ? 6 : 1;
            const targetPiece = boardState[endRow][endCol];

            if (endCol === startCol && endRow === startRow + direction && !targetPiece) {
                return true;
            }
            if (startRow === startRank && endCol === startCol && endRow === startRow + 2 * direction && !targetPiece && !boardState[startRow + direction][startCol]) {
                return true;
            }
            if (Math.abs(endCol - startCol) === 1 && endRow === startRow + direction && targetPiece && getPieceColor(targetPiece) !== pieceColor) {
                return true;
            }
            return false;
        }

        function canRookMove(startRow, startCol, endRow, endCol, boardState) {
            if (startRow === endRow) {
                const step = (endCol > startCol) ? 1 : -1;
                for (let c = startCol + step; c !== endCol; c += step) {
                    if (boardState[startRow][c]) return false;
                }
                return true;
            } else if (startCol === endCol) {
                const step = (endRow > startRow) ? 1 : -1;
                for (let r = startRow + step; r !== endRow; r += step) {
                    if (boardState[r][startCol]) return false;
                }
                return true;
            }
            return false;
        }

        function canKnightMove(startRow, startCol, endRow, endCol) {
            const dr = Math.abs(endRow - startRow);
            const dc = Math.abs(endCol - startCol);
            return (dr === 2 && dc === 1) || (dr === 1 && dc === 2);
        }

        function canBishopMove(startRow, startCol, endRow, endCol, boardState) {
            if (Math.abs(endRow - startRow) === Math.abs(endCol - startCol)) {
                const rowStep = (endRow > startRow) ? 1 : -1;
                const colStep = (endCol > startCol) ? 1 : -1;
                let r = startRow + rowStep;
                let c = startCol + colStep;
                while (r !== endRow && c !== endCol) {
                    if (boardState[r][c]) return false;
                    r += rowStep;
                    c += colStep;
                }
                return true;
            }
            return false;
        }

        function canQueenMove(startRow, startCol, endRow, endCol, boardState) {
            return canRookMove(startRow, startCol, endRow, endCol, boardState) ||
                   canBishopMove(startRow, startCol, endRow, endCol, boardState);
        }

        function canKingMove(startRow, startCol, endRow, endCol) {
            const dr = Math.abs(endRow - startRow);
            const dc = Math.abs(endCol - startCol);
            return dr <= 1 && dc <= 1;
        }

        function validateMove(startRow, startCol, endRow, endCol) {
            const piece = board[startRow][startCol];
            const pieceType = piece[1];
            const pieceColor = piece[0];
            const targetPiece = board[endRow][endCol];
            const targetColor = getPieceColor(targetPiece);

            if (startRow === endRow && startCol === endCol) return false;
            if (targetPiece && targetColor === pieceColor) return false;
            if (!isValidPosition(endRow, endCol)) return false;

            switch (pieceType) {
                case 'p': return canPawnMove(startRow, startCol, endRow, endCol, pieceColor, board);
                case 'r': return canRookMove(startRow, startCol, endRow, endCol, board);
                case 'n': return canKnightMove(startRow, startCol, endRow, endCol);
                case 'b': return canBishopMove(startRow, startCol, endRow, endCol, board);
                case 'q': return canQueenMove(startRow, startCol, endRow, endCol, board);
                case 'k': return canKingMove(startRow, startCol, endRow, endCol);
                default: return false;
            }
        }

        function updateMessageDisplay() {
            messageDisplay.textContent = message;
        }

        function renderBoard() {
            chessBoardDiv.innerHTML = ''; // Limpa o tabuleiro atual
            for (let rowIndex = 0; rowIndex < BOARD_SIZE; rowIndex++) {
                for (let colIndex = 0; colIndex < BOARD_SIZE; colIndex++) {
                    const cell = document.createElement('div');
                    cell.classList.add('board-cell');

                    const isLightSquare = (rowIndex + colIndex) % 2 === 0;
                    cell.classList.add(isLightSquare ? 'light-square' : 'dark-square');

                    if (selectedPiece && selectedPiece.row === rowIndex && selectedPiece.col === colIndex) {
                        cell.classList.add('selected');
                    }

                    const piece = board[rowIndex][colIndex];
                    if (piece) {
                        const span = document.createElement('span');
                        span.textContent = PIECE_SYMBOLS[piece];
                        span.classList.add(getPieceColor(piece) === 'w' ? 'piece-white' : 'piece-black');
                        cell.appendChild(span);
                    }

                    cell.addEventListener('click', () => handleSquareClick(rowIndex, colIndex));
                    chessBoardDiv.appendChild(cell);
                }
            }
        }

        function handleSquareClick(row, col) {
            if (selectedPiece) {
                const { row: sr, col: sc } = selectedPiece;
                const pieceToMove = board[sr][sc];

                if (validateMove(sr, sc, row, col)) {
                    const newBoard = board.map(arr => [...arr]);

                    if (newBoard[row][col]) {
                        message = `${PIECE_SYMBOLS[pieceToMove]} capturou ${PIECE_SYMBOLS[newBoard[row][col]]}!`;
                    } else {
                        message = 'Movimento válido!';
                    }

                    newBoard[row][col] = pieceToMove;
                    newBoard[sr][sc] = null;

                    board = newBoard;
                    turn = (turn === 'w' ? 'b' : 'w');
                    selectedPiece = null;
                } else {
                    message = 'Movimento inválido! Tente novamente.';
                    selectedPiece = null;
                }
            } else {
                const clickedPiece = board[row][col];
                if (clickedPiece && getPieceColor(clickedPiece) === turn) {
                    selectedPiece = { row, col };
                    message = `Peça ${PIECE_SYMBOLS[clickedPiece]} selecionada. Escolha um destino.`;
                } else if (clickedPiece && getPieceColor(clickedPiece) !== turn) {
                    message = 'Essa peça não é sua! Escolha uma peça da sua cor.';
                } else {
                    message = 'Nenhuma peça selecionada. Clique em uma peça sua para mover.';
                }
            }
            updateMessageDisplay();
            renderBoard();
            updateTurnMessage();
        }

        function resetGame() {
            board = INITIAL_BOARD.map(row => [...row]);
            selectedPiece = null;
            turn = 'w';
            message = 'Jogo Reiniciado. Turno das Brancas';
            updateMessageDisplay();
            renderBoard();
            updateTurnMessage();
        }

        function updateTurnMessage() {
            message = `Turno das ${turn === 'w' ? 'Brancas' : 'Pretas'}`;
            updateMessageDisplay();
        }

        document.addEventListener('DOMContentLoaded', () => {
            renderBoard();
            updateMessageDisplay();
            resetButton.addEventListener('click', resetGame);
        });
    </script>
</body>
</html>
