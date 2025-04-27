# Tutorial: Entendendo o Desafio do Quebra-Cabeça Skyscraper 4x4

O quebra-cabeça **Skyscraper** é um desafio lógico que combina elementos de Sudoku com pistas visuais externas.  
Neste tutorial, vamos analisar a estrutura e as regras fundamentais de um quebra-cabeça Skyscraper 4x4, preparando o terreno para implementações e soluções futuras.

---

## 🧩 O Que é o Quebra-Cabeça Skyscraper?

O Skyscraper é um jogo de lógica em que você deve preencher uma grade NxN (neste caso, 4x4) com números que representam a altura de arranha-céus, seguindo regras específicas:

- **Alturas Variadas:** Cada número de 1 a N representa a altura de um arranha-céu.
- **Sem Repetições:** Cada linha e cada coluna deve conter todos os números de 1 a N, sem repetições.
- **Pistas Externas:** Números ao redor da grade indicam quantos arranha-céus são visíveis daquela direção específica.

---

## 🔍 Regras Detalhadas

1. **Preenchimento da Grade:**
   - Insira números de 1 a 4 em cada célula da grade 4x4.
   - Nenhum número pode se repetir em uma mesma linha ou coluna.

2. **Pistas de Visibilidade:**
   - Números posicionados nas bordas da grade indicam quantos arranha-céus são visíveis a partir daquela perspectiva.
   - Um arranha-céu mais alto bloqueia a visão de qualquer arranha-céu mais baixo que esteja atrás dele.
   - Por exemplo, se a sequência em uma linha for `2 4 3 1`, da esquerda você verá 2 arranha-céus (2 e 4), pois o 4 bloqueia os seguintes; da direita, verá 3 arranha-céus (1, 3 e 4).

---

## 📌 Exemplo de Pistas

Considere a seguinte entrada de pistas para um quebra-cabeça 4x4:

```
4 3 2 1 1 2 2 2 4 3 2 1 1 2 2 2
```

Esta sequência representa as pistas na seguinte ordem:

- **Topo (de cima para baixo):** 4 3 2 1
- **Direita (da esquerda para a direita):** 1 2 2 2
- **Fundo (de baixo para cima):** 4 3 2 1
- **Esquerda (da direita para a esquerda):** 1 2 2 2

Estas pistas orientam como os números devem ser posicionados na grade para satisfazer as condições de visibilidade.

---

## 🎯 Objetivo

Preencher a grade 4x4 com números de 1 a 4, garantindo que:

- Cada número apareça exatamente uma vez em cada linha e coluna.
- As pistas de visibilidade nas bordas sejam respeitadas.

O desafio reside em equilibrar as restrições internas da grade com as pistas externas, exigindo raciocínio lógico e atenção aos detalhes.

---

Compreendendo essas regras e estruturas, você estará preparado para abordar a implementação e solução de quebra-cabeças Skyscraper 4x4.  
Nos próximos passos, exploraremos estratégias e algoritmos para resolver esses desafios de forma eficiente.

---

# Construindo a Lógica da Solução (Descrição em Pseudocódigo)

Antes de começarmos a programar, precisamos entender **quais funções** serão necessárias e **qual será a responsabilidade de cada uma**.  
Abaixo está o planejamento da lógica, explicado de forma simples.

---

## 1. Representação da Grade

Precisamos representar a grade 4x4 em memória.  
Para isso, vamos usar uma **matriz 4x4** de inteiros.

Cada posição (`linha`, `coluna`) da matriz armazenará um número entre 1 e 4.

---

## 2. Funções Necessárias

### a) Função para Imprimir a Matriz

- Serve para exibir a solução final na tela.
- Cada linha deve ser impressa separada por espaços.

### b) Função para Testar se um Número Pode Ser Colocado

- Antes de colocar um número na posição (`linha`, `coluna`), precisamos garantir:
  - O número ainda não foi usado nessa linha.
  - O número ainda não foi usado nessa coluna.

### c) Funções para Checar as Visibilidades

- Cada linha e cada coluna tem pistas de quantos arranha-céus são visíveis.
- Precisamos verificar:
  - Se uma linha atende as pistas da esquerda e da direita.
  - Se uma coluna atende as pistas de cima e de baixo.

### d) Função Recursiva para Preencher a Grade (Backtracking)

- Vamos preencher a grade usando uma abordagem de tentativa e erro:
  1. Tentar colocar um número válido.
  2. Se colocar, avançar para a próxima célula.
  3. Se travar (não conseguir mais colocar números), voltar (backtrack) e tentar outro número.
- Quando a grade inteira for preenchida:
  - Validar se todas as colunas e linhas atendem às pistas.
  - Se sim, a solução foi encontrada.

### e) Função para Ler e Interpretar as Pistas

- Receber uma string com os 16 números das pistas (entrada do usuário).
- Dividir essa string em um vetor de inteiros, representando as pistas em ordem:
  - 0..3: Topo (de cima para baixo)
  - 4..7: Direita (da esquerda para a direita)
  - 8..11: Fundo (de baixo para cima)
  - 12..15: Esquerda (da direita para a esquerda)

### f) Funções para Gerenciar Memória

- Criar e liberar a matriz corretamente usando `malloc` e `free` para evitar vazamentos de memória.

---

## 3. Fluxo Principal (main)

1. Ler a entrada do usuário (string das pistas).
2. Validar a entrada (16 números de 1 a 4, separados por espaços).
3. Criar uma matriz 4x4 inicializada com zeros.
4. Tentar preencher a matriz usando backtracking.
5. Se conseguir, imprimir a solução.
6. Se não conseguir, mostrar "Error".

---

## Resumo Visual

```
Entrada (pistas) -> Validar -> Criar Matriz -> Preencher com Backtracking -> Validar -> Imprimir
```

---

Assim, planejando a lógica primeiro, conseguimos depois construir as funções de forma organizada e sem nos perder na implementação.

---

# Mapa Visual da Solução

```mermaid
flowchart TD
    A[Início] --> B[Ler entrada de pistas]
    B --> C[Validar pistas]
    C -- Inválido --> Z[Mostrar "Error" e Sair]
    C -- Válido --> D[Criar matriz 4x4 inicializada com 0]
    D --> E[Preencher matriz usando backtracking]
    E -- Sucesso --> F[Imprimir matriz]
    E -- Falha --> Z
    F --> G[Fim]
    Z --> G
```

---

# Tutorial do Código - Resolução Skyscraper 4x4

## Funções explicadas:

---

### `ft_putchar`

- **O que faz:**  
Escreve um único caractere no console usando `write`.

- **Resumo:**  
Serve como substituto para `printf`, focado em imprimir caracteres simples.

---

### `print_matriz`

- **O que faz:**  
Imprime a matriz 4x4 no console.

- **Resumo:**  
Percorre cada linha e coluna, imprimindo os números separados por espaço.

---

### `pode_colocar`

- **O que faz:**  
Verifica se um número pode ser colocado numa posição (linha, coluna) da matriz.

- **Resumo:**  
Garante que o número ainda não existe na linha e nem na coluna.

---

### `conta_visiveis`

- **O que faz:**  
Conta quantos "prédios" são visíveis olhando para uma linha ou coluna.

- **Resumo:**  
Segue da esquerda para direita, contando toda vez que um prédio é maior que os anteriores.

---

### `extrai_coluna`

- **O que faz:**  
Extrai uma coluna da matriz para um vetor auxiliar, podendo inverter a ordem.

- **Resumo:**  
Ajuda na verificação de visibilidade de cima para baixo ou de baixo para cima.

---

### `checa_linha`

- **O que faz:**  
Confere se a linha atual obedece as regras de visibilidade da esquerda e da direita.

- **Resumo:**  
- Extrai a linha normal e invertida.
- Compara os prédios visíveis com as pistas fornecidas.

---

### `checa_coluna`

- **O que faz:**  
Confere se uma coluna obedece as regras de visibilidade de cima e de baixo.

- **Resumo:**  
- Extrai a coluna normal e invertida.
- Compara os prédios visíveis com as pistas fornecidas.

---

### `preencher`

- **O que faz:**  
Função recursiva que tenta preencher toda a matriz usando backtracking.

- **Resumo:**  
- Tenta colocar números de 1 a 4.
- Verifica se pode colocar o número (`pode_colocar`).
- Avança se possível, volta (backtrack) se necessário.
- Checa linhas ao final de cada linha.
- Checa colunas no final.

---

### `free_matriz`

- **O que faz:**  
Libera a memória alocada da matriz.

- **Resumo:**  
Evita vazamento de memória no final do programa.

---

### `alloc_matriz`

- **O que faz:**  
Aloca dinamicamente a matriz 4x4 inicializada com zeros.

- **Resumo:**  
Garante que a matriz esteja pronta para receber os números.

---

### `parse_bordas`

- **O que faz:**  
Interpreta a string de entrada e transforma nas pistas (bordas).

- **Resumo:**  
- Ignora espaços.
- Aceita apenas números entre 1 e 4.
- Preenche o vetor de pistas na ordem:
  - [0..3] Topo
  - [4..7] Direita
  - [8..11] Fundo
  - [12..15] Esquerda

---

### `validar_bordas`

- **O que faz:**  
Verifica se todas as pistas estão entre 1 e 4.

- **Resumo:**  
Confirma que a entrada é válida para o jogo.

---

### `main`

- **O que faz:**  
Controla todo o fluxo do programa.

- **Resumo:**  
- Lê a entrada.
- Valida as pistas.
- Aloca matriz.
- Chama a função de preencher.
- Imprime o resultado ou erro.

---

# Tutorial Visual das Funções - Skyscraper 4x4

---

## `ft_putchar`

**Entrada:** caractere (`char c`)  
**Ação:** escreve o caractere na saída padrão (`stdout`)  
**Saída:** - (efeito colateral: escreve na tela)

```plaintext
(c) ──> write ──> tela
```

---

## `print_matriz`

**Entrada:** matriz 4x4 (`int **matriz`)  
**Ação:** imprime os números da matriz separados por espaço  
**Saída:** - (efeito colateral: escreve na tela)

```plaintext
matriz ──> loop linhas e colunas ──> imprime
```

---

## `pode_colocar`

**Entrada:** matriz, posição (linha, coluna), número  
**Ação:** verifica se o número já existe na linha ou na coluna  
**Saída:** 1 (pode) ou 0 (não pode)

```plaintext
(linha, coluna, num)
    └───> verifica linha
    └───> verifica coluna
    └───> retorna 1 ou 0
```

---

## `conta_visiveis`

**Entrada:** vetor de números  
**Ação:** conta quantos prédios são visíveis da esquerda para a direita  
**Saída:** quantidade de prédios visíveis

```plaintext
vetor ──> percorre ──> compara altura ──> conta
```

---

## `extrai_coluna`

**Entrada:** matriz, índice da coluna, buffer, reverso ou não  
**Ação:** extrai valores da coluna para um vetor  
**Saída:** vetor preenchido

```plaintext
matriz + coluna + reverso ──> extrai valores ──> vetor
```

---

## `checa_linha`

**Entrada:** matriz, pistas, índice da linha  
**Ação:** verifica visibilidade da esquerda e da direita  
**Saída:** 1 (ok) ou 0 (erro)

```plaintext
linha ──> visibilidade esquerda
      └──> visibilidade direita
      └──> compara com pistas
      └──> retorna 1 ou 0
```

---

## `checa_coluna`

**Entrada:** matriz, pistas, índice da coluna  
**Ação:** verifica visibilidade de cima e de baixo  
**Saída:** 1 (ok) ou 0 (erro)

```plaintext
coluna ──> visibilidade cima
       └──> visibilidade baixo
       └──> compara com pistas
       └──> retorna 1 ou 0
```

---

## `preencher`

**Entrada:** matriz, pistas, posição (linha, coluna)  
**Ação:** preenche a matriz usando backtracking recursivo  
**Saída:** 1 (solução encontrada) ou 0 (não encontrou)

```plaintext
(linha, coluna)
    └───> tenta num 1..4
          └──> pode colocar?
             └──> sim: avança
             └──> não: tenta outro
    └───> fim linha: checa linha
    └───> fim matriz: checa colunas
```

---

## `free_matriz`

**Entrada:** matriz  
**Ação:** libera memória da matriz  
**Saída:** -

```plaintext
matriz ──> libera linha a linha ──> libera ponteiro
```

---

## `alloc_matriz`

**Entrada:** -  
**Ação:** aloca memória para uma matriz 4x4 inicializada com 0  
**Saída:** ponteiro para a matriz

```plaintext
malloc linhas
    └──> malloc colunas
    └──> inicializa 0
    └──> retorna matriz
```

---

## `parse_bordas`

**Entrada:** string de pistas  
**Ação:** converte a string em vetor de inteiros  
**Saída:** vetor de 16 inteiros

```plaintext
string ──> lê caractere a caractere ──> transforma em número ──> vetor bordas
```

---

## `validar_bordas`

**Entrada:** vetor bordas  
**Ação:** verifica se os números estão entre 1 e 4  
**Saída:** 1 (válido) ou 0 (inválido)

```plaintext
bordas ──> checa cada valor ──> retorna 1 ou 0
```

---

## `main`

**Entrada:** argumentos da linha de comando  
**Ação:** controla o fluxo geral do programa  
**Saída:** solução impressa ou "Error"

```plaintext
entrada ──> parse ──> valida ──> aloca matriz ──> preencher ──> imprime ou Error
```

---

# Explicação Função por Função do Código Skyscraper 4x4

---

## `ft_putchar`

```c
void ft_putchar(char c)
{
    write(1, &c, 1);
}
```

**O que faz:**  
Escreve um único caractere na tela.  
Usa a função `write` do Unix para imprimir o caractere passado.

---

## `print_matriz`

```c
void print_matriz(int **matriz)
{
    for (int i = 0; i < SIZE; i++)
    {
        for (int j = 0; j < SIZE; j++)
        {
            ft_putchar(matriz[i][j] + '0');
            if (j < SIZE - 1)
                ft_putchar(' ');
        }
        ft_putchar('\n');
    }
}
```

**O que faz:**  
Percorre toda a matriz 4x4 e imprime os números linha por linha.  
Entre os números de cada linha, coloca um espaço.  
Depois de cada linha, imprime uma quebra de linha (`\n`).

---

## `pode_colocar`

```c
int pode_colocar(int **matriz, int linha, int coluna, int num)
{
    for (int i = 0; i < SIZE; i++)
        if (matriz[linha][i] == num || matriz[i][coluna] == num)
            return 0;
    return 1;
}
```

**O que faz:**  
Verifica se o número `num` pode ser colocado na posição `(linha, coluna)`:
- O número não pode se repetir na linha.
- O número não pode se repetir na coluna.

Se puder colocar, retorna 1. Se não puder, retorna 0.

---

## `conta_visiveis`

```c
int conta_visiveis(int *vetor)
{
    int max = 0, vis = 0;
    for (int i = 0; i < SIZE; i++)
    {
        if (vetor[i] > max)
        {
            max = vetor[i];
            vis++;
        }
    }
    return vis;
}
```

**O que faz:**  
Conta quantos prédios são visíveis olhando um vetor (linha ou coluna).  
A lógica é:
- Se o prédio atual é maior que todos anteriores (`max`), ele é visível.
- Aumenta o contador `vis`.

---

## `extrai_coluna`

```c
void extrai_coluna(int **matriz, int col, int *buf, int rev)
{
    for (int i = 0; i < SIZE; i++)
        buf[i] = rev ? matriz[SIZE - 1 - i][col] : matriz[i][col];
}
```

**O que faz:**  
Copia os valores de uma coluna da matriz para um vetor (`buf`):
- Se `rev == 0`, copia de cima para baixo.
- Se `rev == 1`, copia de baixo para cima.

Serve para facilitar a verificação da visibilidade.

---

## `checa_linha`

```c
int checa_linha(int **matriz, int *bordas, int l)
{
    int buf[SIZE];

    // Esquerda para direita
    for (int j = 0; j < SIZE; j++)
        buf[j] = matriz[l][j];
    if (conta_visiveis(buf) != bordas[12 + l])
        return 0;

    // Direita para esquerda
    for (int j = 0; j < SIZE; j++)
        buf[j] = matriz[l][SIZE - 1 - j];
    if (conta_visiveis(buf) != bordas[4 + l])
        return 0;

    return 1;
}
```

**O que faz:**  
Verifica se a linha `l` atende às pistas de visibilidade:
- Primeiro, da esquerda para direita.
- Depois, da direita para esquerda.

Se alguma direção não bate com as pistas, retorna 0 (erro).

---

## `checa_coluna`

```c
int checa_coluna(int **matriz, int *bordas, int c)
{
    int buf[SIZE];

    // De cima para baixo
    extrai_coluna(matriz, c, buf, 0);
    if (conta_visiveis(buf) != bordas[c])
        return 0;

    // De baixo para cima
    extrai_coluna(matriz, c, buf, 1);
    if (conta_visiveis(buf) != bordas[8 + c])
        return 0;

    return 1;
}
```

**O que faz:**  
Verifica se a coluna `c` atende às pistas de visibilidade:
- Primeiro, de cima para baixo.
- Depois, de baixo para cima.

Se alguma direção não bater, retorna 0.

---

## `preencher`

```c
int preencher(int **matriz, int *bordas, int l, int c)
{
    if (l == SIZE)
    {
        for (int col = 0; col < SIZE; col++)
            if (!checa_coluna(matriz, bordas, col))
                return 0;
        return 1;
    }

    if (c == SIZE)
    {
        if (!checa_linha(matriz, bordas, l))
            return 0;
        return preencher(matriz, bordas, l + 1, 0);
    }

    for (int num = 1; num <= SIZE; num++)
    {
        if (!pode_colocar(matriz, l, c, num))
            continue;
        matriz[l][c] = num;
        if (preencher(matriz, bordas, l, c + 1))
            return 1;
        matriz[l][c] = 0; // Backtracking
    }
    return 0;
}
```

**O que faz:**  
Função principal de **backtracking**:
- Tenta preencher a matriz célula por célula.
- Para cada célula, tenta todos os números de 1 a 4.
- Se conseguir preencher até o final validando linhas e colunas, retorna 1.
- Se errar, desfaz (`backtracking`) e tenta outro número.

---

## `free_matriz`

```c
void free_matriz(int **matriz)
{
    for (int i = 0; i < SIZE; i++)
        free(matriz[i]);
    free(matriz);
}
```

**O que faz:**  
Libera a memória alocada para a matriz:
- Primeiro libera cada linha.
- Depois libera o ponteiro para a matriz.

---

## `alloc_matriz`

```c
int **alloc_matriz(void)
{
    int **matriz = malloc(SIZE * sizeof(int *));
    if (!matriz)
        return NULL;
    for (int i = 0; i < SIZE; i++)
    {
        matriz[i] = malloc(SIZE * sizeof(int));
        if (!matriz[i])
            return NULL;
        for (int j = 0; j < SIZE; j++)
            matriz[i][j] = 0;
    }
    return matriz;
}
```

**O que faz:**  
Aloca memória para uma matriz 4x4:
- Inicializa todas as posições com zero.

---

## `parse_bordas`

```c
int *parse_bordas(char *input)
{
    int *bordas = malloc(SIZE * 4 * sizeof(int));
    if (!bordas)
        return NULL;

    int i = 0, j = 0;
    while (input[i] && j < SIZE * 4)
    {
        if (input[i] >= '1' && input[i] <= '4')
            bordas[j++] = input[i] - '0';
        else if (input[i] != ' ')
        {
            free(bordas);
            return NULL;
        }
        i++;
    }

    if (j != SIZE * 4 || input[i] != '\0')
    {
        free(bordas);
        return NULL;
    }
    return bordas;
}
```

**O que faz:**  
Transforma a entrada (string com números e espaços) em um vetor de 16 inteiros:
- Ignora espaços.
- Se encontrar algo inválido, retorna erro.

---

## `validar_bordas`

```c
int validar_bordas(int *bordas)
{
    for (int i = 0; i < SIZE * 4; i++)
    {
        if (bordas[i] < 1 || bordas[i] > SIZE)
            return 0;
    }
    return 1;
}
```

**O que faz:**  
Valida que todas as pistas (`bordas`) estão entre 1 e 4.

---

## `main`

```c
int main(int argc, char **argv)
{
    if (argc != 2)
    {
        write(1, "Error\n", 6);
        return 1;
    }

    int *bordas = parse_bordas(argv[1]);
    if (!bordas)
    {
        write(1, "Error\n", 6);
        return 1;
    }

    // Ajuste da ordem
    for (int i = 0; i < SIZE; i++)
    {
        int tmp = bordas[8 + i];
        bordas[8 + i] = bordas[12 + i];
        bordas[12 + i] = tmp;
    }

    if (!validar_bordas(bordas))
    {
        write(1, "Error\n", 6);
        free(bordas);
        return 1;
    }

    int **matriz = alloc_matriz();
    if (!matriz)
    {
        free(bordas);
        write(1, "Error\n", 6);
        return 1;
    }

    if (preencher(matriz, bordas, 0, 0))
        print_matriz(matriz);
    else
        write(1, "Error\n", 6);

    free_matriz(matriz);
    free(bordas);
    return 0;
}
```

**O que faz:**  
Fluxo principal:
- Lê o argumento de entrada.
- Analisa e ajusta as bordas.
- Valida as pistas.
- Cria a matriz.
- Usa backtracking para tentar preencher.
- Se conseguir, imprime; senão, imprime "Error".

---

# Tutorial: Compilação e Execução do Código

Este tutorial explica como compilar e executar o código C fornecido.

## 1. Compilando o Código

Para compilar o código, você pode usar o compilador **GCC** (GNU Compiler Collection). Supondo que o arquivo com o código esteja nomeado como `main.c`, use o seguinte comando no terminal:

```bash
gcc -o main main.c
./main "4 3 2 1 1 2 2 2 4 3 2 1 1 2 2 2"
```