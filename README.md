Desafio DIO: Engenharia Reversa e Especialização em C/Assembly
1. Contexto e Objetivos

Este projeto consiste na criação de um Caderno Temático no NotebookLM focado em Engenharia Reversa e na relação intrínseca entre as linguagens C e Assembly (x86/x64).

Objetivos de Estudo:

    Compreender o funcionamento interno de binários e como o código de alto nível (C) é traduzido para instruções de máquina.

    Dominar a análise de fluxo de execução, registradores e convenções de chamada (calling conventions).

    Explorar o futuro da engenharia reversa frente a novas arquiteturas e sistemas de segurança.

    Aprender a utilizar ferramentas consagradas da área como Ghidra, IDA Free, Cutter e GDB.

2. Curadoria de Fontes

Para alimentar o NotebookLM, selecionei as seguintes fontes fundamentais:

    "Computer Systems: A Programmer's Perspective" (CS:APP) - Randal E. Bryant & David R. O'Hallaron. (Referência acadêmica sobre como o hardware e software interagem).

    "The Art of Assembly Language" - Randall Hyde. (Guia completo para entender a lógica por trás do processador).

    "Linux System Programming: Talking Directly to the Kernel" - Robert Love. (Essencial para entender Syscalls e o comportamento do sistema operacional sob o capô).

3. Engenharia de Prompts e "Cicatrizes" (Troubleshooting)

Prompt Mestre Elaborado:

    "Explique o funcionamento interno do programa abaixo passo a passo, identificando fluxo de execução, chamadas de funções, estruturas de dados, uso de memória, registradores, convenções de chamada, algoritmos utilizados e possíveis otimizações do compilador. Sempre apresente exemplos práticos em C e Assembly x86/x64, diagramas ASCII quando necessário e explique como reproduzir a análise utilizando Ghidra, IDA Free, Cutter e GDB. Ao encontrar funções desconhecidas, descreva hipóteses fundamentadas sobre seu comportamento e o raciocínio utilizado para chegar à conclusão. O objetivo é aprendizado profundo de engenharia reversa e compreensão do funcionamento interno do software."

Desafios Encontrados (Troubleshooting):

    Dificuldade: Diferenciar o comportamento do entry point _start em relação ao main() da biblioteca padrão C (libc).

    Aprendizado: Entender que o _start é o ponto de entrada real do binário e que o main é apenas uma convenção de alto nível que depende de um ambiente configurado pelo crt0 (C Runtime Startup).

    Insight: A análise de registradores (como RAX para syscalls) é o ponto de virada para identificar funções que o compilador "escondeu" ou otimizou.

4. Miniguia de Estudo (Entrega Final)
Resumo Estruturado

A engenharia reversa é a arte de reconstruir o "porquê" de um software quando o código-fonte original não está disponível. A tradução C -> Assembly envolve:

    Compilação: O código C é transformado em Assembly, onde variáveis viram posições de memória ou registradores.

    Syscalls: A interface direta entre programa e SO. O registrador RAX dita a operação, enquanto outros registradores (RDI, RSI, RDX) carregam os argumentos.

    Otimização: Compiladores modernos (como gcc -O3) removem redundâncias, tornando a leitura do Assembly mais desafiadora por tentar ser mais eficiente que o código original.

Glossário de Conceitos

    Syscall: Interface entre a aplicação e o Kernel do SO.

    Registradores: Memória de ultra-alta velocidade dentro da CPU (ex: RAX, RDI, RSI).

    Ghidra/IDA: Ferramentas que fazem o "Decompilation", tentando transformar código de máquina de volta em C legível.

    Convenção de Chamada: Regras sobre como argumentos são passados para funções (em x64 Linux, via registradores RDI, RSI, RDX, RCX, R8, R9).

    Seção .data: Área do binário dedicada a dados estáticos/globais.

Prompts Reutilizáveis

    "Analise o seguinte bloco de código Assembly e tente reconstruir a assinatura da função equivalente em C, identificando se os parâmetros são passados via pilha ou registradores."

    "Explique a diferença entre uma chamada de função (call) e uma chamada de sistema (syscall) neste trecho de código."

    "Identifique, neste binário, qualquer padrão que indique o uso de técnicas de proteção ou ofuscação (ex: anti-debugging)."

Como reproduzir a análise:

    GDB: Use break _start para ver o início da execução e stepi para analisar instrução por instrução.

    Ghidra: Importe o binário e utilize o grafo de fluxo de controle para visualizar os saltos (jmp) e condições (jne/je).

Dica de estudo: A prática leva à perfeição. Tente compilar códigos simples com diferentes níveis de otimização (-O0, -O2, -O3) e compare como o Assembly gerado muda!
