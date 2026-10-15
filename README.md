# Planilha-de-investimentos
## 1. Perguntas que a ferramenta responde

| Pergunta | Onde aparece | Resultado atual |
|---|---|---:|
| Quanto investir por mês? | `APP!D10` | R$ 1.000,00 |
| Por quantos anos investir? | `APP!D11` | 10 anos |
| Qual é a taxa mensal? | `APP!D12` | 1,08% a.m. |
| Qual será o patrimônio? | `APP!D13` | R$ 243.284,21 |
| Qual será o dividendo mensal? | `APP!D14` | R$ 1.459,71 |
| Quanto acumulará em diferentes prazos? | `APP!C17:D21` | Cenários de 2 a 30 anos |
| Como distribuir o aporte entre FIIs? | `APP!C29:D34` | Percentuais e valores |
| A distribuição soma o aporte? | `APP!D35` | R$ 1.000,00 |

---

## 2. Como o VF e o PROCV entram nos cálculos

### VF — projeção do patrimônio

Em `APP!D13`:

```excel
=VF(taxa_mensal;qtd_anos*12;aporte*-1)
```

O prazo é convertido de anos para meses. O aporte entra negativo por representar uma saída mensal; assim, o patrimônio futuro retorna positivo.

A mesma lógica calcula os cenários em `APP!C17:C21`. Com R$ 1.000 mensais e taxa interna de 1,079% a.m.:

- 2 anos: **R$ 27.227,63**
- 10 anos: **R$ 243.284,21**
- 30 anos: **R$ 4.322.169,66**

Os dividendos são calculados assim:

```excel
=patrimonio*rendimento_carteira
```

### PROCV — distribuição conforme o perfil

Em `APP!C29:C34`:

```excel
=PROCV($C$25&"-"&B29;Sheet2!$A:$D;4;FALSO)```

A fórmula combina o perfil com o tipo de FII, formando chaves como `Conservador-PAPEL`, e procura o percentual exato em `Sheet2!A3:D20`.

Depois, `APP!D29:D34` multiplica cada percentual pelo aporte mensal.

---

## 3. Intervalos nomeados

| Nome | Referência | Finalidade |
|---|---|---|
| `aporte` | `APP!D10` | Valor mensal investido |
| `qtd_anos` | `APP!D11` | Prazo utilizado pelo VF |
| `taxa_mensal` | `APP!D12` | Taxa periódica do VF |
| `patrimonio` | `APP!D13` | Base do cálculo dos dividendos |
| `rendimento_carteira` | `APP!D6` | Rendimento mensal da carteira |

Eles deixam as fórmulas mais claras e fáceis de auditar.

---

## 4. Percentuais de cada perfil

| Perfil | Papel | Tijolo | Híbridos | FOFs | Desenvolvimento | Hotelarias |
|---|---:|---:|---:|---:|---:|---:|
| Conservador | 30% | 50% | 10% | 10% | 0% | 0% |
| Moderado | 32% | 35% | 8% | 5% | 10% | 10% |
| Agressivo | 50% | 10% | 5% | 5% | 20% | 10% |

Todos somam 100% e estão cadastrados em `Sheet2!B3:D20`.

**Origem:** são premissas fixas da ferramenta. A planilha não apresenta fonte externa nem justificativa metodológica para esses percentuais; portanto, não devem ser apresentados como recomendação oficial.

---

## 5. Mudanças em relação à ferramenta do Expert

Conforme documentado em `Guia do Portfólio!A26:D29`, foram acrescentados ou ajustados:

- seletor de perfil;
- base estruturada de percentuais em `Sheet2!A3:D20`;
- busca automática dos percentuais com PROCV;
- intervalos nomeados;
- cenários de 2, 5, 10, 20 e 30 anos;
- estimativa de dividendos mensais;
- gráfico da distribuição por tipo de FII;
- correção da sugestão de investimento: o texto indicava 30%, mas a fórmula calculava 20%. Agora `APP!D7` calcula corretamente **R$ 4.500,00**, ou 30% de R$ 15.000,00.

As abas `Evidência Conservador` e `Evidência Agressivo` demonstram a mesma simulação, alterando somente o perfil. O patrimônio permanece igual; muda apenas a distribuição dos R$ 1.000 entre os tipos de FII.
