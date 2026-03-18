#include <stdio.h>
#include <time.h>

//quantidade maxima de jogadores armazenados
#define MAX_JOGADORES 5

//estrutura que armazena dados dos jogadores
typedef struct {
  char nome[50];
  int pontuacao;
  int tentativas;
} 
Jogador;

//estrutura que calcula as pontuações

int calcularPontuacao(int tentativas) {

  int pontos = 100 - (tentativas - 1) * 5;

  if (pontos < 0) pontos = 0;

  return pontos;

}


//estrutura que recebe palpites dos jogadores

int jogo(char nomeJogador[]) {

  int numSecreto, palpite;

  int tentativas = 0;
  //estrutura que gera um número aleatorio de 1 a 100
  numSecreto = (int)(time(NULL) % 100) + 1;
  printf("\n=== Iniciando jogo com %s ===\n", nomeJogador);
  printf("Tente adivinhar o numero entre 1 e 100! (Digite abaixo seu palpite!)\n");
  do {
    printf("Palpite: ");
    scanf("%i", &palpite);
    tentativas++;
    if (palpite < numSecreto)
      printf("O número é maior!\n");
    else if (palpite > numSecreto)
      printf("O número é menor!\n");
  } while (palpite != numSecreto);
  printf("Parábens você acertou em %i tentativas!\n", tentativas);
  return tentativas;
}
//estrutura que exibe os Ranks dos jogadores
void mostrarRanking(Jogador jogadores[], int quantidade) {
  printf("\n===== RANK DOS JOGADORES =====\n");
  for (int i = 0; i < quantidade; i++) {
    printf("%i. %s - %i pontos (tentativas: %i)\n",
        i + 1,
        jogadores[i].nome,
        jogadores[i].pontuacao,
        jogadores[i].tentativas);
  }
  printf("=====================\n\n");
}
//estrutura qu exibe o Menu do jogo
int main() {
  Jogador jogadores[MAX_JOGADORES];
  int quantidadeJogadores = 0;
  int opcao;

  do {
    printf("\n====== MENU ======\n");
    printf("1 - jogo\n");
    printf("2 - Mostrar ranking\n");
    printf("3 - Sair\n");
    printf("Escolha: ");
    scanf("%i", &opcao);

    if (opcao == 1) {
      if (quantidadeJogadores < MAX_JOGADORES) {
        printf("Digite o nome do jogador: ");
        scanf("%s", jogadores[quantidadeJogadores].nome);

        int tentativas = jogo(jogadores[quantidadeJogadores].nome);

        jogadores[quantidadeJogadores].tentativas = tentativas;
        jogadores[quantidadeJogadores].pontuacao = calcularPontuacao(tentativas);
        quantidadeJogadores++;

      } else {
        printf("Limite de jogadores atingido!\n");
      }
    }
    else if (opcao == 2) {
      mostrarRanking(jogadores, quantidadeJogadores);

    }
    else if (opcao == 3) {
      printf("Saindo...\n");
    }
    else {
      printf("Opcao invalida!\n");

    }

  } while (opcao != 3);

  return 0;

}