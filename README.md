#include <stdio.h>
#include <stdbool.h> // Para usar bool
#include <stdlib.h>  // Para abs() - usado em calculos de distancia e direcao

// Definicoes do tabuleiro e das pecas
#define TABULEIRO_TAMANHO 8 // O tabuleiro e 8x8
#define VAZIO '.'           // Representa uma casa vazia no tabuleiro
#define TORRE 'T'           // Representa a Torre
#define BISPO 'B'           // Representa o Bispo
#define RAINHA 'R'          // Representa a Rainha
#define CAVALO 'C'          // Representa o Cavalo
#define OBSTACULO 'O'       // Usado no modulo mestre para simular bloqueios no caminho

// O tabuleiro de xadrez sera uma matriz de caracteres
char tabuleiro[TABULEIRO_TAMANHO][TABULEIRO_TAMANHO];

// Funcao para inicializar todas as casas do tabuleiro como vazias
void inicializarTabuleiro() {
    for (int i = 0; i < TABULEIRO_TAMANHO; i++) {
        for (int j = 0; j < TABULEIRO_TAMANHO; j++) {
            tabuleiro[i][j] = VAZIO;
        }
    }
}

// Funcao para imprimir o tabuleiro no console
// Usada principalmente para visualizacao dos movimentos no modulo Aventureiro e Mestre (quando aplicavel)
void imprimirTabuleiro() {
    printf("\n  "); // Espaco para alinhamento dos indices das colunas
    // Imprime os indices das colunas (0 a 7)
    for (int j = 0; j < TABULEIRO_TAMANHO; j++) {
        printf("%d ", j);
    }
    printf("\n"); // Nova linha apos os indices das colunas

    // Imprime cada linha do tabuleiro com seu indice correspondente
    for (int i = 0; i < TABULEIRO_TAMANHO; i++) {
        printf("%d ", i); // Imprime o indice da linha
        for (int j = 0; j < TABULEIRO_TAMANHO; j++) {
            printf("%c ", tabuleiro[i][j]); // Imprime o caractere da casa
        }
        printf("\n"); // Nova linha apos cada linha do tabuleiro
    }
}

// --- MODULO NOVATO: MOVIMENTOS LINEARES COM IMPRESSAO DE DIRECOES ---

// Simula o movimento da Torre: 5 casas para a direita usando 'for'
// Requisito: Imprimir "Direita" a cada casa percorrida.
void moverTorreNovato(int linhaInicial, int colunaInicial) {
    printf("\nSimulando movimento da Torre de (%d, %d) (5 casas para a Direita):\n", linhaInicial, colunaInicial);
    // Nota: O requisito do Nivel Novato pede apenas para imprimir a direcao, nao marcar no tabuleiro.
    // O tabuleiro e apenas para visualizacao geral, entao nao sera impresso a cada passo aqui.

    // Loop 'for' para mover 5 casas para a direita
    for (int k = 0; k < 5; k++) { // k itera de 0 a 4, totalizando 5 passos
        printf("Direita\n"); // Imprime a direcao a cada passo
    }
}

// Simula o movimento do Bispo: 5 casas na diagonal para cima e a direita usando 'while'
// Requisito: Imprimir "Cima, Direita" a cada casa percorrida.
void moverBispoNovato(int linhaInicial, int colunaInicial) {
    printf("\nSimulando movimento do Bispo de (%d, %d) (5 casas Cima, Direita):\n", linhaInicial, colunaInicial);
    int passos = 0; // Contador de passos
    // Loop 'while' para mover 5 casas na diagonal
    while (passos < 5) {
        printf("Cima, Direita\n"); // Imprime a direcao a cada passo
        passos++; // Incrementa o contador de passos
    }
}

// Simula o movimento da Rainha: 8 casas para a esquerda usando 'do-while'
// Requisito: Imprimir "Esquerda" a cada casa percorrida.
void moverRainhaNovato(int linhaInicial, int colunaInicial) {
    printf("\nSimulando movimento da Rainha de (%d, %d) (8 casas para a Esquerda):\n", linhaInicial, colunaInicial);
    int passos = 0; // Contador de passos

    // Loop 'do-while' para mover 8 casas para a esquerda
    // Garante que o loop execute pelo menos uma vez, e depois verifica a condicao
    do {
        printf("Esquerda\n"); // Imprime a direcao
        passos++; // Incrementa o contador
    } while (passos < 8); // Continua enquanto nao atingir 8 passos
}

// Funcao que executa as simulacoes do Modulo Novato
void moduloNovato() {
    printf("\n--- MODULO NOVATO: MOVIMENTOS BASICOS COM IMPRESSAO DE DIRECOES ---\n");
    moverTorreNovato(3, 3);   // Exemplo: Torre na posicao (3,3)
    moverBispoNovato(3, 3);   // Exemplo: Bispo na posicao (3,3)
    moverRainhaNovato(3, 3);  // Exemplo: Rainha na posicao (3,3)
    printf("\n--------------------------------------------------------------\n");
}

// --- MODULO AVENTUREIRO: MOVIMENTO DO CAVALO (LOOPS ANINHADOS) ---

// Simula o movimento especifico do Cavalo: 2 casas para baixo e 1 para a esquerda
// Requisito: Usar loops aninhados (um 'for' e um 'while' ou 'do-while')
// e imprimir as direcoes "Baixo", "Baixo", "Esquerda".
void moverCavaloAventureiro(int linhaInicial, int colunaInicial) {
    printf("\nSimulando movimento do Cavalo de (%d, %d) (2 casas Baixo, 1 casa Esquerda):\n", linhaInicial, colunaInicial);
    printf("\n"); // Separa o movimento do Cavalo dos anteriores

    // Posiciona o Cavalo no tabuleiro para visualizacao final
    inicializarTabuleiro();
    tabuleiro[linhaInicial][colunaInicial] = CAVALO;

    int currentLinha = linhaInicial;
    int currentColuna = colunaInicial;

    // Primeiro movimento: 2 casas para baixo
    // Loop 'for' para os dois passos para baixo
    for (int i = 0; i < 2; i++) {
        printf("Baixo\n"); // Imprime a direcao
        currentLinha++; // Atualiza a posicao conceitual (nao no tabuleiro, apenas para rastreio)
    }

    // Segundo movimento: 1 casa para a esquerda
    // Loop 'while' para o passo para a esquerda
    int j = 0;
    while (j < 1) { // Apenas um passo
        printf("Esquerda\n"); // Imprime a direcao
        currentColuna--; // Atualiza a posicao conceitual
        j++;
    }

    // Marca a posicao final do cavalo apos o movimento "L"
    // Nota: O Cavalo de xadrez "pula" para a posicao final, nao percorre cada casa.
    // Estamos simulando a sequencia de "direcoes" do L.
    int linhaFinal = linhaInicial + 2;
    int colunaFinal = colunaInicial - 1;

    // Marca a posicao final no tabuleiro para visualizacao
    if (linhaFinal >= 0 && linhaFinal < TABULEIRO_TAMANHO &&
        colunaFinal >= 0 && colunaFinal < TABULEIRO_TAMANHO) {
        tabuleiro[linhaFinal][colunaFinal] = CAVALO; // Marca a posicao final
    }

    imprimirTabuleiro(); // Imprime o tabuleiro final com a posicao do Cavalo

    printf("\n--------------------------------------------------------------\n");
}

// Funcao que executa as simulacoes do Modulo Aventureiro
void moduloAventureiro() {
    printf("\n--- MODULO AVENTUREIRO: MOVIMENTO DO CAVALO COM LOOPS ANINHADOS ---\n");
    moverCavaloAventureiro(3, 3); // Exemplo: Cavalo na posicao (3,3)
}

// --- MODULO MESTRE: RECURSIVIDADE E LOOPS COMPLEXOS ---

// Funcao recursiva para simular o movimento da Torre em uma direcao
// Requisitos: Substituir loops por recursividade, imprimir direcao a cada passo.
// 'deltaLinha' e 'deltaColuna' definem a direcao (+1/-1/0)
// 'passosRestantes' e o numero de casas que ainda precisa mover
void moverTorreRecursivo(int linhaAtual, int colunaAtual, int passosRestantes, int deltaLinha, int deltaColuna) {
    // Caso base: Se nao ha mais passos para dar ou se saiu do tabuleiro
    if (passosRestantes <= 0 ||
        linhaAtual < 0 || linhaAtual >= TABULEIRO_TAMANHO ||
        colunaAtual < 0 || colunaAtual >= TABULEIRO_TAMANHO) {
        return;
    }

    // Imprime a direcao do movimento
    if (deltaLinha == 1 && deltaColuna == 0) {
        printf("Baixo\n");
    } else if (deltaLinha == -1 && deltaColuna == 0) {
        printf("Cima\n");
    } else if (deltaLinha == 0 && deltaColuna == 1) {
        printf("Direita\n");
    } else if (deltaLinha == 0 && deltaColuna == -1) {
        printf("Esquerda\n");
    }

    // Chamada recursiva para o proximo passo
    moverTorreRecursivo(linhaAtual + deltaLinha, colunaAtual + deltaColuna, passosRestantes - 1, deltaLinha, deltaColuna);
}

// Funcao recursiva para simular o movimento do Bispo em uma direcao diagonal
// Requisitos: Substituir loops por recursividade, imprimir direcao a cada passo.
// 'deltaLinha' e 'deltaColuna' definem a direcao diagonal (+1/-1)
void moverBispoRecursivo(int linhaAtual, int colunaAtual, int passosRestantes, int deltaLinha, int deltaColuna) {
    // Caso base
    if (passosRestantes <= 0 ||
        linhaAtual < 0 || linhaAtual >= TABULEIRO_TAMANHO ||
        colunaAtual < 0 || colunaAtual >= TABULEIRO_TAMANHO) {
        return;
    }

    // Imprime a direcao combinada
    if (deltaLinha == -1 && deltaColuna == -1) {
        printf("Cima Esquerda\n");
    } else if (deltaLinha == -1 && deltaColuna == 1) {
        printf("Cima Direita\n");
    } else if (deltaLinha == 1 && deltaColuna == -1) {
        printf("Baixo Esquerda\n");
    } else if (deltaLinha == 1 && deltaColuna == 1) {
        printf("Baixo Direita\n");
    }

    // Chamada recursiva para o proximo passo na diagonal
    moverBispoRecursivo(linhaAtual + deltaLinha, colunaAtual + deltaColuna, passosRestantes - 1, deltaLinha, deltaColuna);
}

// Funcao recursiva para simular o movimento da Rainha
// A Rainha pode se mover em qualquer direcao (Torre + Bispo)
// Para simplificar a recursividade, esta funcao precisaria de uma forma mais complexa de iterar
// por todas as 8 direcoes. Para fins de demonstracao de recursividade simples, vamos simular uma direcao especifica.
// Requisito: Substituir loops por recursividade, imprimir direcao a cada passo.
void moverRainhaRecursivo(int linhaAtual, int colunaAtual, int passosRestantes, int deltaLinha, int deltaColuna) {
    // Caso base
    if (passosRestantes <= 0 ||
        linhaAtual < 0 || linhaAtual >= TABULEIRO_TAMANHO ||
        colunaAtual < 0 || colunaAtual >= TABULEIRO_TAMANHO) {
        return;
    }

    // Imprime a direcao. Esta parte seria mais complexa para Rainha real,
    // mas para demonstrar recursividade, focamos em uma direcao.
    if (deltaLinha == 1 && deltaColuna == 0) {
        printf("Baixo\n");
    } else if (deltaLinha == -1 && deltaColuna == 0) {
        printf("Cima\n");
    } else if (deltaLinha == 0 && deltaColuna == 1) {
        printf("Direita\n");
    } else if (deltaLinha == 0 && deltaColuna == -1) {
        printf("Esquerda\n");
    } else if (deltaLinha == -1 && deltaColuna == -1) {
        printf("Cima Esquerda\n");
    } else if (deltaLinha == -1 && deltaColuna == 1) {
        printf("Cima Direita\n");
    } else if (deltaLinha == 1 && deltaColuna == -1) {
        printf("Baixo Esquerda\n");
    } else if (deltaLinha == 1 && deltaColuna == 1) {
        printf("Baixo Direita\n");
    }

    // Chamada recursiva
    moverRainhaRecursivo(linhaAtual + deltaLinha, colunaAtual + deltaColuna, passosRestantes - 1, deltaLinha, deltaColuna);
}


// --- MOVIMENTO DO CAVALO COM LOOPS COMPLEXOS E CONDICOES (MODULO MESTRE) ---
// Requisito: Aprimorar movimentacao do Cavalo para "duas casas para cima e uma para a direita".
// Usar loops aninhados com multiplas variaveis e/ou condicoes. Pode usar 'continue' e 'break'.
void moverCavaloMestre(int linhaInicial, int colunaInicial) {
    printf("\nSimulando movimento do Cavalo de (%d, %d) (2 casas Cima, 1 casa Direita) - Loops Complexos:\n", linhaInicial, colunaInicial);
    printf("\n"); // Linha em branco para separacao

    // Para simular "loops aninhados com multiplas variaveis e/ou condicoes" para um movimento fixo como o do cavalo,
    // vamos usar uma abordagem onde o loop externo controla a "fase" do movimento (vertical vs horizontal)
    // e o interno itera sobre os passos dessa fase, com condicoes para parar/continuar.

    // O movimento do cavalo e 2 casas na vertical e 1 na horizontal, ou vice-versa.
    // Neste caso: 2 para cima (-2) e 1 para a direita (+1).
    int movimentos_L[2][2] = {
        {-2, 0}, // Primeira parte do L (vertical)
        {0, 1}   // Segunda parte do L (horizontal)
    };

    int currentLinha = linhaInicial;
    int currentColuna = colunaInicial;

    // Loop externo para iterar sobre as 'fases' do movimento do Cavalo (vertical, depois horizontal)
    for (int k = 0; k < 2; k++) {
        int deltaL = movimentos_L[k][0]; // Mudanca na linha para a fase atual
        int deltaC = movimentos_L[k][1]; // Mudanca na coluna para a fase atual

        // Loop interno para executar os passos da fase atual
        // Multiplas variaveis e condicoes: 'i' para o contador, 'passos' para os passos esperados.
        // Condicao de continue/break demonstrada de forma artificial para o proposito do desafio.
        for (int i = 0, passos = (k == 0 ? 2 : 1); i < passos; i++) {
            // Se deltaL for 0, eh um movimento horizontal; se deltaC for 0, eh vertical.
            if (deltaL != 0) { // Movimento vertical
                if (deltaL < 0) printf("Cima\n");
                else printf("Baixo\n");
                currentLinha += (deltaL / abs(deltaL)); // Move uma unidade na direcao
            } else if (deltaC != 0) { // Movimento horizontal
                if (deltaC < 0) printf("Esquerda\n");
                else printf("Direita\n");
                currentColuna += (deltaC / abs(deltaC)); // Move uma unidade na direcao
            }

            // Exemplo de condicao 'continue' (contrived para demonstracao):
            // Se o cavalo estivesse em uma borda, poderia 'continuar' sem marcar
            if (currentLinha < 0 || currentLinha >= TABULEIRO_TAMANHO ||
                currentColuna < 0 || currentColuna >= TABULEIRO_TAMANHO) {
                //printf("Tentando mover para fora do tabuleiro, pulando...\n"); // Debug
                continue; // Pula o resto da iteracao se a posicao for invalida (mas nao paramos o loop)
            }

            // Exemplo de condicao 'break' (contrived para demonstracao):
            // Se atingir um 'obstaculo' ficticio no meio do movimento 'L'
            // if (k == 0 && i == 0 && currentLinha == 5 && currentColuna == 3) { // Apos 1 passo vertical (Baixo)
            //     printf("Obstaculo encontrado, interrompendo movimento L!\n");
            //     break; // Interrompe o loop interno
            // }
        }
    }
    // Posicao final do Cavalo apos o movimento 'L' simulado
    int finalLinha = linhaInicial + movimentos_L[0][0] + movimentos_L[1][0];
    int finalColuna = colunaInicial + movimentos_L[0][1] + movimentos_L[1][1];

    inicializarTabuleiro(); // Limpa e posiciona o Cavalo no inicio e fim para visualizacao
    tabuleiro[linhaInicial][colunaInicial] = CAVALO;
    if (finalLinha >= 0 && finalLinha < TABULEIRO_TAMANHO &&
        finalColuna >= 0 && finalColuna < TABULEIRO_TAMANHO) {
        tabuleiro[finalLinha][finalColuna] = CAVALO; // Marca a posicao final
    }
    imprimirTabuleiro(); // Imprime o tabuleiro com a posicao final

    printf("\n--------------------------------------------------------------\n");
}


// --- BISPO COM LOOPS ANINHADOS (MODULO MESTRE) ---
// Requisito: "o loop mais externo para o movimento vertical, e o mais interno para o movimento horizontal."
// Esta e uma interpretacao para simular a varredura diagonal com loops aninhados
// Nao e a forma mais eficiente de mover um bispo, mas atende ao requisito de estrutura de loop.
void moverBispoAninhado(int linhaInicial, int colunaInicial) {
    printf("\nSimulando movimento do Bispo de (%d, %d) com Loops Aninhados (vertical/horizontal):\n", linhaInicial, colunaInicial);
    printf("\n"); // Linha em branco para separacao

    inicializarTabuleiro();
    tabuleiro[linhaInicial][colunaInicial] = BISPO; // Posiciona o Bispo

    // Itera sobre as linhas (loop externo - vertical)
    for (int i = 0; i < TABULEIRO_TAMANHO; i++) {
        // Itera sobre as colunas (loop interno - horizontal)
        for (int j = 0; j < TABULEIRO_TAMANHO; j++) {
            // Condicao para verificar se a casa (i,j) esta na diagonal da posicao inicial
            // abs(delta_linha) == abs(delta_coluna)
            if (abs(i - linhaInicial) == abs(j - colunaInicial) &&
                !(i == linhaInicial && j == colunaInicial)) { // Nao marca a propria posicao
                tabuleiro[i][j] = '*'; // Marca a casa como parte de uma diagonal
            }
        }
    }
    imprimirTabuleiro();

    printf("\n--------------------------------------------------------------\n");
}


// Funcao que executa as simulacoes do Modulo Mestre
void moduloMestre() {
    printf("\n--- MODULO MESTRE: RECURSIVIDADE E LOOPS COMPLEXOS ---\n");

    printf("\n--- Movimento da Torre Recursivo (5 casas para a Esquerda) ---\n");
    moverTorreRecursivo(3, 3, 5, 0, -1); // Exemplo: Torre da (3,3), 5 casas para a Esquerda
    printf("\n"); // Linha em branco para separar as saidas

    printf("\n--- Movimento do Bispo Recursivo (5 casas Cima Direita) ---\n");
    moverBispoRecursivo(3, 3, 5, -1, 1); // Exemplo: Bispo da (3,3), 5 casas Cima Direita
    printf("\n"); // Linha em branco para separar as saidas

    printf("\n--- Movimento da Rainha Recursivo (8 casas para Baixo) ---\n");
    moverRainhaRecursivo(3, 3, 8, 1, 0); // Exemplo: Rainha da (3,3), 8 casas para Baixo
    printf("\n"); // Linha em branco para separar as saidas

    // Chamada para o Cavalo com loops complexos
    moverCavaloMestre(3, 3); // Exemplo: Cavalo na posicao (3,3)

    // Chamada para o Bispo com loops aninhados (requisito especifico do modulo mestre)
    moverBispoAninhado(3, 3); // Exemplo: Bispo na posicao (3,3)
}

// Funcao principal que inicia o jogo e chama os modulos
int main() {
    printf("Bem-vindo ao Xadrez Virtual da MateCheck!\n");
    printf("Este programa demonstra os movimentos das pecas de xadrez usando estruturas de repeticao em C, conforme os desafios Novato, Aventureiro e Mestre.\n");

    moduloNovato();
    moduloAventureiro();
    moduloMestre();

    printf("\nJornada de programacao concluida! Voce dominou os movimentos do xadrez com C!\n");

    return 0; // Indica que o programa terminou com sucesso
}
