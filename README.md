# Chatbot Meteorológico no Telegram — n8n

## 1. Descrição do projeto

Este projeto consiste em um chatbot meteorológico desenvolvido com n8n, integrado ao Telegram e à API OpenWeather.

O chatbot recebe uma mensagem com o nome de uma cidade, normaliza a entrada, consulta a temperatura atual e envia uma resposta ao usuário pelo Telegram.

O workflow também possui validação da resposta e tratamento de erros para situações como cidades não encontradas ou falhas na requisição.

### Exemplo de utilização

**Mensagem enviada:**

`Fortaleza,CE,BR`

**Exemplo de resposta:**

`☀️ A temperatura em Fortaleza é de 29°C.`

A temperatura e o emoji são ilustrativos. A resposta real depende das condições climáticas retornadas pela API.

Em caso de erro, o chatbot envia:

`❌ Cidade não encontrada. Use o formato Cidade, UF, BR (ex.: São Paulo, SP, BR).`

## 2. Tecnologias utilizadas

- n8n para automação do workflow;
- Telegram Bot API para receber e enviar mensagens;
- OpenWeather API para consultar as condições meteorológicas;
- HTTP Request para realizar a requisição à API;
- Edit Fields (Set) para preparar os dados;
- IF para validar a resposta e direcionar o fluxo.

## 3. Pré-requisitos

Antes de executar o projeto, é necessário ter:

- Uma instância do n8n em execução;
- Um bot criado no Telegram por meio do BotFather;
- Uma chave de API da OpenWeather;
- Acesso às configurações de credenciais e variáveis de ambiente do n8n.

## 4. Configuração das credenciais

### 4.1. Telegram

1. Abra o Telegram e acesse o BotFather.
2. Crie um bot ou utilize um bot existente.
3. Copie o token fornecido pelo BotFather.
4. No n8n, acesse **Credentials** e crie uma credencial do tipo Telegram.
5. Informe o token do bot e salve a credencial.
6. Selecione essa credencial nos nós Telegram Trigger e Telegram Send Message.

A variável `TELEGRAM_BOT_TOKEN` é utilizada para documentar a configuração do token no ambiente. Na configuração padrão deste workflow, o token é armazenado na credencial do Telegram no n8n. Definir a variável de ambiente, por si só, não preenche automaticamente a credencial.

### 4.2. OpenWeather

1. Crie uma conta na OpenWeather.
2. Gere uma chave de API.
3. Configure a variável de ambiente `OPENWEATHER_API_KEY` no ambiente em que o n8n é executado.
4. Reinicie o serviço do n8n, se necessário, para carregar a variável.
5. Confirme que o acesso às variáveis de ambiente está habilitado para o workflow.

O nó HTTP Request utiliza a expressão:

`{{ $env.OPENWEATHER_API_KEY }}`

## 5. Variáveis de ambiente

As variáveis esperadas pelo projeto são:

| Variável | Finalidade |
|---|---|
| `OPENWEATHER_API_KEY` | Chave de acesso à API OpenWeather |
| `TELEGRAM_BOT_TOKEN` | Token do bot do Telegram |

Para uma instalação local com Docker Compose, utilize os environments:

```dotenv
OPENWEATHER_API_KEY=sua_chave_openweather
TELEGRAM_BOT_TOKEN=seu_token_telegram
```

Substitua os valores de exemplo pelos valores reais. Não publique esse arquivo no repositório.

O Docker Compose deve disponibilizar `OPENWEATHER_API_KEY` para o serviço n8n. A variável `TELEGRAM_BOT_TOKEN` pode ser disponibilizada no ambiente, mas a credencial Telegram utilizada pelos nós também precisa ser configurada no n8n, conforme descrito anteriormente.

## 6. Importação do workflow

1. Abra a interface do n8n.
2. Crie um workflow ou abra a área de workflows.
3. No menu de opções, selecione a opção de importar workflow a partir de arquivo.
4. Selecione `workflow-telegram-chatbot.json`.
5. Confira as conexões entre os nós.
6. Selecione a credencial Telegram nos nós correspondentes.
7. Configure a variável `OPENWEATHER_API_KEY` no ambiente do n8n.
8. Salve o workflow.

## 7. Como executar

1. Abra o workflow importado.
2. Confira as credenciais e as variáveis de ambiente.
3. Ative o workflow.
4. Envie uma mensagem ao bot pelo Telegram com a cidade desejada.

Exemplo:

`Curitiba, PR, BR`

O chatbot deve consultar a OpenWeather e retornar a temperatura atual, com o valor arredondado para cima e um emoji correspondente à condição climática.

Para verificar o tratamento de erros, envie uma cidade inexistente e confirme o recebimento da mensagem de erro.

## 8. Tratamento de erros

O workflow possui caminhos para tratar respostas inválidas e falhas na requisição HTTP.

- Resposta válida: a temperatura é formatada e enviada pelo Telegram.
- Cidade não encontrada ou resposta inválida: o usuário recebe uma mensagem de erro.
- Falha na execução da requisição: o fluxo de erro do HTTP Request direciona a execução para o envio da mensagem de erro.

## 9. Testes

Os seguintes testes devem ser executados antes da entrega:

- [ ] Consulta de Curitiba,PR,BR.
- [ ] Consulta de São Paulo,SP,BR.
- [ ] Consulta de Belo Horizonte,MG,BR.
- [ ] Teste com uma cidade inexistente.
- [ ] Verificação da mensagem de erro.
- [ ] Verificação do arredondamento da temperatura.
- [ ] Verificação do emoji correspondente ao clima.
- [ ] Verificação de que não há credenciais reais no JSON exportado.


## 11. Arquivos do projeto

- `workflow-telegram-chatbot.json`: workflow exportado do n8n.
- `README.md`: documentação do projeto e instruções de configuração.

