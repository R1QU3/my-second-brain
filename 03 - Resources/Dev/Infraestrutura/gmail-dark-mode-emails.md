# Gmail Mobile e Dark Mode em Emails HTML

**Tags:** #email #html #gmail #dark-mode #resend  
**Área:** [[03 - Resources/Dev/Infraestrutura]]

---

## O Problema

Ao criar um e-mail HTML com fundo escuro (dark theme), o **Gmail mobile** (Android e iOS) renderiza o e-mail completamente em branco — textos somem, layouts quebram. No Gmail desktop isso não acontece.

O usuário só consegue ver o e-mail corretamente clicando em **⋮ → "Ver no tema claro"**.

---

## Por Que Acontece

O Gmail mobile possui um algoritmo de **detecção automática de dark mode**. Quando ele identifica que o `background-color` do e-mail tem luminosidade baixa (fundo escuro), ele assume que o e-mail foi criado para dark mode e aplica uma transformação automática nas cores para se adaptar ao tema do sistema do usuário.

Essa transformação acontece **no lado do cliente, após o download do e-mail** — ou seja, ela ocorre depois que o HTML já foi entregue. Por isso ela não pode ser evitada via CSS ou atributos HTML.

O que o Gmail faz na prática:
- Backgrounds escuros → converte para branco/cinza claro
- Textos claros → converte para escuro
- O resultado é o e-mail aparentemente "em branco"

---

## O Que Não Funciona

Essas abordagens **não resolvem** o problema com o Gmail mobile:

- `!important` em todas as propriedades de cor
- Atributo `bgcolor` nativo nas tabelas
- Meta tags `color-scheme` e `supported-color-schemes`
- Seletor de hack `u + #body-anchor` (específico para Gmail)
- Qualquer combinação das técnicas acima

O Gmail mobile simplesmente ignora tudo isso e aplica a transformação por cima, pois ela ocorre em uma camada abaixo do CSS.

---

## A Solução

**Usar fundo claro.** O Gmail mobile só interfere em e-mails que classifica como "dark". Com fundo claro, ele não aplica nenhuma transformação.

A identidade visual pode ser mantida usando a cor escura/roxa como **acento** sobre fundo claro, em vez de como fundo principal.

### Exemplo de paleta adaptada (dark → light)

| Elemento         | Dark (problemático) | Light (solução)  |
|------------------|---------------------|------------------|
| Fundo wrapper    | `#0d0b14`           | `#f4f1f8`        |
| Fundo container  | `#110f1c`           | `#ffffff`        |
| Texto principal  | `#e8dff5`           | `#2e1f4a`        |
| Texto secundário | `#8c7faa`           | `#7a6b99`        |
| Acento roxo      | `#7c5cbf`           | `#7c5cbf` (igual)|
| Borda            | `#2e2547`           | `#d9d0ea`        |

A cor de acento (`#7c5cbf`) se mantém igual — o que muda é só a inversão de fundo e texto.

---

## Boas Práticas Gerais para E-mails HTML

Além do dark mode, outros pontos importantes ao criar e-mails HTML:

- **Nunca usar `<style>` blocks nem classes CSS** — clientes de e-mail como Gmail removem o `<head>` e tags `<style>`. Todo estilo deve ser **inline** via `style=""` em cada elemento
- **Layout com `<table>`** — usar tabelas aninhadas para estrutura, não `div`/flexbox/grid
- **Fontes seguras** — apenas `Georgia`, `Times New Roman`, `Arial`, `Helvetica`. Web fonts (Google Fonts etc.) não são carregadas de forma confiável
- **`bgcolor` nas tabelas** — usar o atributo HTML nativo além do `style=""` como fallback para clientes mais antigos
- **`color-scheme: light only`** — meta tag útil quando o e-mail é genuinamente claro; sinaliza ao cliente para não tentar adaptar as cores

---

## Contexto de Uso

Descoberto ao criar os e-mails transacionais do **[[01 - Projects/Akasha Oráculo]]** (pré-registro e reset de senha) usando blocos HTML no Resend. Os e-mails chegavam em branco no Gmail mobile até a paleta ser invertida para tema claro.
