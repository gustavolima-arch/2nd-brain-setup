# 2nd Brain — Setup Guide

> **Tempo estimado:** 30–40 minutos. Você não precisa saber programar — siga os passos na ordem e o assistente cuida do resto.

---

## O que você vai ter no final

- Um sistema pessoal de conhecimento no seu Mac (wiki + tarefas + diário)
- Backup automático no GitHub toda vez que você terminar uma sessão
- Um assistente de IA (Claude) que organiza tudo por você

---

## Antes de começar — o que você precisa

- Um **Mac** (este setup é para macOS)
- Uma conta **Claude.ai** com acesso ao Claude Code — [claude.ai](https://claude.ai)
- Uma conta **GitHub** (gratuita) — o assistente ajuda a criar durante o setup
- Cerca de 30–40 minutos sem interrupção

---

## Passo 1 — Abrir o Terminal

O Terminal é o programa onde você vai digitar os comandos de instalação. Fica escondido em Utilitários.

**Como abrir:**
1. Pressione **⌘ + Espaço** (abre o Spotlight)
2. Digite `Terminal`
3. Pressione **Enter**

Uma janela preta ou branca vai abrir com um cursor piscando. É aí que você vai colar os comandos a seguir.

> **Dica:** para colar no Terminal, use **⌘ + V** (não Ctrl+V).

---

## Passo 2 — Instalar o Claude Code

O Claude Code é o assistente que vai conduzir o setup. Ele precisa ser instalado antes de tudo.

Cole este comando no Terminal e pressione **Enter**:

```bash
curl -fsSL https://claude.ai/install.sh | sh
```

Aguarde terminar. Quando aparecer o cursor novamente, continue.

**Se der erro "curl not found"** (raro no Mac):
```bash
/usr/bin/curl -fsSL https://claude.ai/install.sh | sh
```

**Se o comando acima não funcionar**, instale manualmente via Node.js:
```bash
# Instala Homebrew (gerenciador de pacotes do Mac)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Instala Node.js
brew install node

# Instala o Claude Code
npm install -g @anthropic-ai/claude-code
```

> O Homebrew vai pedir sua senha do Mac — é normal. Nada vai aparecer enquanto você digita, mas está sendo registrado.

---

## Passo 3 — Fazer login no Claude Code

Depois de instalar, rode:

```bash
claude
```

Na primeira vez, ele vai pedir para você fazer login. Siga as instruções na tela — vai abrir um link no navegador para autenticar com sua conta Claude.ai.

Quando aparecer o prompt do Claude (algo como `>`), você está dentro. Digite `/exit` para sair por enquanto.

---

## Passo 4 — Baixar o setup do 2nd Brain

Cole este comando no Terminal e pressione **Enter**:

```bash
curl -L https://github.com/gustavolima-arch/2nd-brain-setup/archive/main.zip -o ~/Downloads/setup.zip && unzip ~/Downloads/setup.zip -d ~/Downloads/ && cd ~/Downloads/2nd-brain-setup-main && claude
```

Isso vai:
1. Baixar o setup para a sua pasta Downloads
2. Descompactar automaticamente
3. Entrar na pasta
4. Abrir o Claude Code — que vai iniciar o assistente de setup

**Se der erro "unzip not found":**
```bash
# Descompactar manualmente
cd ~/Downloads && /usr/bin/unzip setup.zip
cd 2nd-brain-setup-main && claude
```

**Se o Claude Code abrir mas não iniciar o setup automaticamente**, escreva:
```
start setup
```

---

## Passo 5 — Seguir o assistente

A partir daqui, **o assistente faz tudo**. Ele vai:

- Criar a estrutura de pastas do seu vault em `~/Documents/2nd-brain/`
- Instalar e configurar o Obsidian (app para visualizar suas notas)
- Criar um repositório privado no GitHub para backup automático
- Configurar o Obsidian Web Clipper (extensão de navegador)
- Explicar como usar o sistema no dia a dia

Você só vai precisar:
- Digitar seu nome e email quando pedido
- Inserir sua senha do Mac uma vez (para o Homebrew)
- Autorizar o GitHub no navegador (o assistente mostra um código)
- Instalar o Obsidian e o Clipper no navegador quando solicitado

---

## Passo 6 — Configurar o Obsidian Clipper (pasta raw/)

Quando o assistente pedir para instalar o Obsidian Clipper, faça assim após instalar a extensão:

1. Clique no ícone do Clipper na barra do navegador
2. Vá em **Settings** (engrenagem)
3. Em **Vault**, selecione `2nd-brain`
4. Em **Default folder**, escreva `raw`
5. Salve

Isso garante que qualquer página que você salvar do navegador vai direto para a caixa de entrada do seu vault.

---

## Passo 7 — Usar o sistema no dia a dia

Depois que o setup terminar, você vai usar assim:

**Abrir o 2nd Brain:**
```bash
cd ~/Documents && claude
```

Ou no Terminal, sempre a partir de `~/Documents/`.

**Comandos principais:**

| O que digitar | O que acontece |
|---|---|
| `today` | Ver tarefas de hoje, reuniões e prioridades |
| `new task: [descrição]` | Criar uma tarefa nova |
| `new idea: [descrição]` | Capturar uma ideia |
| `ingest` | Processar tudo que está na pasta `raw/` |
| `done: [nome da tarefa]` | Marcar uma tarefa como concluída |
| `bye` | Salvar a sessão e fazer backup no GitHub |

---

## Problemas comuns

**"command not found: claude"**
→ O Claude Code não foi instalado corretamente. Volte ao Passo 2 e tente o método manual (Homebrew + Node).

**"command not found: brew"**
→ O Homebrew não foi instalado. Rode o comando de instalação do Homebrew no Passo 2 manualmente.

**O terminal some ou fecha sozinho**
→ Abra de novo via Spotlight (⌘ + Espaço → Terminal). Seu progresso não se perde — os arquivos já criados continuam lá.

**O assistente parou no meio do setup**
→ Abra o Terminal, rode `cd ~/Downloads/2nd-brain-setup-main && claude` e escreva `continue setup`.

**Obsidian não abre após instalar**
→ Procure no Launchpad ou em `/Applications/Obsidian.app`. Se não encontrar, baixe de novo em [obsidian.md](https://obsidian.md).

**Erro de permissão no GitHub**
→ Rode `gh auth login` no Terminal e siga o fluxo de autenticação.

---

## Suporte

Se travar em algum ponto, descreva o erro exato que apareceu no Terminal e o passo em que estava.
