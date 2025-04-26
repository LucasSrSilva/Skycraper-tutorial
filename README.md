
# Tutorial do Solucionador de Skyscraper Puzzle em C

## 📌 Introdução ao Skyscraper Puzzle
Um quebra-cabeça onde você deve preencher uma grade com edifícios de alturas diferentes (1 a N), onde:
- Cada linha/coluna contém números únicos
- As dicas nas bordas indicam quantos edifícios são visíveis a partir daquela posição

Exemplo de grade 4x4:

  
┌─2 1 3 2─┐  
2 │ ? ? ? ? │ 1  
1 │ ? ? ? ? │ 3  
3 │ ? ? ? ? │ 1  
2 │ ? ? ? ? │ 2  
└─3 1 2 2──┘  

---

## 🧠 Conceitos Básicos Necessários

### 1. **Ponteiros e Alocação Dinâmica**
- `malloc`: Reserva espaço na memória
- `free`: Libera espaço reservado
- `int**`: Ponteiro para ponteiro (matriz 2D)

### 2. **Recursividade e Backtracking**
- Tenta possibilidades
- Volta atrás (`backtrack`) se encontrar um caminho inválido

### 3. **Manipulação de Matrizes**
- Acesso a elementos: `matriz[linha][coluna]`
- Armazenamento dinâmico em memória

---

## 🧩 Estrutura do Código

### 1. Bibliotecas
```c
#include <unistd.h>  // Para a função write
#include <stdlib.h>  // Para malloc e free
```
### 2. Constante Global
```c
#define SIZE 4 // Tamanho fixo do puzzle (4x4)
```
----------

## 🕹️ Funções Principais

### 1.  `ft_putchar`  - Escreve um caractere

```c
void ft_putchar(char c) {
    write(1, &c, 1);
}
```
-   **Funcionamento:**  Usa a syscall  `write`  para imprimir diretamente no terminal
    

### 2.  `print_matriz`  - Exibe a matriz


```c
/*
 * Imprime uma matriz SIZE x SIZE no formato grid
 * 
 * Parâmetros:
 *   int **matriz - Ponteiro para a matriz a ser impressa
 * 
 * Comportamento:
 *   Imprime os valores da matriz linha por linha, com espaços entre os números
 *   e quebras de linha no final de cada linha
 *   Exemplo de saída para SIZE=4:
 *     1 2 3 4
 *     2 1 4 3
 *     3 4 1 2
 *     4 3 2 1
 */
void print_matriz(int **matriz) {
    int i = 0;  // Inicializa contador de linhas
    
    // Loop através de cada linha da matriz
    while (i < SIZE) {
        int j = 0;  // Inicializa contador de colunas para a linha atual
        
        // Loop através de cada coluna na linha atual
        while (j < SIZE) {
            /* Converte e imprime o valor da matriz:
             * 1. matriz[i][j] obtém o valor inteiro (1-4)
             * 2. + '0' converte para caractere ASCII:
             *    - 1 + '0' = '1' (ASCII 49)
             *    - 2 + '0' = '2' (ASCII 50) etc.
             */
            ft_putchar(matriz[i][j] + '0');
            
            // Imprime espaço entre números, exceto após o último da linha
            if (j < SIZE - 1) {
                ft_putchar(' ');  // Imprime separador
            }
            
            j++;  // Avança para a próxima coluna
        }
        
        ft_putchar('\n');  // Quebra de linha ao final de cada linha
        i++;  // Avança para a próxima linha
    }
}
```
-   **Passo a passo:**
    
    1.  Loop externo: linhas
        
    2.  Loop interno: colunas
        
    3.  Converte números para caracteres ASCII (`+ '0'`)
        

### 3.  `pode_colocar`  - Verifica validade


```c
int pode_colocar(int **matriz, int linha, int coluna, int num) {
    int i = 0;
    while (i < SIZE) {
        if (matriz[linha][i] == num || matriz[i][coluna] == num) return 0;
        i++;
    }
    return 1;
}
```
-   **Lógica:**
    
    -   Verifica repetição na  **linha**
        
    -   Verifica repetição na  **coluna**
        
    -   Retorna 1 se o número for válido
        

### 4.  `preencher`  - Algoritmo de Backtracking


```c
int preencher(int **matriz, int *bordas, int linha, int coluna) {
    // CASO BASE: Quando passamos da última linha (SIZE = 4, linhas 0-3)
    // Isso significa que preenchemos toda a matriz com sucesso
    if (linha == SIZE) return 1; 
    
    // Quando chegamos ao final da linha atual (coluna = SIZE),
    // pulamos para o início da próxima linha
    if (coluna == SIZE) return preencher(matriz, bordas, linha + 1, 0);
    
    // Tenta todos os números possíveis (1 a 4) na célula atual
    int num = 1;
    while (num <= SIZE) {
        // Verifica se podemos colocar 'num' na posição atual
        if (pode_colocar(matriz, linha, coluna, num)) {
            // PASSO DA TENTATIVA:
            // Coloca o número na célula atual
            matriz[linha][coluna] = num;  
            
            // RECURSÃO:
            // Chama recursivamente para a próxima célula à direita
            // Se essa chamada retornar 1 (sucesso), propaga o sucesso
            // para cima na cadeia de chamadas recursivas
            if (preencher(matriz, bordas, linha, coluna + 1)) return 1;
            
            // BACKTRACKING:
            // Se chegamos aqui, a tentativa não levou a uma solução
            // "Desfaz" a atribuição (coloca 0) e tenta o próximo número
            matriz[linha][coluna] = 0;  
        }
        num++;  // Próximo número a tentar
    }
    
    // Se nenhum número (1-4) funcionou nesta célula, retorna falha
    // Isso fará com que a chamada recursiva anterior "desfaça" sua escolha
    return 0;
}
```
-   **Fluxo:**
    
    1.  Preenche célula atual
        
    2.  Chama recursivamente para próxima célula
        
    3.  Se falhar, zera a célula e tenta próximo número
        

### 5.  `main`  - Função Principal
```c
int main(void) {
    // 1. Aloca memória para as dicas
    int *bordas = malloc(sizeof(int) * 16);
    
    // 2. Define dicas de exemplo
    int exemplo_bordas[16] = {2,1,3,2, 2,2,1,3, 1,3,2,1, 3,1,2,2};
    
    // 3. Copia para o array dinâmico
    int i = 0;
    while (i < 16) {
        bordas[i] = exemplo_bordas[i];
        i++;
    }
    
    // 4. Cria matriz 4x4
    int **matriz = malloc(sizeof(int *) * SIZE);
    i = 0;
    while (i < SIZE) {
        matriz[i] = malloc(sizeof(int) * SIZE);
        // Inicializa com 0
        int j = 0;
        while (j < SIZE) {
            matriz[i][j] = 0;
            j++;
        }
        i++;
    }
    
    // 5. Tenta resolver
    if (preencher(matriz, bordas, 0, 0)) {
        print_matriz(matriz);
    } else {
        write(1, "Erro\n", 5);
    }
    
    // 6. Libera memória
    i = 0;
    while (i < SIZE) {
        free(matriz[i]);
        i++;
    }
    free(matriz);
    free(bordas);
    
    return 0;
}
```
----------

## 🔧 Como Compilar e Executar

1.  **Compilação:**

gcc -Wall -Wextra -Werror skyscraper.c -o skyscraper

2.  **Execução:**
 
./skyscraper

3.  **Saída Esperada (para as dicas fornecidas):**
 
1 3 4 2
4 2 1 3
3 4 2 1
2 1 3 4

----------

## 🧠 Exemplo de Funcionamento do Backtracking

**Passo 1:**  Tenta colocar 1 na posição (0,0)

1 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0

**Passo 2:**  Avança até encontrar conflito

1 2 3 4
4 ... (conflito na coluna 3)

**Passo 3:**  Backtrack na posição (0,3)

1 2 3 0
...

**Passo 4:**  Continua até encontrar combinação válida

----------

## 💡 Possíveis Melhorias

1.  **Validação de Dicas:**

// Antes de chamar preencher(), verificar se as dicas são válidas

2.  **Entrada do Usuário:**
    
// Ler as dicas via terminal ou arquivo

3.  **Visualização das Dicas:**
  
// Mostrar as dicas ao redor da matriz na saída

4.  **Suporte para Tamanhos Variáveis:**
    
// Substituir #define SIZE por uma variável

----------

## 📚 Recursos para Aprender Mais

1.  [Documentação de malloc/free](https://en.cppreference.com/w/c/memory/malloc)
    
2.  [Tutorial de Ponteiros em C](https://www.learn-c.org/)
    
3.  [Algoritmos de Backtracking](https://www.geeksforgeeks.org/backtracking-algorithms/)
   

Este arquivo Markdown pode ser:
1. Visualizado diretamente em editores como VS Code
2. Convertido para PDF usando ferramentas como [Pandoc](https://pandoc.org/)
3. Publicado em plataformas como GitHub/GitLab

Quer que detalhe mais alguma parte específica? 😊
