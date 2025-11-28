# Desafio Detective Quest - Estruturas de Dados e Investigação

Bem-vindo ao desafio **Detective Quest**! Neste jogo de mistério, o jogador explora uma mansão, encontra pistas e relaciona evidências a suspeitos. Para tornar isso possível, você atuará como programador responsável por implementar toda a lógica de estruturas de dados do jogo.

A **Enigma Studios**, especializada em jogos educacionais, contratou você para criar a base de funcionamento da mansão e das investigações usando **árvore binária**, **árvore de busca** e **tabela hash**.

O desafio está dividido em três níveis: **Novato**, **Aventureiro** e **Mestre**, com cada nível adicionando mais complexidade ao anterior.  
**Você deve escolher qual desafio deseja realizar.**

🚨 **Atenção:** O nível Novato foca apenas na árvore binária de navegação de cômodos. Ideal para começar com estruturas básicas.

---

## 🎮 Nível Novato: Mapa da Mansão com Árvore Binária

No nível Novato, você criará a árvore binária que representa o **mapa da mansão**. Cada sala é um nó, e o jogador poderá explorar os caminhos à esquerda ou à direita, começando pelo "Hall de Entrada".

🚩 **Objetivo:** Criar um programa em C que:

- Construa dinamicamente uma árvore binária representando os cômodos.
- Permita que o jogador explore a mansão interativamente (esquerda, direita).
- Exiba o nome de cada cômodo visitado até alcançar um nó-folha (fim do caminho).

⚙️ **Funcionalidades do Sistema:**

- A árvore é criada automaticamente via `main()` com `criarSala()`.
- O jogador interage com o jogo usando `explorarSalas()`, escolhendo entre:
  - `e` → ir para a esquerda
  - `d` → ir para a direita
  - `s` → sair da exploração

📥 **Entrada** e 📤 **Saída de Dados:**

*   O usuário navega pela mansão com base nas opções exibidas no terminal.
*   O programa mostra o nome da sala visitada a cada passo.

**Simplificações para o Nível Novato:**

*   Apenas árvore binária (sem inserção ou remoção durante o jogo).
*   A árvore é montada estaticamente via código.
*   Estrutura imutável em tempo de execução.

---

## 🛡️ Nível Aventureiro: Organização de Pistas com Árvore de Busca

No nível Aventureiro, você expandirá o jogo incluindo uma **árvore de busca (BST)** para armazenar pistas encontradas.

🆕 **Diferença em relação ao Nível Novato:**

*   Agora, ao visitar certos cômodos, o jogador encontrará pistas.
*   Essas pistas são armazenadas ordenadamente em uma BST.

⚙️ **Funcionalidades do Sistema:**

*   Implementar inserção e busca de strings (pistas) na árvore de busca.
*   Permitir que o jogador visualize todas as pistas em ordem alfabética.
*   Adicionar novas pistas automaticamente ao visitar salas específicas.

📥 **Entrada** e 📤 **Saída de Dados:**

*   As pistas são cadastradas via `inserir()` ao serem encontradas.
*   O programa pode listar todas as pistas com `emOrdem()`.

**Simplificações para o Nível Intermediário:**

*   Nenhuma remoção é necessária.
*   Não é necessário balancear a árvore.
*   As pistas são strings simples (nomes curtos).

---

## 🏆 Nível Mestre: Suspeitos e Solução com Tabela Hash

No nível Mestre, você implementará a **tabela hash** para vincular pistas a **suspeitos**. Agora o jogador pode consultar quem está associado a cada pista e deduzir o culpado com base nas evidências coletadas.

🆕 **Diferença em relação ao Nível Aventureiro:**

*   Cada pista armazenada na BST será relacionada a um suspeito via tabela hash.
*   Ao final, o jogador poderá ver qual suspeito está mais associado às pistas e decidir quem é o culpado.

⚙️ **Funcionalidades do Sistema:**

*   Implementar uma tabela hash (array de ponteiros ou lista encadeada).
*   Função de inserção que relaciona pista → suspeito.
*   Permitir consulta de todas as pistas relacionadas a cada suspeito.
*   Mostrar o “suspeito mais citado” ao final da análise.

📥 **Entrada** e 📤 **Saída de Dados:**

*   As pistas e suspeitos são armazenados via `inserirNaHash(pista, suspeito)`.
*   O programa exibe as associações pista → suspeito.
*   Exibe o suspeito mais citado com base nas pistas armazenadas.

**Observações:**

*   Pode utilizar hashing simples com função de espalhamento baseada em primeiros caracteres ou soma ASCII.
*   O ideal é evitar colisões, mas, se ocorrerem, use encadeamento.

---

## 🏁 Conclusão

Ao concluir qualquer um dos níveis, você terá desenvolvido um sistema de investigação funcional em C, utilizando estruturas fundamentais como árvores e tabelas hash para controlar lógica de jogo.

Boa sorte, e divirta-se programando com **Detective Quest**!

Equipe de Ensino – Enigma Studios

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

/* ------------------------------------------------------
   Estrutura: Sala (nó de uma árvore binária)
   Cada sala tem:
   - nome
   - ponteiro para sala à esquerda
   - ponteiro para sala à direita
   ------------------------------------------------------ */
typedef struct Sala {
    char nome[50];
    struct Sala *esquerda;
    struct Sala *direita;
} Sala;

/* ------------------------------------------------------
   criarSala()
   Cria dinamicamente uma sala da mansão.
   ------------------------------------------------------ */
Sala* criarSala(const char *nome) {
    Sala *nova = (Sala*) malloc(sizeof(Sala));

    if (nova == NULL) {
        printf("Erro de alocação de memória!\n");
        exit(1);
    }

    strcpy(nova->nome, nome);
    nova->esquerda = NULL;
    nova->direita = NULL;

    return nova;
}

/* ------------------------------------------------------
   explorarSalas()
   Permite ao jogador navegar pela árvore binária.
   - e → esquerda
   - d → direita
   - s → sair
   A exploração termina quando chega a um nó-folha.
   ------------------------------------------------------ */
void explorarSalas(Sala *atual) {
    char opcao;

    while (atual != NULL) {

        printf("\nVocê está agora em: %s\n", atual->nome);

        // Caso seja um nó-folha (sem salas à esquerda e à direita)
        if (atual->esquerda == NULL && atual->direita == NULL) {
            printf("Não há mais caminhos. Fim da exploração!\n");
            return;
        }

        printf("\nPara onde deseja ir?\n");
        if (atual->esquerda != NULL)
            printf(" (e) Ir para a esquerda → %s\n", atual->esquerda->nome);
        if (atual->direita != NULL)
            printf(" (d) Ir para a direita  → %s\n", atual->direita->nome);

        printf(" (s) Sair da exploração\n");
        printf("Escolha: ");

        scanf(" %c", &opcao);

        if (opcao == 'e' || opcao == 'E') {
            if (atual->esquerda != NULL)
                atual = atual->esquerda;
            else
                printf("Não existe caminho para a esquerda!\n");
        }
        else if (opcao == 'd' || opcao == 'D') {
            if (atual->direita != NULL)
                atual = atual->direita;
            else
                printf("Não existe caminho para a direita!\n");
        }
        else if (opcao == 's' || opcao == 'S') {
            printf("Exploração encerrada!\n");
            return;
        }
        else {
            printf("Opção inválida! Tente novamente.\n");
        }
    }
}

/* ------------------------------------------------------
   liberarArvore()
   Libera toda memória usada pela árvore.
   ------------------------------------------------------ */
void liberarArvore(Sala *raiz) {
    if (raiz == NULL) return;

    liberarArvore(raiz->esquerda);
    liberarArvore(raiz->direita);
    free(raiz);
}

/* ------------------------------------------------------
   main()
   Monta a árvore binária da mansão.
   Inicia a exploração.
   ------------------------------------------------------ */
int main() {

    // Raiz da árvore (primeiro cômodo)
    Sala *hall = criarSala("Hall de Entrada");

    // Construção fixa da árvore (nível novato)
    hall->esquerda = criarSala("Sala de Estar");
    hall->direita  = criarSala("Cozinha");

    hall->esquerda->esquerda = criarSala("Quarto de Hóspedes");
    hall->esquerda->direita  = criarSala("Jardim Interno");

    hall->direita->esquerda  = criarSala("Despensa");
    hall->direita->direita   = criarSala("Garagem");

    printf("=============================================\n");
    printf("         DETECTIVE QUEST - Nível Novato\n");
    printf("     Explore a mansão e encontre o caminho!\n");
    printf("=============================================\n");

    // Iniciar exploração pela raíz
    explorarSalas(hall);

    // Liberar memória ao final
    liberarArvore(hall);

    return 0;
}
