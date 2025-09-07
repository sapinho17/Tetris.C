#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <unistd.h>  // Para usar sleep()

#define LINHAS 15
#define COLUNAS 10

void inicializarTabuleiro(char tabuleiro[LINHAS][COLUNAS]) {
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            tabuleiro[i][j] = '.';
        }
    }
}

void imprimirTabuleiro(char tabuleiro[LINHAS][COLUNAS]) {
    system("clear"); // use "cls" no Windows
    for (int i = 0; i < LINHAS; i++) {
        for (int j = 0; j < COLUNAS; j++) {
            printf("%c ", tabuleiro[i][j]);
        }
        printf("\n");
    }
}

// Verifica se a linha está completa
int linhaCompleta(char tabuleiro[LINHAS][COLUNAS], int linha) {
    for (int j = 0; j < COLUNAS; j++) {
        if (tabuleiro[linha][j] == '.') return 0;
    }
    return 1;
}

// Remove uma linha e desce o resto
void removerLinha(char tabuleiro[LINHAS][COLUNAS], int linha) {
    for (int i = linha; i > 0; i--) {
        for (int j = 0; j < COLUNAS; j++) {
            tabuleiro[i][j] = tabuleiro[i-1][j];
        }
    }
    for (int j = 0; j < COLUNAS; j++) {
        tabuleiro[0][j] = '.';
    }
}

// Peça simples: bloco 2x2
void colocarPeca(char tabuleiro[LINHAS][COLUNAS], int coluna) {
    int linha = 0;

    // desce até encostar em algo
    while (linha < LINHAS - 2 && tabuleiro[linha+2][coluna] == '.' && tabuleiro[linha+2][coluna+1] == '.') {
        linha++;
    }

    // coloca a peça
    tabuleiro[linha][coluna] = '#';
    tabuleiro[linha][coluna+1] = '#';
    tabuleiro[linha+1][coluna] = '#';
    tabuleiro[linha+1][coluna+1] = '#';
}

int main() {
    char tabuleiro[LINHAS][COLUNAS];
    int coluna, pontos = 0;

    srand(time(NULL));
    inicializarTabuleiro(tabuleiro);

    while (1) {
        imprimirTabuleiro(tabuleiro);

        printf("Escolha uma coluna (0 a %d) para soltar o bloco 2x2: ", COLUNAS-2);
        scanf("%d", &coluna);

        if (coluna < 0 || coluna >= COLUNAS-1) {
            printf("Coluna inválida!\n");
            continue;
        }

        colocarPeca(tabuleiro, coluna);

        // verificar linhas completas
        for (int i = 0; i < LINHAS; i++) {
            if (linhaCompleta(tabuleiro, i)) {
                removerLinha(tabuleiro, i);
                pontos += 10;
            }
        }

        // verificar se o jogo acabou (topo ocupado)
        for (int j = 0; j < COLUNAS; j++) {
            if (tabuleiro[0][j] != '.') {
                imprimirTabuleiro(tabuleiro);
                printf("\nGAME OVER! Pontos: %d\n", pontos);
                return
