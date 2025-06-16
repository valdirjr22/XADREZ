import React, { useState, useEffect } from 'react';

/**
 * @file Jogo de Xadrez Interativo em React
 * @brief Este arquivo implementa um jogo de xadrez básico jogável utilizando React.
 * O foco está em demonstrar a aplicação de estruturas de repetição (for, while,
 * múltiplas condições) e a organização de código para validação de movimentos
 * das peças, conforme os requisitos do desafio em PDF.
 *
 * @author [Seu Nome/MateCheck]
 * @date [Data Atual]
 */

// --- Constantes Globais ---
/**
 * @const {number} BOARD_SIZE - Define o tamanho do tabuleiro (8 para 8x8).
 */
const BOARD_SIZE = 8;

/**
 * @const {Object.<string, string>} PIECE_SYMBOLS - Mapeia as abreviações das peças
 * (ex: 'wp' para Peão Branco) aos seus respectivos símbolos Unicode.
 * As abreviações seguem o padrão: [cor][tipo_da_peça] (w=white/b=black, p=pawn/r=rook/n=knight/b=bishop/q=queen/k=king).
 */
const PIECE_SYMBOLS = {
    'wp': '♙', 'wr': '♖', 'wn': '♘', 'wb': '♗', 'wq': '♕', 'wk': '♔',
    'bp': '♟', 'br': '♜', 'bn': '♞', 'bb': '♝', 'bq': '♛', 'bk': '♚',
};

/**
 * @const {Array<Array<string|null>>} INITIAL_BOARD - Representa o estado inicial do tabuleiro de xadrez.
 * Cada sub-array é uma linha do tabuleiro. `null` indica uma casa vazia.
 */
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

// --- Componente Principal do Jogo de Xadrez (App) ---
/**
 * @function App
 * @brief Componente React principal que gerencia o estado do jogo e renderiza a interface.
 * Inclui o tabuleiro, mensagens de estado e controles do jogo.
 *
 * @returns {JSX.Element} O elemento JSX que representa a interface do jogo.
 */
export default function App() {
    /**
     * @state {Array<Array<string|null>>} board - O estado atual do tabuleiro de xadrez.
     * É uma matriz 8x8 onde cada elemento é uma string de peça (ex: 'wp') ou null.
     */
    const [board, setBoard] = useState(INITIAL_BOARD);

    /**
     * @state {{row: number, col: number}|null} selectedPiece - O estado da peça atualmente selecionada.
     * Contém {row, col} da peça se uma foi clicada, ou `null` se nenhuma peça estiver selecionada.
     */
    const [selectedPiece, setSelectedPiece] = useState(null);

    /**
     * @state {'w'|'b'} turn - Indica de quem é o turno atual. 'w' para as brancas, 'b' para as pretas.
     */
    const [turn, setTurn] = useState('w');

    /**
     * @state {string} message - A mensagem atual exibida para o usuário sobre o estado do jogo.
     */
    const [message, setMessage] = useState('Turno das Brancas');

    /**
     * @function getPieceColor
     * @brief Retorna a cor de uma peça ('w' para branco, 'b' para preto) dado seu código.
     * @param {string|null} piece - O código da peça (ex: 'wp', 'br').
     * @returns {'w'|'b'|null} A cor da peça ou `null` se a casa estiver vazia.
     */
    const getPieceColor = (piece) => piece ? piece[0] : null;

    /**
     * @function isValidPosition
     * @brief Verifica se uma dada coordenada (linha, coluna) está dentro dos limites válidos do tabuleiro.
     * @param {number} r - O índice da linha.
     * @param {number} c - O índice da coluna.
     * @returns {boolean} `true` se a posição é válida, `false` caso contrário.
     */
    const isValidPosition = (r, c) => r >= 0 && r < BOARD_SIZE && c >= 0 && c < BOARD_SIZE;

    /*
     * --- Funções de Validação de Movimento para Cada Tipo de Peça ---
     * Estas funções aplicam as estruturas de repetição e condições conforme os objetivos do desafio em PDF,
     * garantindo a lógica de movimento para um jogo jogável.
     */

    /**
     * @function canPawnMove
     * @brief Valida o movimento de um Peão.
     * Aplica múltiplas condições para diferentes tipos de movimento do peão (1 casa, 2 casas, captura).
     * @param {number} startRow - Linha inicial do peão.
     * @param {number} startCol - Coluna inicial do peão.
     * @param {number} endRow - Linha de destino.
     * @param {number} endCol - Coluna de destino.
     * @param {'w'|'b'} pieceColor - Cor do peão ('w' ou 'b').
     * @param {Array<Array<string|null>>} boardState - O estado atual do tabuleiro.
     * @returns {boolean} `true` se o movimento do peão é válido, `false` caso contrário.
     */
    const canPawnMove = (startRow, startCol, endRow, endCol, pieceColor, boardState) => {
        const direction = pieceColor === 'w' ? -1 : 1; // Brancas sobem (-1), Pretas descem (1)
        const startRank = pieceColor === 'w' ? 6 : 1; // Linha de início para o primeiro movimento do peão

        const targetPiece = boardState[endRow][endCol]; // Peça na casa de destino

        // 1. Movimento normal de uma casa para frente
        // Condição: move uma casa na direção correta e a casa de destino está vazia.
        if (endCol === startCol && endRow === startRow + direction && !targetPiece) {
            return true;
        }

        // 2. Primeiro movimento de duas casas para frente
        // Condições: está na linha inicial, move duas casas na direção correta,
        // a casa de destino está vazia, E a casa intermediária também está vazia.
        if (startRow === startRank && endCol === startCol && endRow === startRow + 2 * direction && !targetPiece && !boardState[startRow + direction][startCol]) {
            return true;
        }

        // 3. Captura diagonal
        // Condições: move uma casa na diagonal (coluna +/- 1), na direção correta,
        // E há uma peça inimiga na casa de destino.
        if (Math.abs(endCol - startCol) === 1 && endRow === startRow + direction && targetPiece && getPieceColor(targetPiece) !== pieceColor) {
            return true;
        }

        return false; // Se nenhuma das condições acima for satisfeita, o movimento é inválido.
    };

    /**
     * @function canRookMove
     * @brief Valida o movimento de uma Torre.
     * Aplica loops 'for' para verificar o caminho linear (horizontal ou vertical)
     * e garantir que não há peças bloqueando o percurso.
     * @param {number} startRow - Linha inicial da torre.
     * @param {number} startCol - Coluna inicial da torre.
     * @param {number} endRow - Linha de destino.
     * @param {number} endCol - Coluna de destino.
     * @param {Array<Array<string|null>>} boardState - O estado atual do tabuleiro.
     * @returns {boolean} `true` se o movimento da torre é válido, `false` caso contrário.
     */
    const canRookMove = (startRow, startCol, endRow, endCol, boardState) => {
        if (startRow === endRow) { // Movimento horizontal
            const step = (endCol > startCol) ? 1 : -1; // Define a direção do movimento (direita: +1, esquerda: -1)
            // Loop 'for' para iterar por todas as casas entre a origem e o destino na horizontal.
            // Verifica se alguma casa no caminho está ocupada.
            for (let c = startCol + step; c !== endCol; c += step) {
                if (boardState[startRow][c]) return false; // Caminho bloqueado por outra peça
            }
            return true; // Caminho livre
        } else if (startCol === endCol) { // Movimento vertical
            const step = (endRow > startRow) ? 1 : -1; // Define a direção do movimento (para baixo: +1, para cima: -1)
            // Loop 'for' para iterar por todas as casas entre a origem e o destino na vertical.
            // Verifica se alguma casa no caminho está ocupada.
            for (let r = startRow + step; r !== endRow; r += step) {
                if (boardState[r][startCol]) return false; // Caminho bloqueado por outra peça
            }
            return true; // Caminho livre
        }
        return false; // Se não for movimento horizontal nem vertical, é inválido para a torre.
    };

    /**
     * @function canKnightMove
     * @brief Valida o movimento de um Cavalo.
     * Verifica diretamente as 8 posições possíveis em "L". Embora não use loops explícitos
     * para percorrer um "caminho" (já que o cavalo salta), a checagem das diferenças de
     * coordenadas reflete a lógica de "loops aninhados" ao combinar variações de linha e coluna.
     * @param {number} startRow - Linha inicial do cavalo.
     * @param {number} startCol - Coluna inicial do cavalo.
     * @param {number} endRow - Linha de destino.
     * @param {number} endCol - Coluna de destino.
     * @returns {boolean} `true` se o movimento do cavalo é válido, `false` caso contrário.
     */
    const canKnightMove = (startRow, startCol, endRow, endCol) => {
        const dr = Math.abs(endRow - startRow); // Diferença absoluta entre as linhas (delta row)
        const dc = Math.abs(endCol - startCol); // Diferença absoluta entre as colunas (delta column)

        // O movimento do cavalo é sempre um "L": (2 casas em uma direção e 1 na perpendicular)
        // ou (1 casa em uma direção e 2 na perpendicular).
        return (dr === 2 && dc === 1) || (dr === 1 && dc === 2);
    };

    /**
     * @function canBishopMove
     * @brief Valida o movimento de um Bispo.
     * Aplica loops 'while' para verificar o caminho diagonal e garantir que não há peças bloqueando.
     * @param {number} startRow - Linha inicial do bispo.
     * @param {number} startCol - Coluna inicial do bispo.
     * @param {number} endRow - Linha de destino.
     * @param {number} endCol - Coluna de destino.
     * @param {Array<Array<string|null>>} boardState - O estado atual do tabuleiro.
     * @returns {boolean} `true` se o movimento do bispo é válido, `false` caso contrário.
     */
    const canBishopMove = (startRow, startCol, endRow, endCol, boardState) => {
        // Verifica se é um movimento diagonal (a diferença absoluta de linhas deve ser igual à de colunas)
        if (Math.abs(endRow - startRow) === Math.abs(endCol - startCol)) {
            const rowStep = (endRow > startRow) ? 1 : -1; // Direção da linha (para baixo: +1, para cima: -1)
            const colStep = (endCol > startCol) ? 1 : -1; // Direção da coluna (para direita: +1, para esquerda: -1)

            let r = startRow + rowStep; // Começa a verificar a partir da casa adjacente à peça
            let c = startCol + colStep;

            // Loop 'while' para iterar por todas as casas na diagonal até o destino.
            // Verifica se alguma casa no caminho está ocupada.
            while (r !== endRow && c !== endCol) {
                if (boardState[r][c]) return false; // Caminho bloqueado por outra peça
                r += rowStep;
                c += colStep;
            }
            return true; // Caminho livre
        }
        return false; // Se não for um movimento diagonal, é inválido para o bispo.
    };

    /**
     * @function canQueenMove
     * @brief Valida o movimento de uma Rainha.
     * Combina a lógica de movimento da Torre e do Bispo, utilizando suas respectivas
     * funções de validação de caminho.
     * @param {number} startRow - Linha inicial da rainha.
     * @param {number} startCol - Coluna inicial da rainha.
     * @param {number} endRow - Linha de destino.
     * @param {number} endCol - Coluna de destino.
     * @param {Array<Array<string|null>>} boardState - O estado atual do tabuleiro.
     * @returns {boolean} `true` se o movimento da rainha é válido, `false` caso contrário.
     */
    const canQueenMove = (startRow, startCol, endRow, endCol, boardState) => {
        // A Rainha pode se mover como uma Torre OU como um Bispo.
        // Reutiliza as funções de validação já implementadas.
        return canRookMove(startRow, startCol, endRow, endCol, boardState) ||
               canBishopMove(startRow, startCol, endRow, endCol, boardState);
    };

    /**
     * @function canKingMove
     * @brief Valida o movimento de um Rei.
     * Aplica condições simples de distância para verificar movimentos de uma casa em qualquer direção.
     * @param {number} startRow - Linha inicial do rei.
     * @param {number} startCol - Coluna inicial do rei.
     * @param {number} endRow - Linha de destino.
     * @param {number} endCol - Coluna de destino.
     * @returns {boolean} `true` se o movimento do rei é válido, `false` caso contrário.
     */
    const canKingMove = (startRow, startCol, endRow, endCol) => {
        const dr = Math.abs(endRow - startRow); // Diferença absoluta entre as linhas
        const dc = Math.abs(endCol - startCol); // Diferença absoluta entre as colunas

        // O Rei pode mover uma casa em qualquer direção (horizontal, vertical ou diagonal).
        // A diferença máxima em linha e coluna deve ser 1.
        return dr <= 1 && dc <= 1;
    };

    /**
     * @function isValidMove
     * @brief Função principal para validar qualquer movimento de peça no tabuleiro.
     * Atua como um 'dispatcher', delegando a validação para a função específica de cada peça.
     * @param {number} startRow - Linha de origem da peça.
     * @param {number} startCol - Coluna de origem da peça.
     * @param {number} endRow - Linha de destino da peça.
     * @param {number} endCol - Coluna de destino da peça.
     * @returns {boolean} `true` se o movimento é válido de acordo com as regras da peça e do tabuleiro, `false` caso contrário.
     */
    const isValidMove = (startRow, startCol, endRow, endCol) => {
        const piece = board[startRow][startCol]; // A peça que está na casa de origem
        const pieceType = piece[1]; // O tipo da peça (ex: 'p', 'r', 'n')
        const pieceColor = piece[0]; // A cor da peça (ex: 'w', 'b')

        const targetPiece = board[endRow][endCol]; // A peça (se houver) na casa de destino
        const targetColor = getPieceColor(targetPiece); // A cor da peça de destino

        // --- Condições Básicas de Xadrez Aplicáveis a Qualquer Movimento ---
        // 1. Não pode mover para a mesma casa (movimento nulo).
        if (startRow === endRow && startCol === endCol) return false;
        // 2. Não pode capturar uma peça da sua própria cor.
        if (targetPiece && targetColor === pieceColor) return false;
        // 3. A posição final deve ser válida (dentro dos limites do tabuleiro).
        if (!isValidPosition(endRow, endCol)) return false;

        // --- Delega a validação para a função específica de cada tipo de peça ---
        // Utiliza um `switch` para direcionar a validação, otimizando o fluxo de controle.
        switch (pieceType) {
            case 'p': return canPawnMove(startRow, startCol, endRow, endCol, pieceColor, board);
            case 'r': return canRookMove(startRow, startCol, endRow, endCol, board);
            case 'n': return canKnightMove(startRow, startCol, endRow, endCol);
            case 'b': return canBishopMove(startRow, startCol, endRow, endCol, board);
            case 'q': return canQueenMove(startRow, startCol, endRow, endCol, board);
            case 'k': return canKingMove(startRow, startCol, endRow, endCol);
            default: return false; // Retorna falso se o tipo de peça for desconhecido.
        }
    };

    /**
     * @function handleSquareClick
     * @brief Gerencia a lógica quando uma célula do tabuleiro é clicada.
     * Lida com a seleção de peças e a tentativa de movimentos.
     * @param {number} row - Linha da célula clicada.
     * @param {number} col - Coluna da célula clicada.
     */
    const handleSquareClick = (row, col) => {
        // Cenário 1: Uma peça já foi selecionada anteriormente
        if (selectedPiece) {
            const { row: sr, col: sc } = selectedPiece; // Coordenadas da peça selecionada
            const pieceToMove = board[sr][sc]; // A string que representa a peça a ser movida

            // Tenta mover a peça selecionada para a nova casa clicada (destino)
            if (isValidMove(sr, sc, row, col)) {
                // Cria uma nova cópia do tabuleiro para manter a imutabilidade do estado no React.
                // Isso é crucial para que o React detecte a mudança e re-renderize o componente.
                const newBoard = board.map(arr => [...arr]);

                // Verifica se houve captura de peça na casa de destino
                if (newBoard[row][col]) {
                    setMessage(`${PIECE_SYMBOLS[pieceToMove]} capturou ${PIECE_SYMBOLS[newBoard[row][col]]}!`);
                } else {
                    setMessage('Movimento válido!');
                }

                newBoard[row][col] = pieceToMove; // Move a peça para a nova posição no tabuleiro
                newBoard[sr][sc] = null; // Limpa a posição anterior da peça

                setBoard(newBoard); // Atualiza o estado do tabuleiro com o novo array
                setTurn(turn === 'w' ? 'b' : 'w'); // Troca o turno (brancas <-> pretas)
                setSelectedPiece(null); // Desseleciona a peça após o movimento bem-sucedido
            } else {
                // Se o movimento for inválido, exibe uma mensagem e desseleciona a peça.
                setMessage('Movimento inválido! Tente novamente.');
                setSelectedPiece(null);
            }
        } else {
            // Cenário 2: Nenhuma peça selecionada, tenta selecionar uma peça na casa clicada
            const clickedPiece = board[row][col]; // A peça na casa clicada

            // Só permite selecionar uma peça se a casa não estiver vazia E a peça for da cor do turno atual.
            if (clickedPiece && getPieceColor(clickedPiece) === turn) {
                setSelectedPiece({ row, col }); // Define a peça clicada como a peça selecionada
                setMessage(`Peça ${PIECE_SYMBOLS[clickedPiece]} selecionada. Escolha um destino.`);
            } else if (clickedPiece && getPieceColor(clickedPiece) !== turn) {
                // Se clicou em uma peça do oponente, informa que não pode movê-la.
                setMessage('Essa peça não é sua! Escolha uma peça da sua cor.');
            } else {
                // Se clicou em uma casa vazia sem ter uma peça selecionada.
                setMessage('Nenhuma peça selecionada. Clique em uma peça sua para mover.');
            }
        }
    };

    /**
     * @function resetGame
     * @brief Reinicia o estado do jogo para as configurações iniciais.
     * O tabuleiro é resetado, a peça selecionada é limpa e o turno volta para as brancas.
     */
    const resetGame = () => {
        setBoard(INITIAL_BOARD); // Restaura o tabuleiro para o estado inicial
        setSelectedPiece(null); // Nenhuma peça selecionada
        setTurn('w'); // Turno volta para as brancas
        setMessage('Jogo Reiniciado. Turno das Brancas'); // Mensagem de reinício
    };

    /**
     * @useEffect
     * @brief Um efeito colateral que é executado sempre que o estado 'turn' muda.
     * Atualiza a mensagem exibida para o usuário indicando de quem é o turno atual.
     */
    useEffect(() => {
        setMessage(`Turno das ${turn === 'w' ? 'Brancas' : 'Pretas'}`);
    }, [turn]); // O efeito depende do estado 'turn'

    // --- Renderização da Interface do Usuário (JSX) ---
    return (
        <div className="bg-gray-100 text-gray-900 p-4 min-h-screen flex flex-col items-center justify-center">
            <div className="container mx-auto p-6 bg-white shadow-lg rounded-lg max-w-4xl w-full flex flex-col items-center">
                {/* Título do Jogo */}
                <h1 className="text-4xl font-extrabold text-center mb-6 text-indigo-800 animate-pulse">Xadrez</h1>
                {/* Descrição do Jogo */}
                <p className="text-center text-lg mb-8 text-gray-700">Mostre suas tecnicas</p>

                <div className="flex flex-col md:flex-row gap-8 w-full justify-center items-center">
                    {/* Painel de Mensagens e Controles do Jogo */}
                    <div className="md:w-1/3 p-4 bg-indigo-50 rounded-lg shadow-inner flex flex-col justify-between h-48 md:h-80">
                        <div>
                            {/* Título do Painel de Estado */}
                            <h2 className="text-2xl font-semibold mb-3 text-indigo-700">Estado do Jogo</h2>
                            {/* Mensagem atual do jogo */}
                            <p className="text-xl font-bold text-gray-800 mb-4">{message}</p>
                        </div>
                        {/* Botão para Reiniciar o Jogo */}
                        <button
                            onClick={resetGame}
                            className="w-full bg-red-600 hover:bg-red-700 text-white font-bold py-3 px-4 rounded-xl shadow-md transition duration-300 ease-in-out transform hover:scale-105"
                        >
                            Reiniciar Jogo
                        </button>
                    </div>

                    {/* Tabuleiro de Xadrez Interativo */}
                    <div className="md:w-2/3 flex justify-center items-center p-4 bg-gray-50 rounded-lg shadow-xl">
                        <div className="board-grid grid grid-cols-8 grid-rows-8 border-4 border-gray-700 rounded-lg overflow-hidden">
                            {/* Mapeia as linhas do tabuleiro */}
                            {board.map((row, rowIndex) => (
                                // Mapeia as colunas de cada linha
                                row.map((piece, colIndex) => {
                                    // Determina a cor de fundo da casa (padrão de xadrez)
                                    const isLightSquare = (rowIndex + colIndex) % 2 === 0;
                                    const squareColorClass = isLightSquare ? 'bg-amber-100' : 'bg-amber-700'; // Cores amadeiradas

                                    // Adiciona uma classe CSS se a casa contiver a peça selecionada
                                    const isSelected = selectedPiece && selectedPiece.row === rowIndex && selectedPiece.col === colIndex;
                                    const selectedClass = isSelected ? 'ring-4 ring-blue-500 ring-offset-2' : '';

                                    return (
                                        <div
                                            key={`${rowIndex}-${colIndex}`} // Chave única para cada célula do React
                                            className={`w-10 h-10 md:w-12 md:h-12 flex justify-center items-center text-3xl md:text-4xl cursor-pointer select-none transition-all duration-100 ${squareColorClass} ${selectedClass} rounded-sm`}
                                            onClick={() => handleSquareClick(rowIndex, colIndex)} // Manipulador de clique
                                        >
                                            {/* Exibe o símbolo da peça se a casa não estiver vazia */}
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
                {/* Rodapé ou informação do desenvolvedor */}
                <p className="text-sm mt-8 text-gray-500">
                    Desenvolvido para Iniciante 
                </p>
            </div>
        </div>
    );
}
