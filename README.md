import React, { useState, useEffect } from 'react';

// Constantes para o tamanho do tabuleiro e as peças
const BOARD_SIZE = 8;
const PIECE_SYMBOLS = {
    'wp': '♙', 'wr': '♖', 'wn': '♘', 'wb': '♗', 'wq': '♕', 'wk': '♔',
    'bp': '♟', 'br': '♜', 'bn': '♞', 'bb': '♝', 'bq': '♛', 'bk': '♚',
};
// Estado inicial do tabuleiro com as peças em suas posições de início
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

// Componente principal do jogo de xadrez
export default function App() {
    // Estado do tabuleiro: matriz 8x8 de strings (ex: 'wp' para peao branco) ou null para casas vazias
    const [board, setBoard] = useState(INITIAL_BOARD);
    // Estado da peça selecionada: {row, col} se uma peça foi clicada, null caso contrário
    const [selectedPiece, setSelectedPiece] = useState(null);
    // Estado do turno: 'w' para as brancas, 'b' para as pretas
    const [turn, setTurn] = useState('w');
    // Mensagens exibidas para o usuário durante o jogo
    const [message, setMessage] = useState('Turno das Brancas');

    // Função auxiliar para obter a cor de uma peça (primeiro caractere da string da peça)
    const getPieceColor = (piece) => piece ? piece[0] : null;

    // Função auxiliar para verificar se uma posição (linha, coluna) está dentro dos limites do tabuleiro
    const isValidPosition = (r, c) => r >= 0 && r < BOARD_SIZE && c >= 0 && c < BOARD_SIZE;

    /*
     * Funções de validação de movimento para cada tipo de peça.
     * Estas funções aplicam as estruturas de repetição e condições conforme o desafio do PDF.
     */

    // Valida o movimento do Peão (Pawn)
    // Aplica múltiplas condições para diferentes tipos de movimento do peão
    const canPawnMove = (startRow, startCol, endRow, endCol, pieceColor, boardState) => {
        const direction = pieceColor === 'w' ? -1 : 1; // Brancas sobem (-1), Pretas descem (1)
        const startRank = pieceColor === 'w' ? 6 : 1; // Linha inicial para o primeiro movimento do peão

        const targetPiece = boardState[endRow][endCol];

        // Movimento normal de uma casa para frente
        // Condição: move uma casa na direção correta e a casa de destino está vazia
        if (endCol === startCol && endRow === startRow + direction && !targetPiece) {
            return true;
        }

        // Primeiro movimento de duas casas para frente
        // Condições: está na linha inicial, move duas casas na direção correta,
        // a casa de destino está vazia, e a casa intermediária também está vazia
        if (startRow === startRank && endCol === startCol && endRow === startRow + 2 * direction && !targetPiece && !boardState[startRow + direction][startCol]) {
            return true;
        }

        // Captura diagonal
        // Condições: move uma casa na diagonal, há uma peça inimiga na casa de destino
        if (Math.abs(endCol - startCol) === 1 && endRow === startRow + direction && targetPiece && getPieceColor(targetPiece) !== pieceColor) {
            return true;
        }

        return false; // Movimento inválido para o peão
    };

    // Valida o movimento da Torre (Rook)
    // Aplica loops 'for' para verificar o caminho linear (horizontal ou vertical)
    const canRookMove = (startRow, startCol, endRow, endCol, boardState) => {
        if (startRow === endRow) { // Movimento horizontal
            const step = (endCol > startCol) ? 1 : -1; // Define a direção (direita ou esquerda)
            // Loop 'for' para iterar por todas as casas no caminho
            for (let c = startCol + step; c !== endCol; c += step) {
                if (boardState[startRow][c]) return false; // Caminho bloqueado por outra peça
            }
            return true; // Caminho livre
        } else if (startCol === endCol) { // Movimento vertical
            const step = (endRow > startRow) ? 1 : -1; // Define a direção (para baixo ou para cima)
            // Loop 'for' para iterar por todas as casas no caminho
            for (let r = startRow + step; r !== endRow; r += step) {
                if (boardState[r][startCol]) return false; // Caminho bloqueado por outra peça
            }
            return true; // Caminho livre
        }
        return false; // Não é um movimento horizontal ou vertical
    };

    // Valida o movimento do Cavalo (Knight)
    // Verifica diretamente as 8 posições possíveis em "L" (aplicação conceitual de loops aninhados)
    const canKnightMove = (startRow, startCol, endRow, endCol) => {
        const dr = Math.abs(endRow - startRow); // Diferença absoluta de linhas
        const dc = Math.abs(endCol - startCol); // Diferença absoluta de colunas

        // O movimento do cavalo é sempre um "L" (2 casas em uma direção e 1 na perpendicular)
        return (dr === 2 && dc === 1) || (dr === 1 && dc === 2);
    };

    // Valida o movimento do Bispo (Bishop)
    // Aplica loops 'while' para verificar o caminho diagonal
    const canBishopMove = (startRow, startCol, endRow, endCol, boardState) => {
        // Verifica se é um movimento diagonal (diferença de linha igual a diferença de coluna)
        if (Math.abs(endRow - startRow) === Math.abs(endCol - startCol)) {
            const rowStep = (endRow > startRow) ? 1 : -1; // Direção da linha (para baixo ou para cima)
            const colStep = (endCol > startCol) ? 1 : -1; // Direção da coluna (para direita ou esquerda)

            let r = startRow + rowStep;
            let c = startCol + colStep;

            // Loop 'while' para iterar por todas as casas na diagonal
            while (r !== endRow && c !== endCol) {
                if (boardState[r][c]) return false; // Caminho bloqueado por outra peça
                r += rowStep;
                c += colStep;
            }
            return true; // Caminho livre
        }
        return false; // Não é um movimento diagonal
    };

    // Valida o movimento da Rainha (Queen)
    // Combina a lógica de movimento da Torre e do Bispo
    const canQueenMove = (startRow, startCol, endRow, endCol, boardState) => {
        // A Rainha pode se mover como uma Torre OU como um Bispo
        return canRookMove(startRow, startCol, endRow, endCol, boardState) ||
               canBishopMove(startRow, startCol, endRow, endCol, boardState);
    };

    // Valida o movimento do Rei (King)
    // Aplica condições simples de distância
    const canKingMove = (startRow, startCol, endRow, endCol) => {
        const dr = Math.abs(endRow - startRow); // Diferença absoluta de linhas
        const dc = Math.abs(endCol - startCol); // Diferença absoluta de colunas

        // O Rei pode mover uma casa em qualquer direção (horizontal, vertical ou diagonal)
        return dr <= 1 && dc <= 1;
    };


    // Função principal para validar qualquer movimento de peça
    const isValidMove = (startRow, startCol, endRow, endCol) => {
        const piece = board[startRow][startCol];
        const pieceType = piece[1]; // 'p', 'r', 'n', 'b', 'q', 'k'
        const pieceColor = piece[0]; // 'w' ou 'b'
        const targetPiece = board[endRow][endCol];
        const targetColor = getPieceColor(targetPiece);

        // Condições básicas de xadrez:
        // 1. Não pode mover para a mesma casa
        if (startRow === endRow && startCol === endCol) return false;
        // 2. Não pode capturar uma peça da sua própria cor
        if (targetPiece && targetColor === pieceColor) return false;
        // 3. A posição final deve ser válida (dentro do tabuleiro)
        if (!isValidPosition(endRow, endCol)) return false;


        // Delega a validação para a função específica de cada peça
        switch (pieceType) {
            case 'p': return canPawnMove(startRow, startCol, endRow, endCol, pieceColor, board);
            case 'r': return canRookMove(startRow, startCol, endRow, endCol, board);
            case 'n': return canKnightMove(startRow, startCol, endRow, endCol);
            case 'b': return canBishopMove(startRow, startCol, endRow, endCol, board);
            case 'q': return canQueenMove(startRow, startCol, endRow, endCol, board);
            case 'k': return canKingMove(startRow, startCol, endRow, endCol);
            default: return false; // Tipo de peça desconhecido
        }
    };

    // Função para lidar com o clique em uma célula do tabuleiro
    const handleSquareClick = (row, col) => {
        // Se uma peça já foi selecionada anteriormente
        if (selectedPiece) {
            const { row: sr, col: sc } = selectedPiece; // Coordenadas da peça selecionada
            const pieceToMove = board[sr][sc]; // A peça que será movida

            // Tentar mover a peça selecionada para a nova casa clicada (destino)
            if (isValidMove(sr, sc, row, col)) {
                // Cria uma nova cópia do tabuleiro para manter a imutabilidade do estado no React
                const newBoard = board.map(arr => [...arr]);

                // Verifica se houve captura de peça
                if (newBoard[row][col]) {
                    setMessage(`${PIECE_SYMBOLS[pieceToMove]} capturou ${PIECE_SYMBOLS[newBoard[row][col]]}!`);
                } else {
                    setMessage('Movimento válido!');
                }

                newBoard[row][col] = pieceToMove; // Move a peça para a nova posição
                newBoard[sr][sc] = null; // Limpa a posição anterior da peça

                setBoard(newBoard); // Atualiza o estado do tabuleiro
                setTurn(turn === 'w' ? 'b' : 'w'); // Troca o turno (brancas para pretas, pretas para brancas)
                setSelectedPiece(null); // Desseleciona a peça após o movimento
            } else {
                // Se o movimento for inválido, exibe uma mensagem e desseleciona a peça
                setMessage('Movimento inválido! Tente novamente.');
                setSelectedPiece(null);
            }
        } else {
            // Nenhuma peça selecionada, tentar selecionar uma peça na casa clicada
            const clickedPiece = board[row][col];
            // Só permite selecionar uma peça se a casa não estiver vazia e a peça for da cor do turno atual
            if (clickedPiece && getPieceColor(clickedPiece) === turn) {
                setSelectedPiece({ row, col }); // Define a peça clicada como selecionada
                setMessage(`Peça ${PIECE_SYMBOLS[clickedPiece]} selecionada. Escolha um destino.`);
            } else if (clickedPiece && getPieceColor(clickedPiece) !== turn) {
                // Se clicou em uma peça do oponente
                setMessage('Essa peça não é sua! Escolha uma peça da sua cor.');
            } else {
                // Se clicou em uma casa vazia sem peça selecionada
                setMessage('Nenhuma peça selecionada. Clique em uma peça sua para mover.');
            }
        }
    };

    // Função para reiniciar o jogo para o estado inicial
    const resetGame = () => {
        setBoard(INITIAL_BOARD);
        setSelectedPiece(null);
        setTurn('w');
        setMessage('Jogo Reiniciado. Turno das Brancas');
    };

    // Efeito colateral para atualizar a mensagem do turno sempre que o 'turn' mudar
    useEffect(() => {
        setMessage(`Turno das ${turn === 'w' ? 'Brancas' : 'Pretas'}`);
    }, [turn]);


    return (
        <div className="bg-gray-100 text-gray-900 p-4 min-h-screen flex flex-col items-center justify-center">
            <div className="container mx-auto p-6 bg-white shadow-lg rounded-lg max-w-4xl w-full flex flex-col items-center">
                <h1 className="text-4xl font-extrabold text-center mb-6 text-indigo-800 animate-pulse">Xadrez</h1>
                <p className="text-center text-lg mb-8 text-gray-700">Mostre suas tecnicas</p>

                <div className="flex flex-col md:flex-row gap-8 w-full justify-center items-center">
                    {/* Painel de Mensagens e Controles */}
                    <div className="md:w-1/3 p-4 bg-indigo-50 rounded-lg shadow-inner flex flex-col justify-between h-48 md:h-80">
                        <div>
                            <h2 className="text-2xl font-semibold mb-3 text-indigo-700">Estado do Jogo</h2>
                            <p className="text-xl font-bold text-gray-800 mb-4">{message}</p>
                        </div>
                        <button
                            onClick={resetGame}
                            className="w-full bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-4 rounded-xl shadow-md transition duration-300 ease-in-out transform hover:scale-105"
                        >
                            Reiniciar Jogo
                        </button>
                    </div>

                    {/* Tabuleiro de Xadrez */}
                    <div className="md:w-2/3 flex justify-center items-center p-4 bg-gray-50 rounded-lg shadow-xl">
                        <div className="board-grid grid grid-cols-8 grid-rows-8 border-4 border-gray-700 rounded-lg overflow-hidden">
                            {board.map((row, rowIndex) => (
                                row.map((piece, colIndex) => {
                                    // Determina a cor da casa (clara/escura)
                                    const isLightSquare = (rowIndex + colIndex) % 2 === 0;
                                    const squareColorClass = isLightSquare ? 'bg-amber-100' : 'bg-amber-700'; // Cores amadeiradas

                                    // Adiciona estilo para a casa selecionada
                                    const isSelected = selectedPiece && selectedPiece.row === rowIndex && selectedPiece.col === colIndex;
                                    const selectedClass = isSelected ? 'ring-4 ring-blue-500 ring-offset-2' : '';

                                    return (
                                        <div
                                            key={`${rowIndex}-${colIndex}`}
                                            className={`w-10 h-10 md:w-12 md:h-12 flex justify-center items-center text-3xl md:text-4xl cursor-pointer select-none transition-all duration-100 ${squareColorClass} ${selectedClass} rounded-sm`}
                                            onClick={() => handleSquareClick(rowIndex, colIndex)}
                                        >
                                            <span className={`piece-symbol ${piece && getPieceColor(piece) === 'w' ? 'text-white drop-shadow-md' : 'text-gray-900 drop-shadow-md'}`}>
                                                {piece ? PIECE_SYMBOLS[piece] : ''}
                                            </span>
                                        </div>
                                    );
                                })
                            ))}
                        </div>
                    </div>
                </div>
                <p className="text-sm mt-8 text-gray-500">
                    Desenvolvido para Iniciante 
                </p>
            </div>
        </div>
    );
}
