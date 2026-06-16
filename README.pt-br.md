# Desafio DIO — Engenharia Reversa e Especialização em C/Assembly

## Sobre o Projeto

Este projeto foi desenvolvido como parte de um desafio da DIO (Digital Innovation One) e consiste na criação de um caderno temático no NotebookLM voltado para estudos avançados de Engenharia Reversa, Arquitetura de Computadores e a relação entre as linguagens C e Assembly (x86/x64).

O objetivo principal é compreender como softwares funcionam internamente, analisando a transição entre código-fonte de alto nível e instruções executadas diretamente pelo processador.

---

## Objetivos de Aprendizagem

Durante o desenvolvimento deste estudo, os seguintes objetivos foram definidos:

* Compreender o funcionamento interno de binários executáveis.
* Entender como o código escrito em C é traduzido para Assembly.
* Estudar fluxo de execução, registradores e gerenciamento de memória.
* Aprender convenções de chamada (*calling conventions*) utilizadas em arquiteturas modernas.
* Analisar otimizações realizadas por compiladores.
* Investigar o impacto de mecanismos modernos de proteção e segurança em processos de engenharia reversa.
* Desenvolver proficiência no uso de ferramentas amplamente utilizadas na área.

---

## Ferramentas Estudadas

### Ghidra

Ferramenta de engenharia reversa desenvolvida pela NSA, utilizada para análise estática e decompilação de binários.

### IDA Free

Versão gratuita do Interactive DisAssembler, uma das ferramentas mais tradicionais da área.

### Cutter

Interface gráfica baseada no Radare2 que simplifica tarefas de análise reversa.

### GDB

Depurador amplamente utilizado em ambientes Linux para análise dinâmica de programas.

---

## Referências Bibliográficas

As seguintes fontes foram utilizadas como base para o estudo:

### Computer Systems: A Programmer's Perspective (CS:APP)

**Autores:** Randal E. Bryant e David R. O'Hallaron

Obra fundamental para compreender a interação entre hardware, sistema operacional e software.

### The Art of Assembly Language

**Autor:** Randall Hyde

Referência clássica para o estudo de Assembly e arquitetura de processadores.

### Linux System Programming: Talking Directly to the Kernel

**Autor:** Robert Love

Livro voltado ao entendimento de chamadas de sistema (*syscalls*) e da interação entre aplicações e o kernel Linux.

---

## Engenharia de Prompts

### Prompt Principal

> Explique o funcionamento interno do programa abaixo passo a passo, identificando fluxo de execução, chamadas de funções, estruturas de dados, uso de memória, registradores, convenções de chamada, algoritmos utilizados e possíveis otimizações do compilador. Sempre apresente exemplos práticos em C e Assembly x86/x64, diagramas ASCII quando necessário e explique como reproduzir a análise utilizando Ghidra, IDA Free, Cutter e GDB. Ao encontrar funções desconhecidas, descreva hipóteses fundamentadas sobre seu comportamento e o raciocínio utilizado para chegar à conclusão. O objetivo é aprendizado profundo de engenharia reversa e compreensão do funcionamento interno do software.

---

## Desafios e Aprendizados

### Desafio

Diferenciar o papel do ponto de entrada real do programa (`_start`) em relação à função `main()` normalmente utilizada em aplicações escritas em C.

### Aprendizado

Foi identificado que:

* `_start` é o verdadeiro ponto de entrada do binário.
* A função `main()` é apenas uma convenção fornecida pela biblioteca padrão da linguagem C.
* Antes da execução do `main()`, ocorre a inicialização do ambiente pelo **C Runtime Startup (CRT)**.

### Insight Obtido

A análise de registradores, especialmente durante chamadas de sistema, mostrou-se uma das técnicas mais eficientes para compreender comportamentos ocultos ou otimizações realizadas pelo compilador.

---

## Fundamentos de Engenharia Reversa

A engenharia reversa consiste na reconstrução da lógica e do comportamento de um software quando seu código-fonte não está disponível.

### Processo de Tradução C → Assembly

#### Compilação

O código escrito em C é convertido em instruções Assembly executáveis pela CPU.

#### Uso de Registradores

Variáveis e parâmetros podem ser armazenados diretamente em registradores ou em posições de memória.

#### Chamadas de Sistema (Syscalls)

As syscalls representam a interface direta entre programas e o kernel do sistema operacional.

No Linux x86_64:

| Registrador | Função            |
| ----------- | ----------------- |
| RAX         | Número da syscall |
| RDI         | 1º argumento      |
| RSI         | 2º argumento      |
| RDX         | 3º argumento      |

#### Otimizações de Compilador

Compiladores modernos podem:

* Remover código redundante.
* Eliminar funções.
* Reorganizar instruções.
* Substituir operações por versões mais eficientes.

Essas otimizações tornam a leitura do Assembly significativamente mais complexa.

---

## Glossário

### Syscall

Interface entre uma aplicação e o kernel do sistema operacional.

### Registradores

Pequenas áreas de memória extremamente rápidas localizadas dentro da CPU.

Exemplos:

* RAX
* RBX
* RCX
* RDX
* RDI
* RSI

### Convenção de Chamada

Conjunto de regras que define como funções recebem parâmetros e retornam valores.

### Seção `.data`

Área do binário utilizada para armazenamento de variáveis globais e dados estáticos.

### Decompilação

Processo de reconstrução de uma representação de alto nível a partir de código de máquina.

---

## Prompts Reutilizáveis

### Reconstrução de Funções

> Analise o seguinte bloco de código Assembly e tente reconstruir a assinatura da função equivalente em C, identificando se os parâmetros são passados via pilha ou registradores.

### Análise de Syscalls

> Explique a diferença entre uma chamada de função (`call`) e uma chamada de sistema (`syscall`) neste trecho de código.

### Identificação de Proteções

> Identifique neste binário quaisquer padrões que indiquem técnicas de proteção, anti-debugging ou ofuscação.

---

## Reproduzindo as Análises

### GDB

```bash
break _start
run
stepi
```

Permite acompanhar a execução do programa instrução por instrução desde o ponto de entrada real.

### Ghidra

1. Importe o binário.
2. Execute a análise automática.
3. Abra o grafo de fluxo de controle.
4. Analise saltos condicionais (`je`, `jne`, `jg`, `jl`) e chamadas de função.

---

## Dica de Estudo

Compile programas simples utilizando diferentes níveis de otimização:

```bash
gcc -O0 programa.c -o programa_O0
gcc -O2 programa.c -o programa_O2
gcc -O3 programa.c -o programa_O3
```

Compare os binários gerados e observe como as otimizações alteram o código Assembly produzido pelo compilador.

Essa prática é uma das formas mais eficazes de desenvolver proficiência em engenharia reversa e compreender o comportamento interno dos softwares modernos.
