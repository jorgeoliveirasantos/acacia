# Documentação da Biblioteca Acacia

A biblioteca **Acacia** é um conjunto de componentes web customizáveis e utilitários para construção rápida de interfaces de aplicações web. Ela fornece elementos HTML personalizados, sistema de temas via variáveis CSS, gerenciamento de layout responsivo, modais, tooltips, grids de dados, galerias, players de áudio e muito mais.

## Índice

1. [Instalação](#instalação)
2. [Primeiros passos](#primeiros-passos)
3. [Estrutura da Aplicação](#estrutura-da-aplicação)
4. [Temas e Personalização](#temas-e-personalização)
5. [Componentes](#componentes)
   - [Elementos de layout](#elementos-de-layout)
   - [Elementos de conteúdo](#elementos-de-conteúdo)
   - [Formulários](#formulários)
   - [Navegação](#navegação)
   - [Mídia](#mídia)
   - [Feedback e interação](#feedback-e-interação)
   - [Listas e tabelas](#listas-e-tabelas)
   - [Menus laterais e modais](#menus-laterais-e-modais)
6. [Utilitários](#utilitários)
   - [Content](#content)
   - [Renderer](#renderer)
   - [Tooltip](#tooltip)
   - [Modal](#modal)
   - [Datagrid](#datagrid)
   - [Gallery](#gallery)
   - [IdControl](#idcontrol)
7. [Exemplos](#exemplos)
8. [Referência da API](#referência-da-api)

---

## Instalação

Para utilizar a Acacia em seu projeto, copie os arquivos fornecidos (`acacia.css`, `acacia.js`, `content.js`, `renderer.js`) para uma pasta, por exemplo `/lib/acacia/`. Inclua os arquivos no `<head>` da sua página:

```html
<link rel="stylesheet" href="/lib/acacia/acacia.css">
<script type="module" src="/lib/acacia/acacia.js"></script>
```

Alguns componentes dependem de assets (imagens, ícones, etc.) que devem estar disponíveis em `/files/`. Certifique-se de colocar os arquivos necessários (como `menu.svg`, `close.svg`, `splash.svg`, etc.) nesse diretório ou ajuste os caminhos no código.

---

## Primeiros passos

A maneira mais simples de iniciar uma aplicação Acacia é usando a classe `Acacia`:

```javascript
import { Acacia } from '/lib/acacia/acacia.js';

const app = new Acacia(
  'Meu App',               // Nome do aplicativo (título da página)
  false,                   // Oculta a barra inferior? (true/false)
  { ... }                  // Configuração do menu lateral (opcional)
);
```

Isso criará a estrutura básica com barra superior, área principal e barra inferior. O conteúdo inicial é carregado do arquivo `views/home` (por padrão). Você pode personalizar esse comportamento.

Para aplicações desktop (com menu superior no estilo tradicional), utilize `AcaciaDesktop`:

```javascript
import { AcaciaDesktop } from '/lib/acacia/acacia.js';

const app = new AcaciaDesktop(
  'Meu App Desktop',
  {
    'Arquivo': [
      { title: 'Novo', action: () => console.log('Novo') },
      { title: 'Abrir', action: () => console.log('Abrir') }
    ],
    'Editar': [
      { title: 'Copiar', action: () => console.log('Copiar') }
    ]
  }
);
```

---

## Estrutura da Aplicação

A Acacia define vários elementos customizados que estruturam a interface:

- `<acacia-app>`: Container principal da aplicação. Contém as barras superior e inferior e a área principal.
- `<app-top-bar>`: Barra superior, com logo, ícone de menu e espaço para ícones.
- `<app-bottom-bar>`: Barra inferior.
- `<app-main>`: Área principal dividida em três colunas: barra esquerda, visualização principal e barra direita.
- `<left-bar>`, `<right-bar>`: Barras laterais (visíveis apenas em modo paisagem).
- `<app-view>`: Área central onde o conteúdo principal é renderizado.

Esses elementos são automaticamente criados ao instanciar `Acacia` ou `AcaciaDesktop`. Você pode acessar as referências globais:

```javascript
window.ACACIA      // Elemento <acacia-app>
window.APP         // Elemento <app-main>
window.APPVIEW     // Elemento <app-view>
window.TOPBAR      // Barra superior
window.BOTTOMBAR   // Barra inferior
window.LEFTBAR     // Barra esquerda
window.RIGHTBAR    // Barra direita
```

---

## Temas e Personalização

A aparência é controlada por variáveis CSS definidas em `:root` no arquivo `acacia.css`. Você pode sobrescrevê-las para criar seu próprio tema.

```css
:root {
  --Black: #000;
  --White: #fff;
  --Dark: #101520;
  --DarkBlue: #202530;
  --DarkGreen: #105020;
  --LightYellow: #FD9;
  --LightGreen: #00DD88;
  --MediumBlue: #303540;
  --Crimson: #DD2080;
  --Gradiente45: linear-gradient(45deg, #DD2080, #00DD88)1;
  --SplashImage: url("/files/splash.svg");
  cursor: url("/lib/acacia/pointer.cur") 0 0, auto;
}
```

Altere qualquer variável para modificar cores, gradientes, cursor, etc. A fonte `chisel` é carregada via `@font-face` (certifique-se de ter o arquivo `Chisel Mark.ttf` no caminho indicado).

---

## Componentes

A Acacia oferece dezenas de componentes customizados. Abaixo a lista completa com breve descrição.

### Elementos de layout

| Elemento | Descrição |
|----------|-----------|
| `<acacia-container>` | Container centralizado com fundo padrão, usado para modais ou telas inteiras. |
| `<app-container>` | Container flexível em coluna, usado dentro das views. |
| `<grid-row>` | Linha flexível horizontal. |
| `<grid-column>` | Coluna flexível vertical. |
| `<container-box>` | Caixa com altura e largura ajustadas ao conteúdo. |
| `<vertical-divider>` | Divisor vertical (linha). |
| `<horizontal-divider>` | Divisor horizontal (linha). |

### Elementos de conteúdo

| Elemento | Descrição |
|----------|-----------|
| `<card-big>` | Cartão grande com borda, usado para posts, mensagens, etc. Possui classes auxiliares `.card-left` e `.card-right`. |
| `<text-heading>` | Título principal. |
| `<text-subheading>` | Subtítulo. |
| `<text-paragraph>` | Parágrafo justificado. |
| `<text-label>` | Rótulo (texto pequeno). |
| `<text-link>` | Link personalizado (usa atributo `href` e `target`). |
| `<toast-mini>` | Toast pequeno para notificações. Atributo `autoclose` define segundos para fechar automaticamente. |
| `<markdown-viewer>` | Visualizador de Markdown (usado em conjunto com `Renderer.Components.DocsPage`). |

### Formulários

| Elemento | Descrição |
|----------|-----------|
| `<acacia-form>` | Container de formulário com borda. |
| `<fieldset>` | Fieldset estilizado. |
| `<acacia-slider>` | Slider (range) customizado. Atributos: `min`, `max`, `step`, `value`, `id`. |
| `<acacia-progress>` | Barra de progresso com valor exibido. Atributo `value` (0-100). |
| `<input class="acacia-input">` | Campo de texto estilizado. |
| `<select class="acacia-input-select">` | Select estilizado. |
| `<label class="acacia-checkbox">` | Checkbox personalizado. |
| `<label class="acacia-radio">` | Radio button personalizado. |

### Navegação

| Elemento | Descrição |
|----------|-----------|
| `<acacia-tabs>` | Container de abas. Contém `<acacia-tabs-item>` e `<acacia-tabs-content>`. |
| `<acacia-badge>` | Seletor por badges (abas em linha). Contém `<acacia-badge-item>` e `<acacia-badge-content>`. |
| `<button-squared>` | Botão quadrado. |
| `<button-rounded>` | Botão arredondado. |

### Mídia

| Elemento | Descrição |
|----------|-----------|
| `<image-gallery>` | Galeria de imagens com navegação. Normalmente criada via classe `Gallery`. |
| `<acacia-audio>` | Player de áudio com controles (play/pause, volume, progresso). Atributo `src` aponta para o arquivo de áudio. |

### Feedback e interação

| Elemento | Descrição |
|----------|-----------|
| `<acacia-accordion>` | Accordion (painel expansível). Contém `<accordion-item>`, `<accordion-title>`, `<accordion-content>`. |
| `<compact-list>` | Lista compacta com ícone e título. Usa `<list-item>` e `<text-label>`. |
| `<acacia-table>` | Tabela simples. Usa `<table-title>`, `<table-headers>`, `<table-row>`, etc. |
| `<data-grid>` | Grid de dados editável com seleção de linhas. Normalmente criado via classe `Datagrid`. |
| `<side-menu>` | Menu lateral deslizante. Normalmente gerenciado por `Renderer.SideMenu`. |

### Menus laterais e modais

| Elemento | Descrição |
|----------|-----------|
| `<side-menu>` | Container do menu lateral. Contém `<side-menu-outside>` (fundo semitransparente) e `<side-menu-container>`. |
| `<modal-container>` | Container para modais. Usado internamente por `Modal`. |
| `<modal-box>` | Caixa de modal. |
| `<modal-window>` | Janela modal maior. |
| `<tooltip>` e `<context>` | Elementos flutuantes para tooltips e menus de contexto (gerenciados por `Tooltip`). |

---

## Utilitários

### Content

Objeto com métodos para manipulação de dados, cache, IndexedDB e HTTP.

| Método | Descrição |
|--------|-----------|
| `Content.LoadJSON(path)` | Carrega um arquivo JSON e retorna uma Promise com o objeto. |
| `Content.LoadText(path)` | Carrega um arquivo de texto. |
| `Content.LoadBinary(path)` | Carrega um arquivo binário (Blob). |
| `Content.ImageHandler.ResizeImg(imageElement, w, h)` | Redimensiona uma imagem e retorna nova imagem em base64. |
| `Content.TextHandler.Size(data)` | Retorna o tamanho dos dados em MB. |
| `Content.TextHandler.CurrencyConverter(txt, format)` | Converte string para formato monetário (R$). |
| `Content.TextHandler.MarkdownParser(markdown)` | Converte markdown para HTML. |
| `Content.IndexedDB.New(dbName, tableName)` | Cria banco/ tabela. |
| `Content.IndexedDB.Check(dbName, tableName, id)` | Verifica se registro existe. |
| `Content.IndexedDB.Write(dbName, tableName, data, id)` | Salva objeto. |
| `Content.IndexedDB.Read(dbName, tableName, id)` | Lê objeto. |
| `Content.IndexedDB.Remove(dbName, tableName, id)` | Remove registro. |
| `Content.Cache.SaveData(url, data, cacheName)` | Salva dados no Cache API. |
| `Content.Cache.GetData(url, cacheName)` | Recupera dados do cache. |
| `Content.HTTP.Get(path)` | Requisição GET simples. |
| `Content.HTTP.Post(path, body)` | Requisição POST com JSON. |
| `Content.Social` | Estruturas para dados de usuário e sala (UserData, RoomData). |

### Renderer

Responsável por carregar views, gerenciar layout e componentes complexos.

| Método | Descrição |
|--------|-----------|
| `Renderer.Load(viewName, target)` | Carrega um arquivo HTML da pasta `views/` e insere no elemento alvo. |
| `Renderer.Home(hideBottomBar, callback)` | Inicializa a tela inicial. |
| `Renderer.Desktop(callback)` | Inicializa o modo desktop (menu superior). |
| `Renderer.DropFiles()` | Exibe área de drop e retorna Promise com os arquivos soltos. |
| `Renderer.Layout` | Objeto com métodos de layout (`Verify`, `Landscape`, `Portrait`, `BottomTopLayout`, `TopLayout`, `CleanLayout`, etc.) |
| `Renderer.SideMenu` | Gerencia o menu lateral (`Show`, `StartDrag`, `EndDrag`, etc.). |
| `Renderer.VCard` | Importa/exporta vCard de perfil. |
| `Renderer.Emojis` | Listas de emojis e método `Insert(textBox, emoji)`. |
| `Renderer.Components` | Conjunto de funções para renderizar componentes prontos (perfil, posts, mensagens, comentários, salas, etc.). |

Exemplo de uso de `Renderer.Components`:

```javascript
Renderer.Components.MyProfilePage(
  id, username, name, description, thumbnail, genre, email, state,
  (userData) => { /* callback ao salvar */ }
);
```

### Tooltip

Gerencia tooltips, menus de contexto, popovers de perfil e toasts.

| Método | Descrição |
|--------|-----------|
| `Tooltip.Tooltip(texto, elemento)` | Exibe tooltip simples ao passar o mouse sobre o elemento. |
| `Tooltip.Context(menuItems)` | Exibe menu de contexto (array de objetos com `Ico`, `Title`, `Action`). |
| `Tooltip.ProfilePopover(user, elemento)` | Exibe popover com dados do perfil (thumbnail, nome, username). |
| `Tooltip.Toast(texto, segundos)` | Exibe toast automático (mini notificação). |

### Modal

Sistema de modais com diferentes tipos.

| Método | Descrição |
|--------|-----------|
| `Modal.Waiting(título, mensagem, callback)` | Modal de espera com animação de loading. Retorna Promise. |
| `Modal.Message(título, mensagem, callback)` | Modal de mensagem com botão "Confirmar". |
| `Modal.Error(título, mensagem, fatal, callback)` | Modal de erro, com opção fatal (reiniciar app). |
| `Modal.Confirm(título, mensagem, callback)` | Modal de confirmação (sempre chama callback). Retorna Promise com booleano. |
| `Modal.ConfirmAction(título, mensagem, callback)` | Similar, mas callback só é chamado se confirmado. |
| `Modal.Input(título, mensagem, callback)` | Modal com campo de texto. Retorna Promise com valor. |
| `Modal.Window(título, conteúdo)` | Modal com janela customizável (conteúdo HTML). |

### Datagrid

Classe para criar grids de dados editáveis com seleção de linhas.

```javascript
const dataGrid = new Datagrid(
  'meuGrid',                    // id do grid
  'Título',                     // título
  [['Nome', 'Idade'], ['João', 30], ['Maria', 25]], // dados
  container,                    // elemento alvo
  (selectedValues) => { ... },  // callback quando seleciona linhas
  (count) => { ... }            // callback quando seleciona todos
);
```

### Gallery

Classe para criar galerias de imagens.

```javascript
const gallery = new Gallery(
  'minhaGaleria',
  ['img1.jpg', 'img2.jpg', 'img3.jpg'],
  container
);
```

### IdControl

Gerador de IDs únicos para elementos.

```javascript
IdControl.NextId('Modal'); // ex: 'md-1'
```

---

## Exemplos

### Criando um aplicativo com menu lateral

```javascript
import { Acacia } from '/lib/acacia/acacia.js';

const sideMenuContent = {
  ShowProfileSection: true,
  ProfileSection: {
    ProfilePic: '/files/user.jpg',
    ProfileName: 'João Silva',
    ProfileUsername: '@joao',
    Action: () => { console.log('Perfil'); }
  },
  Links: [
    { LinkIcon: '/files/home.svg', LinkTile: 'Início', LinkAction: () => console.log('Home') },
    { LinkIcon: '/files/config.svg', LinkTile: 'Configurações', LinkAction: () => console.log('Config') }
  ]
};

new Acacia('Meu App', false, sideMenuContent);
```

### Exibindo um modal de confirmação

```javascript
import { Modal } from '/lib/acacia/acacia.js';

async function excluir() {
  const confirmado = await Modal.Confirm('Atenção', 'Deseja realmente excluir?');
  if (confirmado) {
    // executa ação
  }
}
```

### Carregando uma view personalizada

```javascript
Renderer.Load('minhaView', APPVIEW).then(() => {
  console.log('View carregada');
});
```

### Usando o player de áudio

```html
<acacia-audio src="/musicas/minha-musica.mp3"></acacia-audio>
```

---

## Referência da API

Para consulta detalhada, explore os arquivos fonte. Abaixo um resumo das principais classes e objetos expostos globalmente:

- `window.Content` – utilitários de dados.
- `window.Renderer` – renderização e layout.
- `window.Modal` – sistema de modais.
- `window.Tooltip` – tooltips e context menus.
- `window.Datagrid` – classe para grids.
- `window.Gallery` – classe para galerias.
- `window.IdControl` – gerador de IDs.

Elementos customizados registrados:

- `acacia-app`, `acacia-accordion`, `card-big`, `button-rounded`, `button-squared`, `text-link`, `toast-mini`, `app-drawer-group`, `group-title`, `acacia-slider`, `acacia-progress`, `image-gallery`, `acacia-audio`, `acacia-desktop-app`, `acacia-top-menu`, `menu-item`, `menu-item-dropdown`, `dropdown-item`.

---

Esta documentação cobre os principais aspectos da biblioteca Acacia. Para dúvidas ou contribuições, consulte os arquivos fonte ou entre em contato com o mantenedor.