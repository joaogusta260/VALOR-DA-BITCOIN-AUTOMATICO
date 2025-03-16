# Fluxo de Consulta de Preço do Bitcoin (BTC) e Envio Automático via WhatsApp

![Preço do Bitcoin](./preco%20bitcoin.png)

Este fluxo em **n8n** consulta periodicamente (a cada 15 minutos) o valor do **Bitcoin** tanto em **Reais (BRL)** quanto em **Dólares (USD)** usando a **API da Binance**, formata os preços e envia automaticamente uma mensagem via **WhatsApp** (usando o Evolution/BotConversa).

---

## O que este Fluxo Faz?

1. **Gatilho de 15 Minutos:**  
   - O nó “**TRIGGER 15 MIN**” agenda a execução do fluxo a cada 15 minutos, sem precisar verificar manualmente o site ou a cotação.

2. **Consulta à API da Binance (BTCBRL e BTCUSDT):**  
   - O fluxo realiza duas chamadas HTTP:
     - **API BTC BRL**: obtém o preço do BTC em Reais (`BTCBRL`).
     - **API BTC DOLLAR**: obtém o preço do BTC em Dólares (`BTCUSDT`).

3. **Junção dos Dados:**  
   - O nó “**JUNTA OS DOIS DADOS**” (Merge) recebe as duas respostas (BRL e USD) e as combina para tratamento posterior.

4. **Tratamento e Formatação dos Preços:**  
   - “**MUDAR CASAS DECIMAIS**”: Converte os valores numéricos para apenas 2 casas decimais, garantindo padronização.
   - “**FORMATA O PREÇO**”: Aplica formatação de moeda (R\$ e US\$) para exibir os preços de forma mais clara.

5. **Envio Automático via WhatsApp:**  
   - O nó “**Envio JOAO IA**” compõe a mensagem final (com o preço em BRL e USD) e envia automaticamente para um contato ou grupo específico via Evolution/BotConversa, permitindo receber a cotação atualizada no WhatsApp.

---

## Como Funciona o Fluxo

1. **TRIGGER 15 MIN**  
   - Define um agendamento recorrente (intervalo de 15 minutos).  
   - A cada execução, dispara a consulta às APIs de BTC em BRL e BTC em USD.

2. **API BTC BRL** e **API BTC DOLLAR**  
   - Faz duas requisições ao endpoint da **Binance**:
     - `https://data-api.binance.vision/api/v3/ticker/price?symbol=BTCBRL`
     - `https://data-api.binance.vision/api/v3/ticker/price?symbol=BTCUSDT`
   - Retorna objetos JSON contendo o `symbol` (ex.: BTCBRL) e o `price`.

3. **JUNTA OS DOIS DADOS (Merge)**  
   - Recebe ambas as respostas e unifica em um único fluxo para processamento.

4. **MUDAR CASAS DECIMAIS** (nó de código)  
   - Lê os valores de preço e converte para 2 casas decimais, transformando, por exemplo, `123456.789` em `123456.79`.

5. **FORMATA O PREÇO** (nó de código)  
   - Aplica formatação de moeda:  
     - **R\$** para BRL,  
     - **US\$** para USD.  
   - Devolve strings prontas para exibição (por ex.: “R\$ 123.456,79” e “US\$ 23.456,79”).

6. **Envio JOAO IA**  
   - Monta uma mensagem final com ambos os preços e envia para o WhatsApp via Evolution/BotConversa.  
   - Exemplo de mensagem:  
     ```
     PREÇO BTC ATUALIZADO
     Em reais (BRL): R$ 123.456,79
     Em dólares (USD): US$ 23.456,79
     ```

---

## Destaques Técnicos

- **Agendamento Recorrente:** O fluxo roda a cada 15 minutos, sem precisar de intervenção manual.  
- **APIs da Binance:** Dados confiáveis e em tempo real sobre a cotação do BTC.  
- **Formatação e Casas Decimais:** Melhora a legibilidade e padroniza o envio das cotações.  
- **Envio Automático para WhatsApp:** Não é preciso abrir aplicativos ou sites para conferir o preço — a cotação chega no seu WhatsApp periodicamente.

---

## Benefícios

- **Praticidade:** Receber o valor do BTC direto no WhatsApp, a cada 15 minutos, sem esforço.  
- **Acompanhamento Contínuo:** Permite monitorar variações de preço sem precisar acessar sites de câmbio.  
- **Personalização Fácil:** O intervalo de 15 minutos pode ser ajustado conforme necessidade; é possível alterar o destino do WhatsApp ou formatar a mensagem de outras formas.

---

Este fluxo mostra como o **n8n** pode ser usado para integrar APIs de terceiros (como a Binance) com serviços de mensagens (Evolution/BotConversa) e automatizar o envio de informações em tempo real, criando um sistema de notificação personalizado e eficiente.
