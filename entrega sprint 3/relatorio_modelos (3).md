# Relatório de Comparação entre Modelos de Linguagem — Sprint 03

## 1. Objetivo

Nesta Sprint 03, foi realizada uma comparação entre dois modelos de linguagem utilizando o mesmo chatbot GoodWe, o mesmo contexto documental, a mesma configuração de temperatura e o mesmo conjunto de três perguntas.

O objetivo foi observar diferenças de comportamento e selecionar o modelo utilizado na versão final do agente.

## 2. Configuração dos testes

| Modelo | Provedor | Temperature | Max tokens | Perguntas |
|---|---|---:|---:|---:|
| `gpt-4o-mini` | OpenAI | 0.1 | 1000 | Q1, Q2, Q3 |
| `openai/gpt-oss-120b` | Groq | 0.1 | 1000 | Q1, Q2, Q3 |

As perguntas utilizadas foram:

1. The Led Charger is flashing green. What does that mean?
2. What are the charging modes available in the HCA series chargers?
3. How to install the HCA Charger on the wall? What are the steps?

Os testes foram executados sobre a arquitetura da Sprint 03, utilizando LangGraph e a ferramenta de busca na documentação GoodWe.

## 3. Resultados

### Q1 — LED do carregador piscando em verde

**GPT-4o-mini:** informou que o LED piscando em verde indica que o sistema está em processo de atualização. Resposta objetiva, sem formatação adicional. Fonte citada: GW_HCA Series User Manual, p.20.

**GPT-OSS-120b:** apresentou a mesma informação principal com formatação em negrito e citou explicitamente a seção "LED Indicators" do Quick Installation Manual, p.5.

**Análise:** os dois modelos chegaram à mesma conclusão. O GPT-OSS-120b apresentou a resposta com mais estrutura visual e referência de seção mais específica. O GPT-4o-mini foi mais direto e conciso.

### Q2 — Modos de carregamento

**GPT-4o-mini:** listou dois modos — Carregamento Normal e Controle Dinâmico de Carga — com explicação sobre o controle dinâmico e referência ao documento FIAP_EV Challenge_2026_Mentoria1.pdf, p.6.

**GPT-OSS-120b:** listou três modos — Fast, PV Priority e PV + Battery — com descrições detalhadas de cada um e referência às páginas 46-48 do GW_HCA Series User Manual.

**Análise:** diferença relevante. O GPT-OSS-120b recuperou e apresentou um conjunto mais completo de modos de carregamento, com fontes mais específicas do manual técnico. O GPT-4o-mini apresentou uma resposta parcial baseada em um documento de mentoria, não no manual principal.

### Q3 — Instalação na parede

**GPT-4o-mini:** listou 6 passos de instalação com observações de segurança. Especificou broca de 8 mm e profundidade de 50 mm. Fonte: GW_HCA Series User Manual, p.29.

**GPT-OSS-120b:** listou os mesmos passos com detalhes adicionais — incluiu o quadro RCBO e a tomada-moca, torque de aperto (≈ 2 N·m para M5) e referência à seção 5.2.2 do manual. Observações de segurança presentes.

**Análise:** os dois modelos produziram respostas corretas e completas. O GPT-OSS-120b acrescentou detalhes técnicos adicionais (torque, referência de seção) que não apareceram na resposta do GPT-4o-mini.

## 4. Comparação geral

| Critério | GPT-4o-mini | GPT-OSS-120b (via Groq) |
|---|---|---|
| Configuração | temperature 0.1, max_tokens 1000 | temperature 0.1, max_tokens 1000 |
| Q1 | Correta, concisa, fonte citada | Correta, formatada, seção explicitada |
| Q2 | Parcial — 2 modos, fonte secundária | Completa — 3 modos, manual técnico |
| Q3 | Correta, passos detalhados | Correta, detalhes técnicos adicionais |
| Referência documental | Presente, nível de página | Presente, nível de seção e página |
| Estilo | Objetivo, texto corrido | Estruturado, uso de negrito e listas |
| Diferença prática | Respostas corretas, menor detalhe técnico | Respostas mais completas e referenciadas |

Não foram registrados valores de latência, custo por consulta ou contagem de tokens efetivamente consumidos. Portanto, esses critérios não foram utilizados para decidir o modelo.

## 5. Modelo utilizado na versão final

O modelo mantido na versão final foi o **GPT-4o-mini**, com `temperature=0.1` e `max_tokens=1000`.

A escolha considera a integração já realizada com o pipeline do projeto e o desempenho consistente nos testes. Nos três casos, o modelo apresentou respostas coerentes com a documentação recuperada e adequadas ao perfil técnico do chatbot.

A comparação mostrou que o GPT-OSS-120b apresentou respostas mais detalhadas e com referências mais específicas, especialmente na Q2, onde identificou três modos de carregamento contra dois do GPT-4o-mini. Em um contexto de produção, esse nível de detalhe seria relevante. Para os fins desta Sprint, o GPT-4o-mini foi mantido por já estar integrado ao projeto.