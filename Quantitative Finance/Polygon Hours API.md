
#polygon #computer_science #data_science #quantitative_finance #api 
## Resumo

O objeto `MarketStatus` fornece um instantâneo do estado de vários mercados e instrumentos financeiros num dado momento. Os campos principais indicam se o mercado de ações principal está aberto ou fechado (`market`), se há negociação no período estendido pós-fecho (`after_hours`) ou pré-abertura (`early_hours`), o estado dos mercados cambial e de criptomoedas (`currencies.crypto`, `currencies.fx`), o estado das bolsas norte-americanas (`exchanges.nasdaq`, `exchanges.nyse`, `exchanges.otc`) e o estado de diferentes grupos de índices (`indicesGroups`). O atributo `server_time` mostra o carimbo de data/hora do servidor em formato ISO 8601, permitindo correlacionar o status com um horário exato e padronizado [Investopedia](https://www.investopedia.com/terms/a/afterhourstrading.asp?utm_source=chatgpt.com)[Investopedia](https://www.investopedia.com/terms/forex/f/forex-market-trading-hours.asp?utm_source=chatgpt.com).

---

## 1. Horário Regular e Períodos Estendidos

### 1.1. after_hours

Indica se o mercado está em negociação pós-fecho, ou seja, após as 16h00 ET (hora-base da NYSE/Nasdaq). Em `after_hours=False`, não há negociação pública nesse momento. As sessões after-hours normalmente ocorrem entre as 16h00 e as 20h00 ET, mas apresentam menor liquidez e maior volatilidade [Investopedia](https://www.investopedia.com/terms/a/afterhourstrading.asp?utm_source=chatgpt.com)[Wikipedia](https://en.wikipedia.org/wiki/Extended-hours_trading?utm_source=chatgpt.com).

### 1.2. early_hours

Refere-se à negociação pré-abertura (“pre-market”), que ocorre tipicamente entre as 4h00 e as 9h30 ET. Em `early_hours=False`, o pre-market está fechado. Embora ofereça oportunidades de reagir a notícias antes da abertura oficial, também acarreta riscos semelhantes aos do after-hours [InvestopediaInvestopedia](https://www.investopedia.com/ask/answers/06/preaftermarket.asp?utm_source=chatgpt.com).

---

## 2. Mercados de Câmbio e Cripto

O sub-objeto `currencies` mostra submercados que operam normalmente de domingo 17h00 ET até sexta 17h00 ET (forex) e 24/7 (criptomoedas):

- **crypto='open'**: o mercado de criptomoedas está aberto 24 horas, 7 dias por semana, sem interrupções [Fidelity Investments](https://www.fidelity.com/learning-center/smart-money/stock-market-hours?utm_source=chatgpt.com)[Investopedia](https://www.investopedia.com/terms/forex/f/forex-market-trading-hours.asp?utm_source=chatgpt.com).
- **fx='open'**: o mercado de câmbio opera 24 horas por dia durante os dias úteis, fechando apenas na sexta à tarde e reabrindo no domingo à noite [Investopedia](https://www.investopedia.com/terms/forex/f/forex-market-trading-hours.asp?utm_source=chatgpt.com)[Investopedia](https://www.investopedia.com/articles/forex/08/forex-trading-schedule-trading-times.asp?utm_source=chatgpt.com).

---

## 3. Bolsas de Valores

O sub-objeto `exchanges` reflete o estado das principais bolsas norte-americanas:

- **nasdaq='closed'**
- **nyse='closed'**
- **otc='closed'**

Todas as bolsas tradicionais na América do Norte funcionam de segunda a sexta, das 9h30 às 16h00 ET, fechando em feriados oficiais e não operando em finais de semana [Trading Hours](https://www.tradinghours.com/markets/nyse?utm_source=chatgpt.com)[Nasdaq](https://www.nasdaq.com/market-activity/stock-market-holiday-schedule?utm_source=chatgpt.com).

---

## 4. Grupos de Índices

O campo `indicesGroups` lista vários índices e fornece seu estado:

|Índice|Status|Observação|
|---|---|---|
|s_and_p (S&P 500)|closed|Segue o horário das bolsas de ações nos EUA (9h30–16h ET) [Investopedia](https://www.investopedia.com/terms/t/tradingsession.asp?utm_source=chatgpt.com)|
|dow_jones|closed|Mesmo horário do S&P 500|
|nasdaq|closed|Índice das ações da Nasdaq|
|ftse_russell|closed|Índice global operado pela FTSE Russell|
|msci|closed|Índices globais MSCI|
|societe_generale, cgi|closed|Grupos ou índices específicos de determinadas instituições|
|mstar, mstarc|open|Índices Morningstar são calculados em tempo real, mas sem negociação direta|
|cccy|open|Índice de moedas (“currency” index)|

Em geral, **`closed`** indica que o cálculo e disseminação dos dados seguem a agenda de fechamento dos mercados subjacentes, enquanto **`open`** pode se referir a índices que são atualizados continuamente mesmo fora do horário regular de negociação [Investopedia](https://www.investopedia.com/ask/answers/042415/what-average-annual-return-sp-500.asp?utm_source=chatgpt.com)[Wikipedia](https://en.wikipedia.org/wiki/Extended-hours_trading?utm_source=chatgpt.com).

---

## 5. Estado Global do Mercado (`market`)

- **`market='closed'`**: sinaliza que o mercado amplo de ações (agregado) está fechado neste instante, pois não há sessões regulares ou estendidas ativas [Investopedia](https://www.investopedia.com/terms/t/tradingsession.asp?utm_source=chatgpt.com).

---

## 6. Timestamp do Servidor (`server_time`)

- **`server_time='2025-04-28T21:12:53-04:00'`** segue o padrão ISO 8601, que organiza data e hora de forma unívoca e global. Exemplo de formato: `YYYY-MM-DDThh:mm:ss±hh:mm` [Wikipedia](https://en.wikipedia.org/wiki/ISO_8601?utm_source=chatgpt.com)[ISO](https://www.iso.org/iso-8601-date-and-time-format.html?utm_source=chatgpt.com).

Esse carimbo permite sincronizar o estado reportado com um ponto exato no tempo, considerando o deslocamento de fuso (neste caso, UTC-04:00).

---

**Em suma**, este objeto é uma representação programática e padronizada dos horários de operação de diversos mercados financeiros, oferecendo um retrato imediato do que está ou não disponível para negociação no momento especificado.