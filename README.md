# SlothReSkin — Documentação Completa

> Extensão para Chrome que substitui a ficha de personagem padrão do Roll20 pela sua própria ficha personalizada em HTML.

---

## Sumário

- [SlothReSkin — Documentação Completa](#slothreskin--documentação-completa)
  - [Sumário](#sumário)
  - [1. O que é o SlothReSkin?](#1-o-que-é-o-slothreskin)
    - [Como funciona na prática?](#como-funciona-na-prática)
    - [O que a extensão NÃO faz](#o-que-a-extensão-não-faz)
  - [2. Como instalar a extensão](#2-como-instalar-a-extensão)
    - [Passo a passo](#passo-a-passo)
  - [3. Como usar o popup (menu da extensão)](#3-como-usar-o-popup-menu-da-extensão)
    - [Toggle ON/OFF](#toggle-onoff)
    - [Aviso de recarga](#aviso-de-recarga)
    - [Carregar HTML](#carregar-html)
    - [Carregar CSS (opcional)](#carregar-css-opcional)
    - [Carregar config.json](#carregar-configjson)
    - [Limpar tudo](#limpar-tudo)
    - [Auto-refresh ao alterar](#auto-refresh-ao-alterar)
  - [4. Carregando um template](#4-carregando-um-template)
    - [Onde encontrar templates](#onde-encontrar-templates)
    - [Como usar](#como-usar)
  - [5. Criando seu próprio template — Guia do criador](#5-criando-seu-próprio-template--guia-do-criador)
    - [5.1 O arquivo `config.json`](#51-o-arquivo-configjson)
      - [Estrutura completa](#estrutura-completa)
      - [Explicação de cada campo](#explicação-de-cada-campo)
    - [5.2 O arquivo HTML do template](#52-o-arquivo-html-do-template)
      - [Exemplo mínimo de template](#exemplo-mínimo-de-template)
    - [5.3 Atributos especiais `data-sf-*`](#53-atributos-especiais-data-sf-)
      - [Exibir um valor (somente leitura)](#exibir-um-valor-somente-leitura)
      - [Campos embutidos (sem precisar declarar no config.json)](#campos-embutidos-sem-precisar-declarar-no-configjson)
      - [Interpolação com `{{tag}}`](#interpolação-com-tag)
    - [5.4 Seções de texto livre (`freetext`)](#54-seções-de-texto-livre-freetext)
    - [5.5 Listas dinâmicas (`data-sf-list`)](#55-listas-dinâmicas-data-sf-list)
      - [Estrutura HTML de uma lista dinâmica](#estrutura-html-de-uma-lista-dinâmica)
      - [Como funciona](#como-funciona)
      - [Atributos disponíveis dentro do `<template>`](#atributos-disponíveis-dentro-do-template)
      - [Exemplo: lista de inventário em grade](#exemplo-lista-de-inventário-em-grade)
    - [5.6 Abas e painéis](#56-abas-e-painéis)
      - [Exemplo básico](#exemplo-básico)
      - [Abas aninhadas (sub-abas)](#abas-aninhadas-sub-abas)
      - [Estilizando com CSS](#estilizando-com-css)
    - [5.7 Botões de habilidade (rolagens)](#57-botões-de-habilidade-rolagens)
    - [5.8 Imagens e avatar](#58-imagens-e-avatar)
    - [5.9 Edição inline de campos](#59-edição-inline-de-campos)
    - [5.10 Barras de HP (`data-sf-hpbar`)](#510-barras-de-hp-data-sf-hpbar)
  - [6. Referência completa dos atributos `data-sf-*`](#6-referência-completa-dos-atributos-data-sf-)
  - [7. Referência do `config.json`](#7-referência-do-configjson)
  - [8. Como os dados são salvos (arquitetura)](#8-como-os-dados-são-salvos-arquitetura)
    - [Camadas da extensão](#camadas-da-extensão)
    - [Como a leitura funciona](#como-a-leitura-funciona)
    - [Como a escrita funciona](#como-a-escrita-funciona)
    - [Armazenamento de texto livre — pipeline completo (`_sf_data`)](#armazenamento-de-texto-livre--pipeline-completo-_sf_data)
      - [Por que um único atributo?](#por-que-um-único-atributo)
      - [Leitura na abertura da ficha](#leitura-na-abertura-da-ficha)
      - [Renderização no injector](#renderização-no-injector)
      - [Escrita de um item editado](#escrita-de-um-item-editado)
      - [Atualização em tempo real após salvar](#atualização-em-tempo-real-após-salvar)
      - [Diagrama resumido](#diagrama-resumido)
    - [Onde o template é armazenado](#onde-o-template-é-armazenado)
  - [9. Perguntas frequentes (FAQ)](#9-perguntas-frequentes-faq)

---

## 1. O que é o SlothReSkin?

O Roll20 é um site de RPG de mesa online. Ele possui fichas de personagem padrão que nem sempre agradam a todos os mestres e jogadores. O SlothReSkin é uma extensão para o navegador Google Chrome que **sobrepõe visualmente** a ficha padrão do Roll20 pela sua própria ficha feita em HTML, sem alterar nada nos servidores do Roll20.

### Como funciona na prática?

1. Você abre uma sessão no Roll20 normalmente.
2. Quando abre a ficha de um personagem, a extensão **esconde a ficha original** e exibe a **sua ficha personalizada** no lugar.
3. Todos os dados (atributos, valores, textos) continuam sendo lidos e salvos diretamente no Roll20 — a extensão só muda a aparência.
4. As rolagens de dados (abilities) continuam funcionando normalmente, pois a extensão usa o próprio sistema de rolagem do Roll20 por trás dos panos.

### O que a extensão NÃO faz

- Não envia nenhum dado para servidores externos.
- Não tenta hackear ou contornar a API do Roll20.
- Não compartilha sua ficha com outros jogadores (cada um precisa ter a extensão instalada).

---

## 2. Como instalar a extensão

O SlothReSkin está disponível nas lojas oficiais dos principais navegadores baseados em Chromium:

| Navegador | Loja |
|---|---|
| Google Chrome | Chrome Web Store |
| Microsoft Edge | Edge Add-ons |
| Brave / Opera / Vivaldi | Chrome Web Store (compatível) |

### Passo a passo

1. Acesse a página do SlothReSkin na loja do seu navegador e clique em **"Adicionar ao Chrome"** (ou "Adicionar ao Edge").

2. Confirme a instalação na janela de permissões que aparecer. A extensão pedirá acesso apenas ao site `roll20.net`.

3. Após instalada, o ícone do SlothReSkin (o sloth laranja) aparecerá na barra do navegador.

4. Se o ícone não aparecer automaticamente, clique no ícone de quebra-cabeça (🧩) na barra do Chrome/Edge e fixe o SlothReSkin clicando no alfinete ao lado do nome.

---

## 3. Como usar o popup (menu da extensão)

Clique no ícone do SlothReSkin na barra do Chrome para abrir o menu da extensão.

```
┌─────────────────────────────────────┐
│  SlothReSkin              v0.1 ● ON │
├─────────────────────────────────────┤
│   ⚠ Recarregue a página do Roll20   │
│       para aplicar as alterações    │
├─────────────────────────────────────┤
│  TEMPLATE                           │
│  ✓ minha-ficha.html                 │
│  [  Carregar HTML  ]                │
│  [  Carregar CSS   ]                │
├─────────────────────────────────────┤
│  CONFIGURAÇÃO DE CAMPOS             │
│  ✓ MeuSistema                       │
│  [  Carregar config.json  ]         │
├─────────────────────────────────────┤
│  [       Limpar tudo       ]        │
├─────────────────────────────────────┤
│  □ Auto-refresh ao alterar          │
└─────────────────────────────────────┘
```

### Toggle ON/OFF

- O botão **ON/OFF** no canto superior direito ativa ou desativa a extensão completamente.
- **Verde (ON):** a extensão está ativa e substituirá as fichas ao abrir o Roll20.
- **Cinza (OFF):** a extensão está inativa, o Roll20 funciona normalmente como antes.
- Qualquer alteração no toggle exige **recarregar a página do Roll20** para ter efeito.

### Aviso de recarga

Sempre que você alterar qualquer configuração (carregar um novo template, trocar o config, ligar/desligar), aparece um aviso laranja no topo do popup pedindo para recarregar a página. Isso é necessário porque a extensão injeta o template quando a página carrega.

### Carregar HTML

Clique em **"Carregar HTML"** e selecione o arquivo `.html` do seu template. Este é o arquivo que define a aparência da ficha.

### Carregar CSS (opcional)

Se quiser separar os estilos visuais do HTML, clique em **"Carregar CSS"** e selecione um arquivo `.css`. Os estilos serão aplicados junto ao HTML.

### Carregar config.json

Clique em **"Carregar config.json"** e selecione o arquivo de configuração do seu sistema de RPG. Este arquivo diz à extensão quais campos do Roll20 correspondem às tags do seu template.

### Limpar tudo

O botão vermelho **"Limpar tudo"** remove o template e a configuração do armazenamento da extensão. Após limpar, as fichas voltarão a mostrar a ficha padrão do Roll20 (após recarregar a página).

### Auto-refresh ao alterar

Quando esta caixa está marcada, toda vez que você alterar qualquer configuração no popup, o Chrome recarregará automaticamente a aba do Roll20 ativa — sem precisar fazer isso manualmente.

---

## 4. Carregando um template

Para usar o SlothReSkin você precisa de dois arquivos: um **template HTML** (a aparência da ficha) e um **config.json** (os campos do seu sistema de RPG). Esses arquivos podem vir de fontes diferentes:

### Onde encontrar templates

- **Loja de templates SlothForge** — templates prontos, com suporte, para os sistemas de RPG mais populares.
- **Comunidade** — criadores independentes que disponibilizam seus templates gratuitamente ou pagos em sites e fóruns de RPG.
- **Criação própria** — você mesmo pode criar um template do zero seguindo o [Guia do criador](#5-criando-seu-próprio-template--guia-do-criador) na seção 5.

### Como usar

Independente da origem do template, o processo é sempre o mesmo:

1. Abra o popup da extensão clicando no ícone do SlothReSkin na barra do navegador.
2. Clique em **"Carregar HTML"** e selecione o arquivo `.html` do template.
3. Clique em **"Carregar config.json"** e selecione o arquivo `.json` correspondente.
4. Recarregue a página do Roll20 (F5) — o aviso laranja no popup lembrará você disso.
5. Entre em uma campanha e abra a ficha de um personagem.

> **Atenção:** o `config.json` precisa corresponder ao mesmo sistema do template HTML. Misturar arquivos de sistemas diferentes fará os campos aparecerem errados ou vazios.

---

## 5. Criando seu próprio template — Guia do criador

Esta seção assume conhecimento prévio de HTML, CSS e JSON. Não há introdução a conceitos web — o foco é descrever o que a extensão oferece e como integrá-la ao seu HTML.

> **Gerando templates com IA:** modelos como Claude, ChatGPT e Gemini conseguem criar templates compatíveis com a extensão quando alimentados com esta documentação. Passe a seção 5 completa (e o `config.json` do seu sistema) como contexto — a IA entenderá os atributos `data-sf-*`, o formato do config e as restrições técnicas (como a regra de abas).

### 5.1 O arquivo `config.json`

O `config.json` é o "dicionário" que conecta as **tags** do seu HTML aos **nomes dos campos** no Roll20.

#### Estrutura completa

```json
{
  "system_name": "NomeDoSeuSistema",

  "fields": [
    { "roll20": "PV's",     "tag": "pv"     },
    { "roll20": "Força",    "tag": "forca"  },
    { "roll20": "Destreza", "tag": "des"    }
  ],

  "freetext": ["pericias", "inventario", "notas"],

  "abilities": [
    { "roll20": "Ataque",   "tag": "ataque" },
    { "roll20": "Defesa",   "tag": "defesa" }
  ]
}
```

#### Explicação de cada campo

| Propriedade | Obrigatório | Descrição |
|---|---|---|
| `system_name` | Sim | Nome exibido no popup ao carregar o config |
| `fields` | Sim | Lista de campos numéricos/texto do Roll20 |
| `fields[].roll20` | Sim | **Nome exato** do atributo como aparece no Roll20 (aba de atributos da ficha) |
| `fields[].tag` | Sim | Tag curta que você usará no HTML (sem espaços, sem acentos) |
| `freetext` | Não | Lista de seções de texto livre (listas dinâmicas ou caixas de texto) |
| `abilities` | Não | Lista de abilities (macros) que podem ser roladas via botões |
| `abilities[].roll20` | Sim | **Nome exato** da ability como aparece na aba de abilities do Roll20 |
| `abilities[].tag` | Sim | Tag que você usará no atributo `data-sf-ability` do HTML |

> **Dica:** Para descobrir o nome exato de um campo no Roll20, abra a ficha padrão, vá na aba "Atributos e Habilidades" e copie o nome do campo exatamente como aparece, incluindo maiúsculas e caracteres especiais.

---

### 5.2 O arquivo HTML do template

O template é um arquivo HTML normal com algumas convenções especiais:

- Os **estilos CSS** ficam dentro de uma tag `<style>` no `<head>`.
- O **conteúdo** fica dentro do `<body>`.
- Os valores do Roll20 são inseridos usando **tags especiais** no HTML.

#### Exemplo mínimo de template

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <style>
    body {
      background: #111;
      color: #eee;
      font-family: sans-serif;
      padding: 12px;
    }
    h1 { color: #ff6600; }
  </style>
</head>
<body>

  <h1 data-sf="_name">{{_name}}</h1>
  <p>Vida: <strong data-sf="pv">{{pv}}</strong></p>
  <p>Força: <strong data-sf="forca">{{forca}}</strong></p>

</body>
</html>
```

---

### 5.3 Atributos especiais `data-sf-*`

Estes são os atributos HTML que a extensão reconhece para exibir e sincronizar dados.

#### Exibir um valor (somente leitura)

```html
<!-- Exibe o valor atual do campo "pv" -->
<span data-sf="pv">{{pv}}</span>

<!-- Exibe o valor máximo do campo "pv" -->
<span data-sf-max="pv">{{pv_max}}</span>

<!-- Exibe o nome do personagem (campo embutido) -->
<span data-sf="_name">{{_name}}</span>
```

> O texto entre `{{` e `}}` é o valor inicial renderizado quando a ficha abre. O atributo `data-sf` mantém o valor sincronizado quando o Roll20 atualiza o campo em tempo real.

#### Campos embutidos (sem precisar declarar no config.json)

Os três campos abaixo existem automaticamente para todo personagem e não precisam ser declarados no `config.json`.

---

##### `_name` — Nome do personagem

Leitura síncrona. Disponível imediatamente quando a ficha abre.

```html
<!-- Como texto simples -->
<span data-sf="_name">{{_name}}</span>

<!-- Em atributos HTML -->
<img alt="{{_name}}">
<h1>{{_name}}</h1>
```

---

##### `_avatar` — Avatar do personagem

Leitura síncrona. Retorna a URL da imagem de perfil. **Use sempre `data-sf-src`** (não `src` direto) — a extensão busca a imagem via seu service worker para contornar as restrições de segurança do Roll20 (CORS/CSP).

```html
<img data-sf-src="_avatar" src="{{_avatar}}" alt="{{_name}}">
```

> Usar `src="{{_avatar}}"` sozinho funciona na primeira renderização mas pode falhar dependendo da política de segurança do navegador. Com `data-sf-src`, a imagem sempre carrega.

---

##### `_bio` — Biografia do personagem

**Leitura assíncrona.** O Roll20 armazena a bio como um blob separado — não é um atributo simples como `_name`. A extensão lê o conteúdo diretamente do editor Summernote dentro do iframe da ficha, o que leva um momento após a ficha abrir.

Na prática: a bio aparece **logo após** o resto da ficha, sem atraso perceptível na maioria dos casos. Se o iframe ainda estiver carregando, a extensão espera até 3 segundos.

A bio contém **HTML** (o Roll20 usa Summernote, que salva com tags `<p>`, `<strong>`, etc.). Use **`data-sf-html`** para exibi-la, não `data-sf`:

```html
<!-- Correto: renderiza o HTML da bio -->
<div data-sf-html="_bio"></div>

<!-- Errado: exibiria as tags HTML como texto literal -->
<div data-sf="_bio"></div>
```

> **Atenção:** `data-sf-html` insere HTML diretamente no DOM. Use apenas para `_bio` e outros campos cujo conteúdo você controla — nunca para campos editáveis por jogadores desconhecidos.

#### Interpolação com `{{tag}}`

Você pode usar `{{tag}}` em qualquer lugar do HTML para inserir um valor na primeira renderização:

```html
<img src="{{_avatar}}" alt="{{_name}}">
<div class="titulo">{{_name}} — Nível {{nivel}}</div>
```

---

### 5.4 Seções de texto livre (`freetext`)

Campos de texto livre são seções que **não mapeiam para atributos individuais do Roll20**. Todos ficam serializados como JSON em um único atributo oculto chamado `_sf_data`, gerenciado automaticamente pela extensão — o template nunca acessa esse atributo diretamente.

Declare as seções no `config.json`:

```json
"freetext": ["pericias", "inventario", "notas"]
```

Cada nome vira uma tag acessível no HTML. Há dois usos possíveis:

| Tipo | Armazenamento | Como usar no HTML |
|---|---|---|
| **Texto único** | String | `<textarea data-sf-input="notas">` |
| **Lista dinâmica** | Array JSON | `<div data-sf-list="pericias">` |

O tipo não é declarado no config — é determinado pelo que o template usa. Se o HTML usa `data-sf-list`, a extensão trata o valor como array. Se usa `data-sf-input`, trata como string. Ambos persistem em `_sf_data` e ficam visíveis na aba de atributos do Roll20 como `{"pericias":[...],"notas":"..."}`.

---

### 5.5 Listas dinâmicas (`data-sf-list`)

Uma lista dinâmica permite ao usuário **adicionar, editar e remover itens** durante a sessão. Cada item é um texto livre (pode ter múltiplas linhas).

#### Estrutura HTML de uma lista dinâmica

```html
<div data-sf-list="pericias">

  <!-- Template de um item: será clonado para cada entrada na lista -->
  <template>
    <div>
      <!-- Botão que expande/colapsa o item -->
      <button data-sf-item-toggle>
        <span data-sf-item-preview></span>  <!-- Preenchido automaticamente: 1ª linha do texto -->
      </button>

      <!-- Corpo do item (escondido por padrão) -->
      <div data-sf-item-body hidden>
        <textarea data-sf-item-input></textarea>   <!-- Campo de edição -->
        <button data-sf-item-delete>Remover</button>
      </div>
    </div>
  </template>

  <!-- Botão para adicionar um novo item vazio -->
  <button data-sf-list-add>+ Adicionar</button>

</div>
```

#### Como funciona

1. Quando a ficha carrega, a extensão lê o array salvo no Roll20 e clona o `<template>` uma vez para cada item.
2. O `[data-sf-item-preview]` é preenchido automaticamente com a **primeira linha** do texto do item (máximo 60 caracteres).
3. Clicar no botão `[data-sf-item-toggle]` mostra/esconde o `[data-sf-item-body]`.
4. Editar o `[data-sf-item-input]` e tirar o foco (ou pressionar `Ctrl+Enter`) **salva automaticamente** no Roll20.
5. Clicar em `[data-sf-item-delete]` remove o item e salva o array atualizado.
6. Clicar em `[data-sf-list-add]` adiciona um item vazio e salva.

#### Atributos disponíveis dentro do `<template>`

| Atributo | Elemento | Função |
|---|---|---|
| `data-sf-item-toggle` | `<button>` | Expande/colapsa o corpo do item |
| `data-sf-item-preview` | Qualquer | Exibido: 1ª linha não vazia (≤ 60 chars) |
| `data-sf-item` | Qualquer | Exibe o texto completo (somente leitura) |
| `data-sf-item-input` | `<textarea>` | Campo editável; auto-salva no blur |
| `data-sf-item-save` | `<button>` | Salva manualmente o item |
| `data-sf-item-delete` | `<button>` | Remove o item da lista |
| `data-sf-item-body` | Qualquer | Marcado como `hidden` por padrão; toggle abre/fecha |

> Todos os atributos dentro do `<template>` são **opcionais** — use apenas os que sua interface precisa.

#### Exemplo: lista de inventário em grade

```html
<div data-sf-list="inventario" style="display:grid; grid-template-columns: repeat(3, 1fr); gap:8px">
  <template>
    <div style="background:#111; border:1px solid #333; padding:8px; border-radius:4px">
      <span data-sf-item-preview style="font-weight:bold"></span>
      <button data-sf-item-toggle>✎</button>
      <button data-sf-item-delete>✕</button>
      <div data-sf-item-body hidden>
        <textarea data-sf-item-input style="width:100%"></textarea>
      </div>
    </div>
  </template>
  <button data-sf-list-add style="grid-column: 1/-1">+ Adicionar Item</button>
</div>
```

---

### 5.6 Abas e painéis

O sistema de abas do SlothReSkin é **100% controlado por JavaScript**, scoped por overlay. Isso garante que múltiplas fichas abertas ao mesmo tempo nunca interfiram entre si.

> **⚠ Não use radio buttons CSS** (`<input type="radio" name="...">`) para abas. Eles usam `name` e `id` globais — com dois overlays na mesma página, clicar em uma aba de uma ficha deseleciona a aba da outra. Use sempre `data-sf-tab` / `data-sf-tabpanel`.

#### Exemplo básico

```html
<!-- 1. Regra CSS obrigatória: painéis ficam ocultos por padrão -->
<style>
  [data-sf-tabpanel] { display: none; }

  /* Estilo da aba ativa (classe adicionada automaticamente) */
  [data-sf-tab].sf-tab-active {
    background: #ff6600;
    color: #000;
  }
</style>

<!-- 2. Botões de aba — qualquer elemento com data-sf-tab -->
<div class="minhas-abas">
  <button data-sf-tab="stats">Estatísticas</button>
  <button data-sf-tab="inv">Inventário</button>
  <button data-sf-tab="notas">Anotações</button>
</div>

<!-- 3. Painéis — o nome deve bater exatamente com o data-sf-tab -->
<div data-sf-tabpanel="stats">
  <!-- conteúdo de estatísticas -->
</div>

<div data-sf-tabpanel="inv">
  <!-- conteúdo de inventário -->
</div>

<div data-sf-tabpanel="notas">
  <!-- conteúdo de anotações -->
</div>
```

**Como funciona:**
- Ao injetar o overlay, a extensão encontra todos os `[data-sf-tab]` e ativa o **primeiro** automaticamente.
- Clicar em um botão de aba: adiciona `sf-tab-active` nele, remove dos demais, e alterna `display` dos painéis.
- Os painéis inativos ficam com `style="display:none"` e o ativo com `style="display:block"` — isso sobrescreve a regra CSS de default.

---

#### Abas aninhadas (sub-abas)

Se você precisar de **dois níveis de aba** (ex: abas principais + sub-abas dentro de um painel), use o atributo `data-sf-tabgroup` para criar grupos independentes. Tabs e panels sem `data-sf-tabgroup` pertencem ao grupo padrão.

```html
<style>
  [data-sf-tabpanel] { display: none; }

  /* Aba principal ativa */
  [data-sf-tab].sf-tab-active { background: #ff6600; color: #000; }

  /* Sub-aba ativa */
  .sub-nav [data-sf-tab].sf-tab-active { background: #fff; color: #000; }
</style>

<!-- Abas principais (grupo padrão — sem data-sf-tabgroup) -->
<div>
  <button data-sf-tab="stats">Estatísticas</button>
  <button data-sf-tab="inv">Inventário</button>
</div>

<div data-sf-tabpanel="stats">

  <!-- Sub-abas dentro do painel de Estatísticas (grupo "sub") -->
  <div class="sub-nav">
    <button data-sf-tab="pericias"    data-sf-tabgroup="sub">Perícias</button>
    <button data-sf-tab="habilidades" data-sf-tabgroup="sub">Habilidades</button>
  </div>

  <div data-sf-tabpanel="pericias"    data-sf-tabgroup="sub">
    <!-- conteúdo de perícias -->
  </div>
  <div data-sf-tabpanel="habilidades" data-sf-tabgroup="sub">
    <!-- conteúdo de habilidades -->
  </div>

</div>

<div data-sf-tabpanel="inv">
  <!-- conteúdo de inventário -->
</div>
```

**Regra:** `data-sf-tab` e `data-sf-tabpanel` com o mesmo `data-sf-tabgroup` formam um grupo isolado. Clicar em uma aba do grupo `"sub"` nunca afeta as abas do grupo padrão, e vice-versa. Você pode ter quantos grupos quiser — use nomes descritivos (`"sub"`, `"detalhes"`, `"combate"`, etc.).

---

#### Estilizando com CSS

A extensão só adiciona/remove a classe `sf-tab-active` e altera `style.display` dos painéis. O visual é totalmente seu:

```css
/* Botão de aba base */
[data-sf-tab] {
  padding: 8px 16px;
  background: #1a1a1a;
  border: none;
  color: #888;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.15s, color 0.15s;
}

/* Aba ativa */
[data-sf-tab].sf-tab-active {
  background: #ff6600;
  color: #000;
}

/* Hover */
[data-sf-tab]:hover { color: #eee; }
```

> **Dica:** Use `clip-path`, `border-radius`, `border-bottom`, ou qualquer efeito CSS para criar estilos de aba diferentes — a extensão não impõe nenhum visual.

---

### 5.7 Botões de habilidade (rolagens)

Para criar botões que rolam uma ability do Roll20 no chat, use o atributo `data-sf-ability`:

```html
<!-- Clicar neste elemento dispara a rolagem da ability "Ataque" -->
<button data-sf-ability="ataque">
  Ataque: <span data-sf="ataque">{{ataque}}</span>
</button>
```

> **Importante:** O valor em `data-sf-ability` deve corresponder à `tag` definida em `abilities[]` no `config.json`, que por sua vez aponta para o **nome exato** da ability no Roll20.

Quando clicado, o elemento recebe momentaneamente a classe `sf-rolling`, que você pode usar para animar o botão:

```css
@keyframes sf-roll-flash {
  0%   { box-shadow: 0 0 0 0   rgba(255, 102, 0, 0.7); }
  100% { box-shadow: 0 0 0 10px rgba(255, 102, 0, 0); }
}
.sf-rolling {
  animation: sf-roll-flash 0.45s ease-out;
}
```

---

### 5.8 Imagens e avatar

Para exibir o avatar do personagem, use `data-sf-src` em conjunto com `src="{{_avatar}}"`:

```html
<img data-sf-src="_avatar" src="{{_avatar}}" alt="{{_name}}">
```

O `src="{{_avatar}}"` define o valor no primeiro render. O `data-sf-src` faz a extensão reprocessar a imagem via seu service worker — necessário para contornar as restrições de CORS/CSP do Roll20. Use os dois juntos.

Para imagens de outros campos que armazenem URLs, o mesmo padrão se aplica:

```html
<img data-sf-src="foto_item" src="{{foto_item}}">
```

> Ver `_bio` na seção 5.3 para exibição da biografia (HTML assíncrono).

---

### 5.9 Edição inline de campos

A extensão oferece dois mecanismos de edição inline. Ambos são **contratos de comportamento** — a aparência visual (ícones, botões, layout) é inteiramente sua.

---

#### `data-sf-click-edit="tag"` — clique no display para editar

Colocar `data-sf-click-edit` em um elemento faz com que clicar nele o oculte e exiba um elemento irmão com a classe `.sf-click-input`. No blur ou Enter, o display volta. Escape cancela sem salvar.

O `.sf-click-input` deve também ter `data-sf-input="tag"` para que o valor seja salvo no Roll20.

```html
<span data-sf="pv" data-sf-click-edit="pv">{{pv}}</span>
<input type="number" class="sf-click-input" data-sf-input="pv"
       value="{{pv}}" style="display:none">
```

---

#### `data-sf-editable` — container com modo de edição ativado por gatilho

Marcar um container com `data-sf-editable` ativa um sistema de show/hide entre três elementos filhos reconhecidos por classe:

| Classe | Papel |
|---|---|
| `.sf-edit-btn` | Gatilho — clicar abre o modo de edição |
| `.val-display` | Exibido em modo leitura; oculto durante edição |
| `.val-input` | Exibido durante edição (`<input>`, `<textarea>`, ou `<div>` contendo inputs) |

Ao clicar em `.sf-edit-btn`: `.val-display` some, `.val-input` aparece e recebe foco. No focusout do `.val-input` (ou Enter): `.val-input` some, `.val-display` volta. Escape cancela.

```html
<div data-sf-editable>
  <button class="sf-edit-btn" type="button">Editar</button>
  <span class="val-display" data-sf="pv">{{pv}}</span>
  <input type="number" class="val-input" data-sf-input="pv"
         value="{{pv}}" style="display:none">
</div>
```

Para editar o valor **máximo** de um campo, use `data-sf-input-max` no input dentro de `.val-input`:

```html
<input type="number" class="val-input" data-sf-input-max="pv"
       value="{{pv_max}}" style="display:none">
```

> `.val-input` pode ser um `<div>` contendo múltiplos inputs (ex: current e max juntos). Nesse caso, o focusout fecha o modo de edição apenas quando o foco sai completamente do wrapper.

---

### 5.10 Barras de HP (`data-sf-hpbar`)

Crie barras de progresso visuais usando a propriedade CSS customizada gerada pela extensão:

```html
<!-- A extensão calculará --sf-pv-percent com base em current/max -->
<div data-sf-hpbar="pv"
     style="width:100%; height:8px; background:#333; border-radius:4px; overflow:hidden">
  <div style="height:100%; background:red;
              width: var(--sf-pv-percent, 100%);
              transition: width 0.5s ease;"></div>
</div>
```

> Para que funcione, o campo precisa ter **valor máximo** definido no Roll20 (coluna "max" na aba de atributos).

---

## 6. Referência completa dos atributos `data-sf-*`

| Atributo | Elemento | Descrição |
|---|---|---|
| `data-sf="tag"` | Qualquer | Exibe `current` do campo; atualiza em tempo real |
| `data-sf-max="tag"` | Qualquer | Exibe `max` do campo; atualiza em tempo real |
| `data-sf-html="tag"` | Qualquer | Insere HTML do campo (use apenas para conteúdo confiável) |
| `data-sf-src="tag"` | `<img>` | Define o `src` da imagem com o valor do campo |
| `data-sf-input="tag"` | `<input>`, `<textarea>` | Campo editável; salva `current` no Roll20 ao alterar |
| `data-sf-input-max="tag"` | `<input>` | Campo editável; salva `max` no Roll20 ao alterar |
| `data-sf-click-edit="tag"` | `<span>` ou similar | Clicar no elemento mostra um `.sf-click-input` irmão para editar `current` inline |
| `data-sf-editable` | Container | Modo de edição inline; reconhece `.sf-edit-btn` (gatilho), `.val-display` (leitura) e `.val-input` (edição) |
| `data-sf-tab="nome"` | `<button>` | Botão de aba; ativa o painel com mesmo nome no mesmo grupo |
| `data-sf-tabgroup="grupo"` | `<button>` ou Container | Isola abas em grupos independentes (ver seção 5.6) |
| `data-sf-tabpanel="nome"` | Container | Painel de aba; mostrado/escondido pelo sistema de abas |
| `data-sf-ability="tag"` | Qualquer clicável | Rola a ability do Roll20 ao clicar |
| `data-sf-hpbar="tag"` | Container | Calcula `current/max` e define variáveis CSS `--sf-{tag}-ratio`, `--sf-{tag}-percent`, `--sf-bar-ratio`, `--sf-bar-percent` |
| `data-sf-list="secao"` | Container | Lista dinâmica de itens para a seção de freetext |
| `data-sf-list-add` | `<button>` | Adiciona um item vazio à lista |
| `data-sf-item-toggle` | `<button>` dentro de `<template>` | Expande/colapsa o item; recebe `aria-expanded` |
| `data-sf-item-preview` | Qualquer dentro de `<template>` | Prévia: 1ª linha não-vazia do texto (≤ 60 chars) |
| `data-sf-item` | Qualquer dentro de `<template>` | Texto completo do item (somente leitura) |
| `data-sf-item-input` | `<textarea>` dentro de `<template>` | Campo de edição; auto-salva no blur ou `Ctrl+Enter` |
| `data-sf-item-save` | `<button>` dentro de `<template>` | Salva manualmente o item |
| `data-sf-item-delete` | `<button>` dentro de `<template>` | Remove o item da lista |
| `data-sf-item-body` | Container dentro de `<template>` | Corpo expandível do item; começa com `hidden` |

---

## 7. Referência do `config.json`

```jsonc
{
  // Nome do sistema (aparece no popup ao carregar)
  "system_name": "NomeDoSistema",

  // Campos que mapeiam atributos do Roll20 para tags usadas no HTML
  "fields": [
    {
      "roll20": "Nome exato no Roll20",   // Sensível a maiúsculas!
      "tag": "tag_no_html"                // Sem espaços, sem acentos
    }
  ],

  // Seções de texto livre (arrays dinâmicos ou blocos de texto)
  // Cada nome vira uma seção acessível como data-sf-list="nome" ou data-sf-input="nome"
  "freetext": ["pericias", "inventario", "notas"],

  // Abilities (macros) que podem ser disparadas via data-sf-ability
  "abilities": [
    {
      "roll20": "Nome exato da ability no Roll20",
      "tag": "tag_no_html"
    }
  ]
}
```

---

## 8. Como os dados são salvos (arquitetura)

Esta seção é para quem tem curiosidade técnica sobre como a extensão funciona internamente.

### Camadas da extensão

```
Chrome Extension
├── popup/           → Interface do popup (HTML + CSS + JS)
├── background/      → Service Worker (proxy de imagens)
└── content/         → Scripts injetados no Roll20
    ├── roll20_bridge.js   → Roda no contexto PRINCIPAL da página (acesso ao JS do Roll20)
    ├── mapper.js          → Funções de leitura/escrita de atributos
    ├── injector.js        → Injeta e mantém o overlay da ficha
    └── main.js            → Coordena tudo; observa dialogs de personagem
```

### Como a leitura funciona

1. Quando uma ficha de personagem é aberta no Roll20, o `main.js` detecta o elemento `.characterdialog`.
2. O `main.js` pede ao `roll20_bridge.js` (via `CustomEvent`) todos os atributos daquele personagem.
3. O `roll20_bridge.js` acessa os objetos Backbone.js do Roll20 (`Campaign.characters`) diretamente e devolve os dados.
4. O `injector.js` usa os dados para renderizar o template HTML e inseri-lo sobre a ficha original.

### Como a escrita funciona

1. O usuário edita um campo no overlay (digita num `data-sf-input`, edita um item de lista, etc.).
2. O `injector.js` dispara um evento `sf-write-request` com o nome do campo e o novo valor.
3. O `roll20_bridge.js` recebe o evento e chama `attr.save({ current: valor })` no objeto Backbone do Roll20.
4. O Roll20 salva automaticamente no servidor, exatamente como faria com a ficha padrão.

### Armazenamento de texto livre — pipeline completo (`_sf_data`)

Esta é a parte mais complexa da extensão. Entender o pipeline completo ajuda a depurar problemas e a estender o sistema.

#### Por que um único atributo?

O Roll20 não impõe limite prático de atributos por personagem, mas criar um atributo por item de lista (ex: `inventario_0`, `inventario_1`, ...) teria problemas sérios: IDs imprevisíveis, conflito com outros sistemas de ficha, e impossibilidade de saber quantos itens existem sem varrer todos os atributos. A solução adotada é armazenar tudo como um **JSON serializado** em um único atributo oculto:

```json
// Valor do atributo "_sf_data" no Roll20:
{
  "pericias":   ["Arco e Flecha 15/15", "Rastrear 10/15"],
  "inventario": ["Espada longa", "3x Poção de cura"],
  "notas":      "Encontramos o antigo templo de Zorath..."
}
```

Arrays e strings convivem no mesmo blob. O tipo (array ou string) é decidido pelo template, não pelo config.

#### Leitura na abertura da ficha

Quando `main.js` detecta um `.characterdialog` e chama `readAttributesForCharacter(characterId)`:

1. **`roll20_bridge.js` / `readAttribs()`** varre todos os `char.attribs` normalmente.
2. Ao final, chama `readFreetextData(char)` → faz `JSON.parse` do valor de `_sf_data` (fallback `{}`).
3. Para cada chave do JSON, adiciona uma entrada `_ft_<chave>` ao mapa de atributos:
   ```js
   // Array → serializado de volta para string JSON (o injector vai parseá-lo depois)
   attribs['_ft_pericias']   = { current: '["Arco 15/15","Rastrear 10/15"]', max: '' };
   // String → passa direto
   attribs['_ft_notas']      = { current: 'Encontramos o templo...', max: '' };
   ```
4. O atributo bruto `_sf_data` **não** é incluído no mapa retornado — o injector nunca o vê diretamente.

#### Renderização no injector

Em `injector.js / buildTagMap()`, cada seção declarada em `config.freetext` gera um mapeamento:
```js
map['pericias'] = '_ft_pericias';
map['notas']    = '_ft_notas';
```

Isso torna as seções de texto livre transparentes para o template: `data-sf-input="notas"` funciona exatamente como qualquer outro campo — o `tagMap` resolve `notas` → `_ft_notas` → valor atual.

Para listas dinâmicas, `bindLists()` varre todos os `[data-sf-list]`, chama `parseListItems(raw)` que faz `JSON.parse` do valor de `_ft_<secao>` e então `renderList()` clona o `<template>` interno uma vez por item.

#### Escrita de um item editado

Quando o usuário edita um item (blur no `[data-sf-item-input]` ou clica em salvar):

1. `sfListSave(container, index, novoValor, characterId, sectionKey)` clona `container._sfItems`, substitui o índice e chama:
   ```js
   writeAttributeForCharacter(characterId, '_ft_pericias', JSON.stringify(novoArray));
   ```
2. `mapper.js / writeAttributeForCharacter()` dispara `sf-write-request` para o MAIN world.
3. `roll20_bridge.js / writeAttrib()` detecta o prefixo `_ft_` e redireciona para `writeFreetext(char, 'pericias', novoJSON)`.
4. `writeFreetext()` lê o blob atual de `_sf_data`, substitui a chave `pericias` com o novo valor, e salva:
   ```js
   sfAttr.save({ current: JSON.stringify(data) });
   // ou, se _sf_data ainda não existe:
   char.attribs.create({ name: '_sf_data', current: JSON.stringify(data) });
   ```

#### Atualização em tempo real após salvar

O Backbone.js do Roll20 emite um evento `change` no `_sf_data` após o `attr.save`. O `listenToChar()` intercepta:

```js
if (name === '_sf_data') {
  // Explode o blob em eventos individuais por seção
  for (const [key, val] of Object.entries(data)) {
    window.dispatchEvent(new CustomEvent('sf-attrib-changed', {
      detail: { characterId, name: '_ft_' + key, current: String(val), max: '' }
    }));
  }
  return; // Não encaminha o _sf_data bruto
}
```

No lado isolado, `main.js / onAttribChanged()` recebe `name = '_ft_pericias'`, detecta o prefixo `_ft_` e chama `rerenderList()` para atualizar apenas a lista afetada — sem re-renderizar a ficha inteira.

#### Diagrama resumido

```
Usuário edita item
      │
      ▼
sfListSave() [injector.js]
      │  writeAttributeForCharacter('_ft_pericias', '[...]')
      ▼
sf-write-request (CustomEvent → MAIN world)
      │
      ▼
writeAttrib() [roll20_bridge.js]
      │  detecta _ft_ → writeFreetext()
      ▼
_sf_data.save({ current: '{"pericias":[...],...}' })
      │
      ▼  (Backbone 'change' event)
listenToChar() [roll20_bridge.js]
      │  explode → sf-attrib-changed { name: '_ft_pericias' }
      ▼
onAttribChanged() [main.js]
      │  detecta _ft_ → rerenderList()
      ▼
renderList() [injector.js]  ← lista visual atualizada
```

### Onde o template é armazenado

O template HTML e a configuração JSON ficam no **armazenamento local do Chrome** (`chrome.storage.local`), no seu navegador. Eles **não são enviados ao Roll20** nem para nenhum servidor externo.

---

## 9. Perguntas frequentes (FAQ)

**P: Outros jogadores na mesma mesa vão ver minha ficha personalizada?**

R: Não. A extensão funciona apenas no seu navegador. Cada jogador que quiser usar uma ficha personalizada precisa instalar a extensão e carregar o template por conta própria.

---

**P: Minha ficha personalizada vai sobrescrever os dados do personagem no Roll20?**

R: A extensão lê e **escreve** dados no Roll20. Qualquer campo que você editar na ficha do ReSkin (atributos, texto livre, inventário) é salvo de volta nos atributos do personagem no Roll20 — exatamente como se você tivesse editado na ficha padrão. Se desligar a extensão, os dados continuarão lá, visíveis na ficha original.

---

**P: Por que preciso recarregar a página depois de alterar o template?**

R: A extensão injeta o template quando a página do Roll20 termina de carregar. Se você troca o template depois, a versão antiga já foi injetada. Recarregar (F5) garante que o novo template e a nova configuração sejam usados do zero.

---

**P: Posso usar o mesmo template para sistemas diferentes?**

R: Sim! O template HTML define a aparência; o `config.json` define os campos. Você pode ter um HTML genérico e trocar apenas o `config.json` para adaptar a outro sistema (desde que os campos do Roll20 correspondam).

---

**P: As rolagens de dados funcionam?**

R: Sim. Ao clicar num elemento com `data-sf-ability`, a extensão localiza a ability correspondente no Roll20 e simula um clique no botão de rolagem original do Roll20, que então dispara a rolagem no chat normalmente.

---

**P: Posso usar CSS externo ou fontes do Google Fonts?**

R: Fontes externas como Google Fonts funcionam normalmente no `<style>` do seu HTML via `@import url(...)`. Para CSS separado, use o botão **"Carregar CSS"** no popup.

---

**P: A extensão funciona em outros sites além do Roll20?**

R: Não. Ela foi construída especificamente para `*.roll20.net` e depende de estruturas internas do Roll20 (objetos Backbone, atributos específicos, etc.).

---

**P: Como descubro o nome exato de um campo no Roll20?**

R: Abra a ficha de um personagem no Roll20, vá na aba **"Atributos e Habilidades"** e você verá a lista de atributos com seus nomes exatos. Copie o nome tal qual aparece na coluna "ATTRIBUTE NAME".

---

---

*SlothReSkin v0.1 — desenvolvido por SlothForge.*
