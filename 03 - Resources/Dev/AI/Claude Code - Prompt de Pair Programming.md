# Claude Code - Prompt de Pair Programming

> **Data:** 2026-04-09
> **Tags:** #ai #claude-code #prompt #pair-programming #workflow

---

## O que é o Prompt de Pair Programming?

Quando você usa o Claude Code pra tarefas grandes — migrações, refatorações, criação de features complexas — ele tende a sair executando tudo de uma vez. Isso pode dar merda: ele remove arquivos sem avisar, toma decisões de design sozinho, e às vezes entrega um resultado que funciona tecnicamente mas não era o que você queria.

O **Prompt de Pair Programming** é uma instrução estruturada que você passa no início da conversa pra mudar esse comportamento. Em vez de deixar a IA no modo "piloto automático", você coloca ela no modo **co-piloto**: ela executa, mas você revisa e aprova cada passo antes de seguir.

Pense nisso como uma **hierarquia de comando**: você é o tech lead, o Claude é o dev senior que sabe codar mas precisa da sua aprovação antes de mergear qualquer coisa.

---

## Por que usar?

### O problema sem o prompt

Sem instrução clara, o Claude Code tende a:

- **Executar tudo de uma vez** — faz 15 mudanças sem parar pra você revisar
- **Tomar decisões de design sozinho** — escolhe padrões, renomeia coisas, refatora código que você não pediu
- **Remover arquivos sem avisar** — "limpou" algo que você ainda precisava
- **Não perguntar quando tem dúvida** — assume e segue em frente, mesmo quando a decisão não é óbvia

### O que o prompt resolve

Com o prompt de pair programming, você ganha:

- **Controle granular** — cada etapa significativa para pra revisão antes de continuar
- **Transparência** — a IA avisa o que pretende fazer e por quê antes de agir
- **Segurança** — nada é removido ou alterado sem seu ok explícito
- **Decisões melhores** — quando encontra algo ambíguo, ela pergunta em vez de chutar
- **Orquestração inteligente** — o Claude principal atua como gerente, delegando tarefas pra sub-agentes e consolidando os resultados pra você

É como a diferença entre dar a chave do carro pra alguém e dizer "me leva no aeroporto" vs "vai dirigindo, eu vou dizendo o caminho". Nos dois casos você chega, mas no segundo você tem controle da rota.

---

## O Prompt

Copie e adapte esse template pro seu contexto. As partes entre `[colchetes]` são placeholders pra você preencher.

```markdown
## O que preciso que você faça

Quero que você atue como meu **pair programmer** nessa tarefa: [descreva a tarefa aqui].
Você executa, eu reviso, testo e aprovo.

Antes de qualquer coisa, explore o projeto para entender o estado atual.

## Como vamos trabalhar

- Trabalhe como **orquestrador**: lance sub-agentes para executar planos e criar planos.
  Você é o chefe deles, eu sou o seu chefe.
- Me avise quando terminar cada etapa significativa para eu revisar antes de continuar.
- Se encontrar algo inesperado ou precisar tomar uma decisão de design que não está
  coberta por essas instruções, **me pergunte antes de agir**.

## Restrições importantes

- **Antes de remover qualquer arquivo ou configuração existente**, me avise o que pretende
  remover e por quê.
- Não refatore ou "melhore" código que não faz parte do escopo da tarefa.
- Não crie arquivos ou pastas desnecessárias.
- Se algo pode ser feito de mais de um jeito, me apresente as opções com prós e contras
  antes de escolher.
```

---

## Explicação de cada seção

### "O que preciso que você faça"

Define o escopo e o modelo de trabalho. A frase **"Você executa, eu reviso, testo e aprovo"** é a mais importante — ela estabelece que a decisão final é sempre sua. Pedir pra ele explorar o projeto antes de começar evita que ele assuma coisas sobre a estrutura do código.

### "Como vamos trabalhar"

Aqui você define a **dinâmica de comunicação**. O ponto sobre sub-agentes é relevante porque o Claude Code pode lançar agentes especializados (um pra pesquisar, outro pra implementar, outro pra revisar). Sem essa instrução, ele faz tudo num agente só, o que é mais lento e menos organizado.

A metáfora da hierarquia funciona bem: o Claude principal é um gerente de projeto que delega pros devs (sub-agentes) e reporta pra você (o stakeholder).

### "Restrições importantes"

São os **guardrails**. Sem eles, o Claude tende a ser "proativo demais" — ele vê um código feio e refatora, vê um arquivo que parece obsoleto e deleta. Essas restrições forçam ele a pedir permissão antes de qualquer ação destrutiva ou fora do escopo.

### "Regras de estilo e contexto"

Essa seção é **customizável por projeto**. Você adapta conforme o que precisa. Exemplos de regras que podem entrar aqui:

- Idioma dos comentários e commits
- Padrões de código (naming conventions, estrutura de pastas)
- Framework ou lib específica que deve ser usada
- Tom da documentação (técnico, casual, formal)

---

## Dicas de uso

- **Não precisa usar o prompt inteiro toda vez.** Pra tarefas pequenas, às vezes basta a seção de restrições. O template completo é pra tarefas grandes (migrações, refatorações, novos módulos).
- **Salve uma versão no CLAUDE.md do projeto.** Se colocar as restrições e regras de estilo direto no `CLAUDE.md` do repositório, o Claude Code já carrega automaticamente em toda conversa — não precisa ficar colando o prompt toda vez.
- **Ajuste a "rédea" conforme confiança.** Se já testou bastante e confia no resultado, pode afrouxar: "pode seguir sem parar pra revisar nas próximas 3 etapas". Se é algo crítico, aperta: "quero aprovar cada arquivo individualmente".

---

**Veja também:** [[LLMs Locais]]
