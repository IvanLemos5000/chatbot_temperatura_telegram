# Telegram Weather Chatbot – N8N Workflow

Chatbot para Telegram que consulta a temperatura atual de qualquer cidade usando a API da OpenWeather.  
O workflow foi feito no N8N e exportado como JSON para fácil importação.

## Funcionalidades

- Recebe mensagens de texto do Telegram (ex.: `São Paulo,SP,BR`)
- Formata e normaliza o texto (remove acentos, espaços, deixa minúsculo)
- Consulta a OpenWeather com os parâmetros corretos
- Retorna a temperatura atual no formato:
  `🌤️ A temperatura em Belo Horizonte é de 25°C.`
- Se a cidade não for encontrada, envia:
  `❌ Cidade não encontrada. Use o formato Cidade,UF,BR (ex.: São Paulo,SP,BR).`

## Como importar o workflow no N8N

1. Abra seu N8N (local ou cloud).
2. Vá em **Workflows** → **Import from File**.
3. Selecione o arquivo `workflow-chatbot-telegram.json`.
4. O workflow será importado com todos os nós e conexões.

## Credenciais necessárias

Você precisa configurar duas variáveis de ambiente no N8N (ou como credenciais no nó):

| Variável              | Descrição                                      |
|-----------------------|------------------------------------------------|
| `TELEGRAM_BOT_TOKEN`  | Token do seu bot do Telegram (criado via @BotFather) |
| `OPENWEATHER_API_KEY` | Chave da API da OpenWeather (obtida em openweathermap.org) |

### Como inserir as variáveis no N8N

- No N8N, vá em **Settings** → **Environment Variables** (ou use um arquivo `.env` se estiver rodando via Docker).
- Adicione as duas variáveis com os valores reais.
- **Nunca coloque as chaves no repositório ou no JSON exportado.**

## Como testar o chatbot

1. No Telegram, envie `/start` para o seu bot (opcional, o trigger já captura qualquer texto).
2. Envie uma mensagem com o nome de uma cidade, no formato:  
   `Cidade,UF,BR`  
   Exemplo: `Belo Horizonte,MG,BR`
3. O bot deve responder com a temperatura atual.
4. Teste também com uma cidade inválida (ex.: `CidadeQualquer,XX,BR`) para ver a mensagem de erro.

### Exemplos de teste

| Entrada                    | Resposta esperada (aproximada)                       |
|----------------------------|------------------------------------------------------|
| `São Paulo,SP,BR`         | 🌤️ A temperatura em São Paulo é de 22°C.            |
| `Rio de Janeiro,RJ,BR`    | 🌤️ A temperatura em Rio de Janeiro é de 30°C.       |
| `Londres,UK`              | ❌ Cidade não encontrada...                          |

## Requisitos atendidos

- [x] Trigger via Telegram
- [x] Captura e formatação da entrada (normalização, remoção de acentos, lowercase)
- [x] Chamada à OpenWeather com query parameters
- [x] Extração e formatação da temperatura (arredondada)
- [x] Validação de resposta e tratamento de erro (IF node)
- [x] Mensagens amigáveis com emojis
- [x] Exportação do workflow sem credenciais
- [x] Variáveis de ambiente para tokens

## (Opcional) Uso do Google Gemini

Este workflow não inclui o nó Gemini por padrão para manter a simplicidade e a compatibilidade com a avaliação automática.  
Caso queira melhorar a saída com reescrita natural, siga as instruções do desafio original:

1. Adicione um nó **Google Gemini** após o nó de sucesso da OpenWeather.
2. Configure com temperatura baixa (0 a 0.2) e instrução para retornar JSON `{"message":"...","ok":true}`.
3. Inclua também um nó **Function/Code** como fallback (usando a mesma formatação do nó original).
4. Defina a variável `GEMINI_API_KEY` no N8N.
5. O avaliador usará o fallback caso a chave não esteja disponível.
