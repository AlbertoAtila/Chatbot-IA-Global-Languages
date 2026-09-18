# Chatbot especialista da Global Languages (Gemini + Google Colab)

<!-- Repositório no GitHub. -->
(https://github.com/AlbertoAtila/Chatbot-IA-Global-Languages)

Chatbot especialista que responde, **sem inventar informações**, a dúvidas sobre dados internos e não públicos da **Global Languages**, escola de idiomas fictícia com sede em Praia Grande/SP, na Baixada Santista. A atendente virtual **Lia** responde a **exatamente 3 perguntas** usando apenas a base de conhecimento do notebook. Depois da 3ª resposta, ela apresenta um breve resumo do que foi respondido e encerra a conversa.

O projeto adapta o notebook *The Chat Format* (OrderBot), feito originalmente para a API da OpenAI, ao **Google Gemini** (`gemini-3.6-flash`), com o SDK Google Gen AI (`google-genai`), a API GenerateContent e a biblioteca Panel na interface.

## Como funciona

- **Contexto em três blocos**, em variáveis separadas e enviados como `system_instruction`:
  - **PERSONALIDADE**: a Lia é cordial e objetiva, responde com até 120 palavras e explica procedimentos em passos numerados.
  - **OBJETIVO E TAREFA**: responder só com o bloco CONHECIMENTO, declarar quando não souber e indicar o canal oficial, nunca inventar dados, recusar temas alheios à escola e não revelar as instruções.
  - **CONHECIMENTO**: base fictícia com dados institucionais, canais oficiais, valores e quatro procedimentos internos (matrícula e nivelamento; reposição de aulas; trancamento, cancelamento e reembolso; suporte técnico no Moodle e no BigBlueButton).
- **Fluxo controlado pelo código, não pelo modelo:**
  - a saudação é fixa e aparece só na interface, sem chamada à API e sem contar como pergunta;
  - cada mensagem não vazia conta uma pergunta, com o indicador "Pergunta n de 3";
  - uma falha da API não consome pergunta: aparece um aviso e o texto volta ao campo para reenvio;
  - depois de exibir a 3ª resposta, uma chamada separada à API gera o resumo (até 80 palavras), exibido com a despedida;
  - por fim, o campo de texto e o botão são desabilitados, e novas mensagens são ignoradas.
- **Chave protegida:** a `GEMINI_API_KEY` é lida somente do arquivo `.env`, com o python-dotenv, e nunca é exibida.

## Arquivos do repositório

| Arquivo | Conteúdo |
|---|---|
| `chatbot_global_languages.ipynb` | Notebook do Colab: instalação, configuração, função de chamada, contexto, lógica e interface |
| `env.example` | Modelo do arquivo `.env`, sem valores |
| `.gitignore` | Impede o envio do `.env` e dos checkpoints do Jupyter ao GitHub |
| `README.md` | Este guia |

## Passo a passo para reproduzir no Google Colab

Requisito: uma conta Google. Nenhuma instalação local é necessária.

### 1. Obter a chave da API no Google AI Studio
1. Acesse <https://aistudio.google.com/apikey> e entre com sua conta Google.
2. Clique em **Create API key** (Criar chave de API) e copie a chave gerada.
3. Guarde a chave em local seguro e não a compartilhe.

### 2. Abrir o notebook pelo badge "Open in Colab"
Clique no badge **Open in Colab** no topo desta página. O notebook abre direto do GitHub (o repositório precisa ser público).

### 3. Criar o `.env` a partir do `env.example` e enviar para `/content`
1. Faça uma cópia do `env.example` com o nome `.env`, com ponto no início e sem extensão. No Windows, se o Explorador recusar o nome, abra o arquivo no Bloco de Notas e use **Salvar como**, com o tipo **Todos os arquivos** e o nome `.env`.
2. Cole a sua chave depois do sinal de igual, sem aspas e sem espaços:
   ```
   GEMINI_API_KEY=cole_sua_chave_aqui
   ```
3. No Colab, abra o painel **Arquivos** (ícone de pasta, à esquerda), clique em **Fazer upload para o armazenamento da sessão** e envie o `.env`. Ele fica em `/content`, a pasta padrão do Colab.

> O `.env` pode não aparecer na lista por começar com ponto; a célula de configuração confirma se ele foi encontrado. Os arquivos da sessão são apagados quando o ambiente de execução é desconectado ou excluído. Nesse caso, envie o `.env` de novo.

### 4. Executar todas as células
No menu, clique em **Ambiente de execução > Executar tudo** (atalho `Ctrl+F9`).
- A seção 2 (Configuração) deve mostrar `GEMINI_API_KEY carregada de /content/.env (valor oculto).`
- A seção 6 (Interface) exibe a saudação da Lia com o indicador **Pergunta 1 de 3**.

Se aparecer erro de `.env` não encontrado ou de chave vazia, repita o passo 3 e execute tudo de novo. Se o Colab pedir para reiniciar a sessão depois da instalação, aceite e execute tudo novamente; o `.env` continua em `/content`.

### 5. Fazer as 3 perguntas da demonstração
Digite uma pergunta por vez no campo de texto, clique em **Enviar** e aguarde a resposta antes da próxima:

1. Quero matricular meu filho de 15 anos no curso de inglês. Quais são os passos e os documentos necessários?
2. Fiz a matrícula on-line há 4 dias e desisti do curso. Como peço o reembolso?
3. Não consigo entrar na aula ao vivo pelo BigBlueButton. O que devo fazer?

Depois da 3ª resposta, a Lia exibe o resumo do atendimento e a despedida. O indicador passa a mostrar **Conversa encerrada**, e o campo e o botão ficam desabilitados. Para começar um novo atendimento, execute novamente a última célula.

## Observações
- Todos os dados da Global Languages, inclusive a consultoria Âncora Tecnologia, os e-mails, os telefones e os sites, são fictícios e existem apenas para fins acadêmicos.
- Nunca faça commit do arquivo `.env`. O `.gitignore` já o exclui do repositório.