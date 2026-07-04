# MIC1 

Projeto no Quartus II (v13.0) que implementa, em diagramas de blocos (`.bdf`), partes da arquitetura **MIC-1** descrita no livro *Structured Computer Organization* (Tanenbaum): um decoder 2-para-4, um somador completo de 1 bit e ULAs de 1, 8 e 32 bits. Elaborado durantes as aulas de laboratório de Arquitetura e Organização de Computadores I, pelo grupo composto por Rafael Alves Faria, Gabriel Alves Faria e Samuel Pianetti 

## Estrutura dos arquivos

### Decoder 2-para-4
| Arquivo | Descrição |
|---|---|
| `DECODER_2T04.bdf` | Diagrama do decoder: 2 entradas (F0, F1) → 4 saídas (S0–S3), construído com portas AND2 e NOT |
| `DECODER_2T04.bsf` | Símbolo do bloco (para ser reutilizado dentro de outros diagramas) |
| `DECODER_2T04.vwf` | Vetor de teste, varrendo as combinações de F0/F1 |

### Somador completo de 1 bit (Fig. 3.17)
| Arquivo | Descrição |
|---|---|
| `full_adder_1bit.bdf` | Entradas A, B, Carry_in → saídas Sum, Carry_out. Construído com 2 XOR + 2 AND2 + 1 OR2 |
| `full_adder_1bit.bsf` | Símbolo do bloco |
| `full_adder_1bit.vwf` | Vetor de teste das 5 linhas (A, B, Carry_in, Carry_out, Sum) |

### ULA de 1 bit (Fig. 3.18)
| Arquivo | Descrição |
|---|---|
| `ULA_1bit.bdf` | ULA de 1 bit completa: usa o decoder interno + portas lógicas para implementar as funções controladas por F0/F1/ENA/ENB/INVA/CIN |
| `ULA_1bit.bsf` | Símbolo do bloco (reutilizado dentro da ULA de 8 bits) |
| `ULA_1bit.vwf` | Teste com ENA=1, ENB=1, INVA=0 fixos e F0/F1 variando → cobre **A AND B, A OR B, INV B, A+B** |
| `ULA_1bit_b.vwf` | Teste com F0=F1=0 fixos e **ENA, ENB, INVA** variando |

### ULA de 8 bits (Fig. 3.19)
| Arquivo | Descrição |
|---|---|
| `ULA_8bit.bdf` | ULA de 8 bits, montada a partir de 8 instâncias de `ULA_1bit` encadeadas pelo carry |
| `ULA_8bit.bsf` | Símbolo do bloco (reutilizado dentro da ULA de 32 bits) |
| `ULA_8bit_AND.vwf` | Vetor de teste |
| `ULA_8bit_ADD.vwf` | Vetor de teste |
| `ULA_8bit_OR.vwf` | Vetor de teste |
| `ULA_8bit_CONSTANTS.vwf` | Vetor de teste |
| `ULA_8bit (2).vwf` | Vetor de teste |

> Os 5 arquivos acima cobrem, juntos, os grupos de funções pedidos no enunciado (A/B/A!/B!, A+B/A+B+1/A+1/B+1, B-A/B-1/-A/A AND B, A OR B/0/1/-1), mas a correspondência exata **arquivo → grupo de funções** só pode ser confirmada rodando a simulação no Quartus (não verificado neste README).

### ULA de 32 bits (Fig. 4.5, via MIR)
| Arquivo | Descrição |
|---|---|
| `ULA_32bit.bdf` | ULA de 32 bits, montada a partir de 4 instâncias de `ULA_8bit`. O controle (F0, F1, ENA, ENB, INVA) vem dos bits `MIR[16..21]` do registrador de microinstrução (MIR), em vez de pinos soltos |
| `ULA_32bit.vwf` | Teste com MIR[18]=MIR[19]=1 e MIR[16]=MIR[17]=0 fixos, MIR[20]/MIR[21] variando → cobre **A AND B, A OR B, INV B, A+B** |

### Arquivos de projeto
| Arquivo | Descrição |
|---|---|
| `MIC1.qpf` | Arquivo de projeto do Quartus |
| `MIC1.qsf` | Configurações do projeto (família do dispositivo, lista de arquivos-fonte, entidade top-level) |
| `MIC1.bdf` | Diagrama de nível superior (contém, atualmente, apenas uma instância do decoder) |


## Requisitos
- Quartus II 13.0 SP1 (ou compatível com o formato de arquivo salvo)
