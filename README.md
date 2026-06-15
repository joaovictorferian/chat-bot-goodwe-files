#  GoodWe Assist — Chatbot Técnico EV Challenge 2026

> Solução de IA para suporte técnico aos carregadores da linha HCA da GoodWe,  
> desenvolvida como parte do **EV Challenge 2026 — FIAP x GoodWe**.

---

## Integrantes - Turma 1CCPO

| João Victor Canello Ferian | RM573295 |
| João Pedro Costenari Silva | RM572260 |
| Gustavo Melo dos Santos | RM573562 | 
---

## Problema Abordado

A GoodWe não possui um modelo padrão de cobrança para a linha **HCA G2**, devido à ausência de suporte a plataformas terceiras de billing e pagamento. Os carregadores da linha HCA não se integram nativamente com sistemas de faturamento, o que dificulta a dinâmica de pagamento em operações comerciais e condominiais.
---

##  Proposta do Chatbot

O **GoodWe Assist** é um chatbot baseado em IA com arquitetura **RAG (Retrieval-Augmented Generation)** voltado para o suporte técnico de instalação, configuração e manutenção dos carregadores da linha HCA.

**Persona atendida:** Técnico de campo responsável pela instalação e manutenção dos eletropostos GoodWe.

**O chatbot é capaz de responder sobre:**
- Procedimentos de instalação elétrica e física dos carregadores HCA
- Configuração via app SolarGo e SEMS Portal
- Diagnóstico e resolução de códigos de erro
- Modos de carregamento e integração com sistemas fotovoltaicos
- Autenticação por RFID e gerenciamento de sessões
- Comunicação e conectividade do equipamento

**O chatbot não substitui** a solução completa de billing — ele é o componente de suporte técnico que habilita a implantação correta dos equipamentos, pré-requisito para qualquer modelo comercial funcionar.

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Função | Justificativa |
|---|---|---|
| **Python 3.10+** | Linguagem base | Ecossistema consolidado para IA/ML |
| **LangChain 0.1.x** | Orquestração do pipeline RAG | Framework padrão de mercado para aplicações com LLMs |
| **OpenAI API (GPT-4o-mini)** | Geração de respostas | Modelo que garante respostas precisas e rápidas, além de uma análise minuciosa dos documentos disponibilzados |
| **OpenAI Embeddings (text-embedding-3-small)** | Vetorização dos documentos | Modelo mais eficiente da OpenAI em custo por token, suficiente para recuperação semântica em manuais técnicos |
| **FAISS (Facebook AI Similarity Search)** | Banco vetorial | Leve, sem dependências externas, roda inteiramente em memória — ideal para protótipos no Google Colab |
| **PyPDF** | Leitura de PDFs | Extração de texto de manuais técnicos com preservação de metadados de página |
| **Google Colab** | Ambiente de execução | Gratuito, sem configuração local, acessível para toda a equipe |

### Por que RAG e não Fine-tuning?

Fine-tuning exige grandes volumes de dados rotulados, tempo de treinamento e custo elevado — inviável para um protótipo acadêmico. RAG injeta o conhecimento específico da GoodWe em cada consulta via recuperação de documentos, sem retreinar o modelo. Isso permite atualizar a base de conhecimento simplesmente adicionando novos PDFs.

---

## ⚙️ Como Executar

**Pré-requisitos:**
- Conta Google (para o Colab)
- API Key da OpenAI configurada nos Secrets do Colab como `OPENAI_API_KEY`

**Passos:**

```bash
# 1. Clone o repositório dentro do Colab
!git clone https://github.com/joaovictorferian/chat-bot-goodwe-files.git

# 2. Execute as células na ordem:
#    Célula 1 — Instala dependências
#    Célula 2 — Configura API Key e caminhos
#    Célula 3 — Carrega e divide os PDFs em chunks
#    Célula 4 — Gera embeddings e cria o índice FAISS
#    Célula 5 — Configura a chain de conversação
#    Célula 6 — Executa os testes base e exibe as respostas
```

---

## 🧪 Modelo de Teste — Sprint 1

**Persona:** Técnico de Campo | **Contexto:** Suporte a instalação e manutenção GoodWe HCA

| # | Pergunta | Resposta Esperada |
|---|---|---|
| 1 | O LED do carregador está piscando em verde. O que significa? | LED piscando em verde indica que o sistema do carregador está em processo de atualização de firmware. |
| 2 | Quais são os modos de carregamento disponíveis no HCA Series? | Os modos disponíveis são: carregamento Fast (Rápido), Prioridade para energia solar (PV priority) carregamento misto (PV+battery). |
| 3 | Como instalar o carregador HCA na parede? Quais são os passos? | Fixar a placa de montagem na parede, posicionar o carregador, conectar os cabos AC e de comunicação, conectar o RCBO e ligar o equipamento. |
| 4 | Como configurar a autenticação por cartão RFID no HCA? | Acessar o app SolarGo, entrar nas configurações do carregador, habilitar o modo RFID e cadastrar os cartões autorizados. |
| 5 | Como conectar o carregador ao app SolarGo pela primeira vez? | Ligar o carregador, abrir o app SolarGo, selecionar a aba Bluetooth, localizar o dispositivo e seguir o assistente de configuração para vincular e configurar a rede Wi-Fi. |

---

## 🧠 System Prompt (Contexto-Base)

```
Você é um assistente técnico especializado em equipamentos de recarga 
veicular da GoodWe. Você auxilia técnicos de campo na instalação, configuração, 
diagnóstico de falhas e manutenção dos eletropostos HCA Series.

Regras:
- Responda com base no contexto fornecido.
- Mesmo que a informação seja parcial, responda com o que tiver disponível.
- Seja objetivo e técnico. O usuário é um técnico de campo, não um leigo.
- Quando relevante, cite o documento de onde veio a informação.
- Só diga que não encontrou a informação se o contexto realmente não contiver 
  nada relacionado à pergunta.
- Responda em português brasileiro.
```
## 🧪 Modelo de Teste — Sprint 2

**Persona:** Técnico de Campo | **Contexto:** Documentação e perguntas feitas pelo usuário

A grande mudança dessa sprint foi a adição do sistema de input, onde nossa IA se transforma em um chatbot real. A partir de agora o usuário consegue digitar a sua pergunta e o chat bot GoodWe responderá de acordo com o conhecimento adquirido a partir dos documentos acessados por ele.

| # | Pergunta | Resposta Esperada |
|---|---|---|
| 1 | Olá chat, me conte o que você sabe sobre o challenge da goodwe. |O EV Challenge 2026 da GoodWe é um evento que busca transformar dados em inteligência, energia em estratégia e ideias em soluções reais. O desafio é promovido pela GoodWe e está relacionado à mobilidade elétrica, com foco em orquestrar o futuro dessa área utilizando dados, IoT (Internet das Coisas) e inteligência artificial.|
| 2 |O que impede que os pagamentos sejam feitos de forma automática nos carregadores da goodwe | Os carregadores da linha HCA G2 da GoodWe não possuem um modelo padrão de cobrança e não oferecem suporte a plataformas terceiras de billing/pagamento. Portanto, não há suporte a sistemas de pagamento integrado para essa linha. Essa informação pode ser encontrada na seção "Pagamento e Modelo Comercial" do protocolo técnico |
| 3 |E como podemos resolver esse problema?| 1. **Desenvolvimento de uma Plataforma de Billing Interna**: Criar um sistema de cobrança próprio que possa ser integrado aos carregadores HCA G2. Isso incluiria o desenvolvimento de um gateway de pagamento que permita a autenticação e liberação das recargas.

2. **Parcerias com Provedores de Pagamento**: Estabelecer parcerias com empresas que oferecem soluções de pagamento e billing, permitindo que os carregadores se conectem a essas plataformas de forma personalizada, mesmo que não haja suporte nativo.

3. **Modelo de Cobrança Flexível**: Implementar um modelo de cobrança que permita a divisão de receita entre os usuários e o condomínio, facilitando a gestão dos custos de energia e a cobrança automática por unidade habitacional.

4. **Gerenciamento Inteligente de Demanda**: Integrar um sistema de gerenciamento que otimize a demanda de potência e permita a cobrança baseada no consumo individual, garantindo que cada unidade habitacional pague apenas pelo que consome.

5. **Interface de Usuário**: Desenvolver uma interface amigável para os usuários e para a gestão do condomínio, permitindo que os usuários visualizem seu consumo e realizem pagamentos de forma simples e eficiente. |
   
| 4 | Resuma a resposta anterior |  Um resumo das possíveis soluções para resolver a ausência de um modelo padrão de cobrança nos carregadores HCA G2 da GoodWe inclui:

1. **Desenvolvimento de uma Plataforma de Billing Interna**: Criar um sistema de cobrança próprio que integre um gateway de pagamento para autenticação e liberação das recargas.

2. **Parcerias com Provedores de Pagamento**: Estabelecer colaborações com empresas que oferecem soluções de pagamento, permitindo a conexão personalizada com as plataformas de billing.

3. **Modelo de Cobrança Flexível**: Implementar um modelo que possibilite a divisão de receita entre usuários e condomínios, facilitando a gestão dos custos de energia.

4. **Gerenciamento Inteligente de Demanda**: Integrar um sistema que otimize a demanda de potência e permita a cobrança baseada no consumo individual.

5. **Interface de Usuário**: Desenvolver uma interface amigável para que os usuários visualizem seu consumo e realizem pagamentos de forma eficiente. |


---

*EV Challenge 2026 — FIAP x GoodWe | Turma 1CCPO*
