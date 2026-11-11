# Semáforo de passagem de nível (CLP Branqs) - Treinamento de estágio em automação industrial 

## Objetivo
Ao detectar um trem na via, acionar alternadamente as lâmpadas A e B do
sinaleiro. O tempo de alternância é configurado pela IHM.

## Entradas e saídas
| Tipo    | Nome                 | Descrição                          |
|---------|----------------------|------------------------------------|
| Entrada | sensorTremPresente   | 1 = trem na via                    |
| Saída   | lampadaA             | Lâmpada A do sinaleiro             |
| Saída   | lampadaB             | Lâmpada B do sinaleiro             |
| Parâmetro | tempoAlternancia   | Setpoint em centésimos (100 = 1 s) |

## Lógica de funcionamento
1. **Temporizador:** `dsp_tmpAlternancia` segue o sensor e cai após cada
   `fim_`, reiniciando a contagem. Enquanto há trem, o ciclo se repete.
2. **Pulso de troca:** `auxPulsoTroca` = `fim_` E sensor. Ocorre uma vez
   por período de alternância.
3. **Vez da B (selo):** `auxSetB` e `auxResetB` geram pulsos de liga e
   desliga a partir do pulso de troca. O selo `auxVezDaLampadaB` mantém
   o estado entre ciclos e zera quando o trem sai.
4. **Saídas:** a lâmpada A acende com trem e sem vez da B. A lâmpada B
   acende com trem e vez da B.

## Correções em relação à versão anterior
- `lampadaA` usava `E(aux)` e `EN(aux)` juntos, o que a mantinha sempre apagada.
- `auxVezDaLampadaB` não era desligado no fim seguinte.
- O `dsp_` não caía após o `fim_`, então a contagem não reiniciava.

## Validação
- [ ] Sintaxe aceita pelo software Branqs
- [ ] Alternância observada com trem presente
- [ ] Lâmpadas apagam quando o trem sai
- [ ] Vez reinicia em A em um novo trem
- [ ] Tempo configurado pela IHM tem efeito



