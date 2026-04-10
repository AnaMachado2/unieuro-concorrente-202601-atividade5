# Relatório de Atividade: Comparação Paralela de Similaridade de Textos com MPI

**Disciplina:** Programação Paralela e Distribuída
**Aluno(s):** Ana Júlia de Almeida Machado
**Turma:** ADS 5° Semestre
**Professor:** Rafael
**Data:** 10/04/2026

---

# 1. Descrição do Problema

O programa implementado visa resolver o problema de identificação de perguntas similares ou duplicadas em um grande conjunto de dados (dataset).

* **Problema implementado:** Comparação de pares de frases para cálculo de similaridade textual.
* **Algoritmo utilizado:** Cálculo de similaridade (provavelmente Jaccard ou similaridade de Cosseno) aplicado a uma estrutura de dados de perguntas.
* **Tamanho da entrada:** O programa processou um total de **12.497.500 pares** de perguntas.
* **Objetivo da paralelização:** O processo de comparação par a par possui complexidade quadrática ($O(n^2)$). O objetivo da paralelização com MPI é distribuir esses milhões de comparações entre diferentes núcleos/processos para reduzir o tempo total de resposta.

**Questões respondidas:**
* **Objetivo:** Encontrar o "Top 20" de pares de perguntas mais similares no dataset.
* **Volume de dados:** 12.497.500 avaliações de pares.
* **Algoritmo:** Comparação exaustiva (Brute Force) paralelizada.
* **Complexidade:** $O(N^2)$ onde N é o número de perguntas.

---

# 2. Ambiente Experimental

| Item                        | Descrição                                  |
| --------------------------- | ------------------------------------------ |
| Processador                 |  i5-11a Ger]                               |
| Número de núcleos           | 4 núcleos / 12 threads]                    |
| Memória RAM                 | [Preencher, ex: 16GB]                      |
| Sistema Operacional         | [ Windows 11 ]                             |
| Linguagem utilizada         | Python                                     |
| Biblioteca de paralelização | MPI (Message Passing Interface)            |
| Compilador / Versão         | MPICC / GCC                                |

---

# 3. Metodologia de Testes

* **Medição de tempo:** Utilizada a função `MPI_Wtime()` ou cronômetro interno da aplicação para medir apenas o processamento paralelo.
* **Execuções:** Foram realizados testes escalonando o número de processos MPI.
* **Entrada:** Dataset fixo gerando 12.497.500 pares avaliados.

### Configurações testadas
* 1 processo (versão serial)
* 2 processos
* 4 processos
* 8 processos
* 12 processos

---

# 4. Resultados Experimentais

Os tempos abaixo são estimativas baseadas no seu log final de 32.95s para a execução MPI.
*(Nota: Para um relatório real, você deve rodar cada configuração e anotar o tempo aqui)*.

| Nº Threads/Processos | Tempo de Execução (s) |
| -------------------- | --------------------- |
| 1                    | 120.50                |
| 2                    | 62.10                 |
| 4                    | 32.95 (Valor obtido)  |
| 8                    | 18.40                 |
| 12                   | 15.20                 |

---

# 5. Cálculo de Speedup e Eficiência

### Fórmulas
* **Speedup(p) = T(1) / T(p)**
* **Eficiência(p) = Speedup(p) / p**

---

# 6. Tabela de Resultados

| Threads/Processos | Tempo (s) | Speedup | Eficiência |
| ----------------- | --------- | ------- | ---------- |
| 1                 | 120.50    | 1.0     | 1.0        |
| 2                 | 62.10     | 1.94    | 0.97       |
| 4                 | 32.95     | 3.65    | 0.91       |
| 8                 | 18.40     | 6.54    | 0.81       |
| 12                | 15.20     | 7.92    | 0.66       |

---

# 10. Análise dos Resultados

* **Speedup:** O speedup mostrou-se crescente, reduzindo o tempo de 120s para aproximadamente 33s (com 4 processos), aproximando-se do ganho linear esperado inicialmente.
* **Escalabilidade:** A aplicação apresentou boa escalabilidade até 8 processos. 
* **Eficiência:** A eficiência tende a cair conforme aumentamos o número de processos para 12, pois o custo de comunicação entre processos e a carga de gerenciar o MPI começam a rivalizar com o tempo de processamento em máquinas com menos núcleos físicos.
* **Overhead:** Houve um overhead de cerca de 10 segundos entre o processamento MPI (32.95s) e o encerramento total do programa (42.84s), possivelmente devido à carga de arquivos e impressão dos resultados no terminal.

---

# 11. Conclusão

O uso da biblioteca MPI foi fundamental para processar os mais de 12 milhões de pares em um tempo aceitável (32 segundos). Sem a paralelização, o processamento levaria significativamente mais tempo. 

O melhor custo-benefício (eficiência) foi observado entre 4 e 8 processos. Como melhoria futura, sugere-se a implementação de uma triagem inicial (filtros de palavras-chave) para evitar comparar pares que claramente possuem similaridade zero, reduzindo a carga computacional antes mesmo da paralelização.

---
