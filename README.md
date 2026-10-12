# 🌙 Jogo da Lua

Uma roleta interativa com 7 setores temáticos sobre a Lua, perguntas de Verdadeiro ou Falso, placar de grupos e um desafio bônus com diagramas ilustrados — desenvolvida em React para uso em sala de aula.

**🔗 Acesse o jogo online:** [Jogo da Lua](https://danielmasson.github.io/Jogo-Da-Lua/)

---

## Sumário

- [Sobre o projeto](#sobre-o-projeto)
- [Créditos](#créditos)
- [Tecnologias utilizadas](#tecnologias-utilizadas)
  - [1. Node.js e npm](#1-nodejs-e-npm-pré-requisito-de-todo-o-projeto)
  - [2. React](#2-react)
  - [3. Vite](#3-vite)
  - [4. CSS com variáveis customizadas](#4-css-com-variáveis-customizadas)
  - [5. Tailwind CSS](#5-tailwind-css)
  - [6. lucide-react (ícones)](#6-lucide-react-ícones)
  - [7. Web Audio API](#7-web-audio-api)
  - [8. GitHub Pages e GitHub Actions](#8-github-pages-e-github-actions)
  - [9. Google Fonts](#9-google-fonts)
- [Roteiro de replicação do ambiente](#roteiro-de-replicação-do-ambiente)
- [Como obter o projeto (clonar o repositório)](#como-obter-o-projeto-clonar-o-repositório)
- [Rodando localmente](#rodando-localmente)
- [Publicação (deploy)](#publicação-deploy)
- [Estrutura do projeto](#estrutura-do-projeto)

---

## Sobre o projeto

Versão em React do jogo original em Python/Tkinter: uma roleta com 7 setores temáticos sobre a Lua, perguntas de Verdadeiro/Falso, placar de 6 grupos e um desafio bônus com diagramas ilustrados. O projeto também oferece uma versão adaptada para dispositivos móveis, em que a roleta sorteia uma pergunta do setor automaticamente.

O jogo é 100% estático (roda inteiro no navegador, sem servidor nem banco de dados) e é publicado no GitHub Pages.

## Créditos

- **Programação:** Daniel Augusto Masson
- **Coordenação:** Fábio Lombardo Evangelista
- **Colaboração:** Gabriela Nietiedt
- **Instituição:** IFC Campus Concórdia

---

## Tecnologias utilizadas

Esta seção documenta as tecnologias empregadas na construção do projeto, o papel de cada uma e um tutorial de instalação para quem deseja replicar o ambiente de desenvolvimento.

| Camada | Tecnologia | Função no projeto |
|---|---|---|
| Linguagem/Runtime | Node.js + JavaScript (ESM) | Base de execução das ferramentas de build |
| Biblioteca de interface | React 18 | Renderização dos componentes e gerenciamento de estado da UI |
| Ferramenta de build/dev server | Vite | Compilação, hot-reload e empacotamento do projeto |
| Estilização | CSS3 (variáveis customizadas) + Tailwind CSS | Aparência visual e sistema de temas |
| Ícones | lucide-react | Ícones vetoriais usados nos botões e controles |
| Áudio | Web Audio API (nativa do navegador) + `<audio>` | Efeitos sonoros sintetizados e trilha de fundo |
| Hospedagem e CI/CD | GitHub Pages + GitHub Actions | Build automático e publicação do site estático |
| Fontes | Google Fonts (Fraunces, Inter, IBM Plex Mono) | Tipografia do projeto |

### 1. Node.js e npm (pré-requisito de todo o projeto)

**O que é:** Node.js é um ambiente de execução JavaScript fora do navegador. O npm (Node Package Manager) é o gerenciador de pacotes que acompanha o Node, usado para instalar bibliotecas e rodar scripts do projeto.

**Por que é necessário:** Todas as demais ferramentas (React, Vite etc.) dependem do Node/npm para serem instaladas e executadas localmente.

**Instalação:**
1. Acesse [nodejs.org](https://nodejs.org) e baixe a versão **LTS** (o projeto exige Node `^20.19.0` ou `>=22.12.0`).
2. Instale seguindo o instalador padrão do seu sistema operacional (Windows, macOS ou Linux).
3. Verifique a instalação no terminal:
   ```bash
   node -v
   npm -v
   ```

### 2. React

**O que é:** React é uma biblioteca JavaScript para construção de interfaces de usuário baseada em componentes reutilizáveis. Permite que a interface reaja automaticamente a mudanças de estado, sem manipulação manual do DOM.

**Papel no projeto:** Todo o jogo — roleta, quiz, placar, controles de áudio, diagramas — é composto por componentes React (`.jsx`), organizados em pastas como `components/` e `hooks/`.

**Instalação (como parte de um projeto novo):**
```bash
npm install react react-dom
```
No `package.json` do projeto, as dependências aparecem como:
```json
"dependencies": {
  "react": "^18.3.1",
  "react-dom": "^18.3.1"
}
```

### 3. Vite

**O que é:** Vite é uma ferramenta de build e servidor de desenvolvimento moderna, criada para substituir ferramentas mais lentas (como o Webpack clássico). Ele oferece inicialização quase instantânea e *hot module replacement* (atualização da tela sem recarregar a página inteira).

**Papel no projeto:** É o Vite que compila os arquivos `.jsx`/`.css`, serve o projeto em desenvolvimento (`npm run dev`) e gera o pacote final otimizado para produção (`npm run build`).

**Instalação:**
```bash
npm install --save-dev vite @vitejs/plugin-react
```

**Criando um projeto do zero com Vite (alternativa rápida para replicar a base):**
```bash
npm create vite@latest jogo-da-lua -- --template react
cd jogo-da-lua
npm install
```

**Scripts padrão usados no projeto (`package.json`):**
```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview"
}
```
- `npm run dev` → inicia o servidor local de desenvolvimento.
- `npm run build` → gera os arquivos finais (HTML/CSS/JS) na pasta `dist/`, prontos para hospedagem.
- `npm run preview` → serve localmente o resultado do build, simulando produção.

**Arquivo de configuração (`vite.config.js`):**
```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  base: "/Jogo-Da-Lua/", // deve ser igual ao nome do repositório no GitHub
  plugins: [react(), tailwindcss()],
});
```
> A opção `base` é obrigatória para o GitHub Pages: o site é servido em `https://<usuario>.github.io/<repositorio>/`, e não na raiz do domínio. Os caminhos dos arquivos de áudio em `public/` já usam `import.meta.env.BASE_URL` para respeitar esse prefixo.

### 4. CSS com variáveis customizadas

**O que é:** Recurso nativo do CSS3 que permite declarar variáveis reutilizáveis (cores, fontes, espaçamentos) dentro de um seletor e referenciá-las em qualquer regra de estilo com `var(--nome-da-variavel)`.

**Papel no projeto:** Todo o tema visual (paleta de cores "noturna", tipografia, tons de destaque) é centralizado em variáveis declaradas no seletor raiz do jogo. Exemplo (`jogo-da-lua.css`):
```css
.jdl-root {
  --jdl-void: #0b1420;
  --jdl-gold: #f0d889;
  --jdl-mist: #8ca0c4;
  --jdl-display: "Fraunces", Georgia, serif;
  --jdl-body: "Inter", system-ui, sans-serif;
}
```

**Instalação:** Não requer instalação — é um recurso nativo do navegador, presente em qualquer arquivo `.css` importado no projeto (`import "./styles/jogo-da-lua.css"`).

### 5. Tailwind CSS

**O que é:** Framework CSS utilitário que fornece classes prontas (como `flex`, `p-4`, `text-center`) para estilização rápida, sem escrever CSS customizado do zero.

**Papel no projeto:** O plugin do Tailwind está configurado no `vite.config.js` e o arquivo `src/App.css` contém a diretiva `@import "tailwindcss";`. Atualmente a identidade visual do jogo vem quase toda do CSS customizado (`jogo-da-lua.css`); o Tailwind fica disponível como camada de utilitários complementar. Para ativá-lo de fato, importe o `App.css` no `main.jsx` (`import "./App.css";`).

**Instalação (Tailwind v4, compatível com a sintaxe `@import "tailwindcss"`):**
```bash
npm install tailwindcss @tailwindcss/vite
```
> Apenas importar `tailwindcss` no CSS não é suficiente — o pacote e o plugin do Vite precisam estar de fato instalados, ou o build falha ao processar essa diretiva.

### 6. lucide-react (ícones)

**O que é:** Biblioteca de ícones SVG em formato de componentes React, com um conjunto amplo e consistente de ícones.

**Papel no projeto:** Usada em elementos de interface como os botões de volume/mudo (`Volume2`, `VolumeX`), incremento/decremento de pontuação (`Plus`, `Minus`) e ajuda (`HelpCircle`, `Sparkles`, `X`).

**Instalação:**
```bash
npm install lucide-react
```

**Uso típico:**
```jsx
import { Volume2, VolumeX } from "lucide-react";
<Volume2 size={18} />
```

### 7. Web Audio API

**O que é:** API nativa dos navegadores modernos para geração e processamento de áudio em tempo real, sem depender de arquivos de som externos.

**Papel no projeto:** Usada para sintetizar efeitos sonoros (cliques, acertos, erros, giro da roleta) diretamente via osciladores (`AudioContext`, `OscillatorNode`, `GainNode`), com uso automático de arquivos `.mp3` quando existirem em `public/sounds/`. A música de fundo é um arquivo `public/music/trilha.mp3` tocado por um elemento `<audio>`, com controle de volume/mudo salvo no `localStorage`.

**Instalação:** Não requer instalação — é nativa do navegador. Basta acessá-la via `window.AudioContext` (com fallback `window.webkitAudioContext` para compatibilidade com navegadores mais antigos).

### 8. GitHub Pages e GitHub Actions

**O que é:**
- **GitHub Pages:** serviço gratuito do GitHub para hospedar sites estáticos diretamente a partir de um repositório.
- **GitHub Actions:** sistema de automação (CI/CD) do GitHub, usado aqui para compilar o projeto e publicá-lo a cada alteração.

**Papel no projeto:** A cada `push` na branch `main`, o workflow `.github/workflows/deploy.yml` instala as dependências (`npm ci`), gera o build (`npm run build`) e publica a pasta `dist/` no GitHub Pages. Não há backend, banco de dados nem variáveis de ambiente/segredos para configurar.

**Configuração (uma única vez, no repositório):**
1. No GitHub, abra **Settings → Pages**.
2. Em **Build and deployment → Source**, selecione **GitHub Actions**.
3. Garanta que o valor de `base` no `vite.config.js` seja igual ao nome do repositório (com a mesma grafia de maiúsculas/minúsculas).
4. Faça um `push` na `main` — o site ficará disponível em `https://<usuario>.github.io/<repositorio>/`.

### 9. Google Fonts

**O que é:** Serviço gratuito do Google para hospedagem e distribuição de fontes web via CDN.

**Papel no projeto:** Fornece as três famílias tipográficas usadas na identidade visual — **Fraunces** (títulos), **Inter** (texto corrido) e **IBM Plex Mono** (elementos técnicos/numéricos, como o placar).

**Instalação:** Não requer pacote npm — é feita por uma única linha de importação no topo do CSS principal:
```css
@import url("https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,600;9..144,700&family=Inter:wght@400;500;600;700&family=IBM+Plex+Mono:wght@500;600&display=swap");
```

---

## Roteiro de replicação do ambiente

```bash
# 1. Pré-requisito
# instalar Node.js LTS (inclui npm)

# 2. Criar o projeto base com Vite + React
npm create vite@latest jogo-da-lua -- --template react
cd jogo-da-lua

# 3. Instalar dependências principais
npm install react react-dom lucide-react
npm install --save-dev vite @vitejs/plugin-react tailwindcss @tailwindcss/vite

# 4. Rodar em desenvolvimento
npm run dev

# 5. Gerar build de produção
npm run build

# 6. (Opcional) Publicar no GitHub Pages
#    - subir o código para um repositório no GitHub
#    - Settings → Pages → Source: GitHub Actions
#    - manter o workflow .github/workflows/deploy.yml e o "base" do vite.config.js
```

---

## Como obter o projeto (clonar o repositório)

Esta etapa é diferente de "criar um projeto novo com Vite" (visto no roteiro acima) — aqui você baixa o código-fonte já existente do Jogo da Lua para a sua máquina.

1. **Instale o Git** (se ainda não tiver):
   - [git-scm.com/downloads](https://git-scm.com/downloads)
   - Verifique com:
     ```bash
     git --version
     ```

2. **Clone o repositório:**
   ```bash
   git clone https://github.com/DanielMasson/Jogo-da-Lua.git
   ```
   Isso cria uma pasta local (`Jogo-da-Lua/`) com todo o código do projeto — já é a pasta do projeto React, não é necessário criar uma nova com `npm create vite`.

3. **Entre na pasta do projeto:**
   ```bash
   cd Jogo-da-Lua
   ```

4. **Instale as dependências declaradas no `package.json`:**
   ```bash
   npm install
   ```
   Esse comando lê o `package.json`/`package-lock.json` do repositório e baixa automaticamente React, Vite, lucide-react e demais pacotes já configurados no projeto — não é preciso reinstalar cada tecnologia manualmente como no roteiro de "criar do zero".

5. **Siga para a seção [Rodando localmente](#rodando-localmente)** para iniciar o servidor de desenvolvimento. Não é necessária nenhuma configuração de chaves ou credenciais.

---

## Rodando localmente

```bash
npm install
npm run dev
```

Abra o endereço mostrado no terminal. Por causa da opção `base` do Vite, o endereço local inclui o prefixo do repositório (normalmente `http://localhost:5173/Jogo-Da-Lua/`).

Para testar o build de produção localmente:

```bash
npm run build
npm run preview
```

---

## Publicação (deploy)

A publicação é automática: todo `push` na branch `main` dispara o workflow **Deploy GitHub Pages** (`.github/workflows/deploy.yml`), que compila o projeto e atualiza o site. O andamento pode ser acompanhado na aba **Actions** do repositório.

---

## Estrutura do projeto

```
.github/
└── workflows/
    └── deploy.yml          # build + deploy automático no GitHub Pages

public/
├── music/trilha.mp3        # música de fundo (opcional)
└── sounds/                 # efeitos sonoros em .mp3 (opcionais)

src/
├── data/                 # dados estáticos do jogo (nenhuma lógica aqui)
│   ├── perguntas.js       # as 70 perguntas de V/F, por setor
│   ├── setores.js         # letra + título de cada um dos 7 setores
│   └── bonus.js           # as 4 perguntas do desafio bônus
│
├── hooks/
│   ├── useJogoDaLua.js     # todo o estado e as regras do jogo (desktop)
│   ├── useJogoMobile.js    # lógica equivalente para o modo mobile
│   ├── useSorteioAleatorio.js
│   ├── useIsMobile.js
│   └── useSfx.js
│
├── context/
│   └── AudioProvider.jsx   # estado global de volume/mudo da música de fundo
│
├── utils/
│   ├── geometry.js         # helpers de SVG usados pela roleta
│   ├── sfx.js               # motor de efeitos sonoros globais
│   └── sound.js             # motor de efeitos sonoros do modo mobile
│
├── components/
│   ├── Header.jsx
│   ├── Footer.jsx
│   ├── BonusPanel.jsx      # autocontido: pergunta bônus + modal de resposta
│   ├── BonusModal.jsx
│   ├── Roleta.jsx          # roleta + botão Girar (desktop)
│   ├── RoletaSvg.jsx       # desenho da roleta, compartilhado entre telas
│   ├── QuizPanel.jsx       # grade de perguntas + pergunta/correção
│   ├── Scoreboard.jsx      # placar dos 6 grupos
│   ├── MobileQuiz.jsx      # versão mobile do quiz
│   ├── MoonIllustration.jsx
│   ├── AudioControls.jsx   # botão flutuante de volume/mudo
│   └── diagrams/           # os 4 diagramas de resposta do bônus
│       ├── FaseLua.jsx
│       ├── DiagramaEscala.jsx
│       ├── DiagramaInclinacao.jsx
│       ├── DiagramaPosicoes.jsx
│       ├── DiagramaFases.jsx
│       └── index.js        # mapa id -> componente
│
├── styles/
│   └── jogo-da-lua.css
│
├── JogoDaLua.jsx           # junta os hooks com os componentes visuais
├── App.jsx
├── App.css                 # diretiva do Tailwind
└── main.jsx                # entrada do Vite
```
