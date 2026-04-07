# Gemma 4 - Rodando Local

> **Data:** 2026-04-06
> **Tags:** #ai #llm #local #ollama #google

---

## O que e o Gemma 4?

Lancado em 2 de abril de 2026 pelo Google DeepMind, o **Gemma 4** e uma familia de modelos open-weight construidos a partir da mesma pesquisa por tras do Gemini 3. Diferente das versoes anteriores, todos os modelos agora saem com licenca **Apache 2.0** — ou seja, pode usar pra qualquer coisa, inclusive comercial, sem restricao.

### Destaques

- **Contexto de ate 256K tokens** — equivale a ~500 paginas de texto
- **Multimodal** — processa texto, imagens e audio (nos modelos E2B e E4B)
- **Multilingual** — suporte a mais de 140 idiomas
- **Reasoning avancado** — projetado pra workflows agenticos e raciocinio complexo
- **Roda 100% local** — sem API key, sem custo, sem dados saindo da sua maquina

### Benchmarks vs Gemma 3

| Benchmark         | Gemma 3 | Gemma 4 | Salto     |
| ----------------- | ------- | ------- | --------- |
| AIME 2026 (math)  | 20.8%   | 89.2%   | **+329%** |
| LiveCodeBench      | 29.1%   | 80.0%   | **+175%** |
| GPQA Diamond (sci) | 42.4%   | 84.3%   | **+99%**  |

---

## Variantes disponiveis

| Modelo       | Params totais | Params ativos | Download | VRAM minima | Indicado pra                                 |
| ------------ | ------------- | ------------- | -------- | ----------- | -------------------------------------------- |
| `gemma4:e2b` | 2.3B          | 2.3B (dense)  | ~1.5 GB  | ~4 GB       | Tarefas leves, maquinas antigas, edge/mobile |
| `gemma4:e4b` | 4.5B          | 4.5B (dense)  | ~3 GB    | ~6 GB       | **Sweet spot** — recomendado pra maioria     |
| `gemma4:26b` | 25.2B         | 3.8B (MoE)    | ~18 GB   | ~8 GB       | Qualidade perto de 31B, velocidade de 4B     |
| `gemma4:31b` | 30.7B         | 30.7B (dense) | ~20 GB   | ~20 GB+     | RTX 4090 ou Apple Silicon 32GB+              |

### Por que o 26B e impressionante?

O modelo 26B usa arquitetura **Mixture of Experts (MoE)** com **128 especialistas**, mas so **8 especialistas + 1 compartilhado** sao ativados por token. Isso significa:

- Apenas **3.8B parametros ativos** por inferencia (de 25.2B totais)
- **8x menos compute** por passo comparado ao modelo denso de 31B
- Atinge **97% da qualidade** do modelo denso 31B
- No GPQA Diamond, o 26B MoE (79.2%) bateu o GPT-oss-120B da OpenAI (76.2%) — um modelo com quase 5x mais parametros

---

## Instalacao via Ollama (WSL/Linux)

### 1. Instalar o Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Verificar se instalou corretamente:

```bash
ollama --version
```

### 2. Iniciar o servico (se necessario)

No WSL, o systemd pode nao estar ativo. Se `ollama run` der erro de conexao:

```bash
# Inicia o servidor em background
ollama serve &
```

### 3. Baixar e rodar o modelo

```bash
# Padrao (e4b) — melhor custo-beneficio pra maioria
ollama run gemma4

# Modelo minimo — pra maquinas com pouca RAM
ollama run gemma4:e2b

# MoE 26B — melhor qualidade sem precisar de GPU monstro
ollama run gemma4:26b

# Denso 31B — so se tiver hardware pesado
ollama run gemma4:31b
```

O primeiro `run` faz o download automaticamente. Depois de baixado, ele inicia direto.

### 4. Usar via API (opcional)

O Ollama sobe um servidor local compativel com a API do OpenAI em `http://localhost:11434`:

```bash
# Testar via curl
curl http://localhost:11434/api/generate -d '{
  "model": "gemma4",
  "prompt": "Explique o que e MoE em modelos de linguagem",
  "stream": false
}'

# Listar modelos baixados
ollama list

# Remover um modelo
ollama rm gemma4:e2b
```

### 5. Dicas de performance

- **Sem GPU dedicada?** O Ollama roda na CPU, so vai ser mais lento. Prefira `e2b` ou `e4b`
- **Tem GPU NVIDIA no WSL?** Instale o [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit) e o Ollama detecta automaticamente
- **Quer interface grafica?** Suba o [Open WebUI](https://github.com/open-webui/open-webui) pra ter um chat estilo ChatGPT apontando pro Ollama local
- **Keep-alive:** por padrao o modelo fica carregado na memoria por 5 minutos apos o ultimo uso. Pra manter carregado: `ollama run gemma4 --keepalive -1`

---

## Links uteis

- [Blog oficial do Google sobre o Gemma 4](https://blog.google/innovation-and-ai/technology/developers-tools/gemma-4/)
- [Google DeepMind - Pagina do Gemma 4](https://deepmind.google/models/gemma/gemma-4/)
- [Documentacao Ollama + Gemma](https://ai.google.dev/gemma/docs/integrations/ollama)
- [Hugging Face - Gemma 4](https://huggingface.co/blog/gemma4)
- [Guia completo no DEV Community](https://dev.to/linnn_charm_2e397112f3b51/gemma-4-complete-guide-architecture-models-and-deployment-in-2026-3m5b)

---

**Veja tambem:** [[Ollama]] | [[LLMs Locais]]
