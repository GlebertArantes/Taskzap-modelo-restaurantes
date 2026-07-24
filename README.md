# Sabor da Casa — Modelo Demonstrativo TaskZap (Pacote Restaurantes)

> 🟡 **VERSÃO PROVISÓRIA.** Código, estrutura e documentação estão
> completos e testados. As **imagens ainda são provisórias** (composições
> abstratas, não fotografias) — ver `PENDENTE-IMAGENS-REALISTAS.md`. Os
> 4 arquivos em `prints/` são **mockups ilustrativos** (não capturas de
> navegador real — ver nota em `CHECKLIST-DE-TESTES.md`). Assim que as
> fotos reais forem fornecidas, a substituição é só trocar os arquivos
> em `img/` pelos mesmos nomes, e depois gerar os prints definitivos.
> **Este projeto ainda não foi publicado.**

## O que é este projeto

Site demonstrativo, completo e reutilizável, do **Pacote Restaurantes da
TaskZap**: cardápio digital com categorias, modal de pedido com tamanhos,
quantidade, entrega/retirada e observação, e envio direcionado ao WhatsApp.
Serve para (1) revisão final, (2) publicação no Netlify, (3) apresentação
comercial, (4) portfólio e (5) duplicação rápida para novos clientes —
trocando **apenas** o objeto `CONFIG` dentro do `index.html`.

> ⚠️ A marca **"Sabor da Casa"** é fictícia. Nome, telefone, endereço,
> cardápio, preços, avaliações e fotos **não pertencem a nenhum
> estabelecimento real**. Este projeto ainda **não foi publicado**.

---

## Estrutura de arquivos

```
TaskZap_Modelo_Demonstrativo_Restaurantes_v3_Final/
├── index.html                  ← site completo (HTML+CSS+JS, um arquivo só)
├── LEIA-ME.md                  ← este arquivo
├── CHECKLIST-DE-TESTES.md      ← checklist técnico completo
├── CAMPOS-DO-CONFIG.md         ← referência de todos os campos do CONFIG
├── PENDENTE-IMAGENS-REALISTAS.md ← especificação das fotos que faltam
├── prints/                     ← 4 mockups ilustrativos (ver nota no CHECKLIST)
└── img/
    ├── logo.png                ← logotipo (fundo transparente)
    ├── hero.webp                ├── marmita-tradicional.webp
    ├── sobre.webp                ├── marmita-fitness.webp
    ├── compartilhamento.webp     ├── marmita-vegetariana.webp
    ├── feijoada.webp             ├── frango-quiabo.webp
    ├── parmegiana.webp           ├── porcoes.webp
    ├── bebidas.webp              ├── sobremesas.webp
    └── LEIA-ME-IMAGENS.md      ← especificação de cada imagem
```

---

## Como abrir localmente

1. Extraia o ZIP mantendo a pasta `img/` **ao lado** do `index.html`
   (mesma pasta-pai — os caminhos são relativos, tipo `img/hero.webp`).
2. Dê duplo clique em `index.html` — abre em qualquer navegador, sem
   precisar de servidor, instalação ou internet (exceto pelas fontes do
   Google Fonts, que exigem conexão; sem internet o site ainda funciona,
   só troca a fonte pela padrão do sistema).

---

## Como alterar o CONFIG

Abra `index.html` em um editor de texto (VS Code, Bloco de Notas, etc.) e
procure por `const CONFIG = {`. Esse objeto único controla **tudo**: textos,
cores, cardápio, contatos, imagens, SEO e o que fica visível. Edite os
valores entre aspas ou colchetes e salve — não é necessário mexer em mais
nada no arquivo. Veja `CAMPOS-DO-CONFIG.md` para a lista completa de campos.

### Como trocar a identidade (nome, slogan, cidade)
No bloco `identidade`:
```js
identidade:{ nome:"Nome do Cliente", slogan:"Frase de efeito.", emoji:"🍔", cidade:"Cidade/UF" }
```

### Como trocar o telefone (WhatsApp)
No bloco `contato`, campo `whatsapp` — **só números**, no formato
`55` + DDD + número (sem espaço, traço ou parênteses):
```js
contato:{ whatsapp:"5534999998888", ... }
```

### Como trocar o endereço
No bloco `local`:
```js
local:{ endereco:"Rua Exemplo, 123 — Bairro, Cidade/UF", enderecoCurto:"Rua Exemplo, 123", ... }
```

### Como trocar os horários
No bloco `operacao`, array `horarios`:
```js
horarios:[ { dia:"Seg a Sex", hora:"11h–15h · 18h–22h" }, { dia:"Sábado", hora:"11h–16h" }, ... ]
```

### Como editar o cardápio
No bloco `categorias` (bloco 9). Cada categoria tem `itens[]`; cada item
tem `nome`, `desc`, `foto`, `alt` e **ou** `preco` (produto único) **ou**
`tamanhos:[{rot,preco}, ...]` (produto com variações, ex.: P/M/G).

### Como trocar as imagens
Substitua os arquivos dentro de `img/` **mantendo os mesmos nomes** — o
CONFIG não precisa mudar. Detalhes completos em `img/LEIA-ME-IMAGENS.md`.

### Como configurar o mapa
No bloco `local`: cole o `src` do iframe do Google Maps em `mapaEmbedUrl`
para mostrar o mapa embutido, ou preencha `latitude`/`longitude` (o botão
"Ver localização" monta o link do Google Maps automaticamente). Se preferir
um link específico, use `mapaLink` (tem prioridade sobre lat/long).

### Como ativar e desativar o modo demonstração
No topo do `CONFIG`:
```js
modoDemonstracao: true   // demonstração: nada é enviado, número fica oculto
modoDemonstracao: false  // real: WhatsApp funciona normalmente
```

### Como testar o WhatsApp (antes de entregar ao cliente)
1. Coloque um número **real ou de teste** em `contato.whatsapp`.
2. Mude `modoDemonstracao` para `false` temporariamente.
3. Abra o site, escolha um produto, tamanho, quantidade, entrega/retirada
   e observação; confira a prévia da mensagem e clique em "Enviar pelo
   WhatsApp" — deve abrir o WhatsApp Web/app já com a mensagem pronta,
   **sem enviar nada sozinho**.
4. Depois do teste, **volte `modoDemonstracao` para `true`** (ou coloque o
   número definitivo do cliente) antes de publicar/entregar.

### Como publicar no Netlify
1. Acesse [app.netlify.com/drop](https://app.netlify.com/drop).
2. Arraste a **pasta inteira** (`index.html` + `img/`) para a página —
   não arraste só o `index.html`.
3. O Netlify gera uma URL (ex.: `https://nome-aleatorio.netlify.app`).
   Pode renomear em Site settings → Change site name.

### Como configurar o canonical
Depois de publicar, copie a URL final e cole em **dois lugares**:
- o elemento `link` com `id="canonical"` na seção `head` do HTML;
- `CONFIG.seo.canonical` (perto do fim do arquivo).
Publique de novo. Isso faz o `og:image` virar uma URL absoluta correta.

### Como configurar a imagem de compartilhamento
Já vem configurada (`img/compartilhamento.webp`, 1200×630). Para trocar,
substitua o arquivo (mesmo nome) ou aponte `CONFIG.imagens.compartilhamento`
para outro caminho. Depois de publicar, teste colando a URL do site no
verificador de compartilhamento do WhatsApp/Facebook.

### Como duplicar o modelo para um novo cliente
1. Copie a pasta inteira e renomeie (ex.: `Cliente_Hamburgueria_v1`).
2. Edite o `CONFIG` seguindo o checklist de 10 passos comentado dentro
   do próprio arquivo (identidade → contatos → horários → endereço →
   cardápio → fotos → mapa → mensagem → desligar demonstração → testar).
3. Troque as imagens em `img/` pelas fotos reais do cliente.
4. Rode a `CHECKLIST-DE-TESTES.md` antes de publicar.

### Como fazer backup
Mantenha uma cópia do ZIP original (este) intacta como "modelo-base" e
sempre duplique a pasta antes de editar para um cliente — nunca edite o
modelo-base diretamente. Guarde os ZIPs de cada cliente com o nome do
negócio e a data (ex.: `Hamburgueria-do-Ze_2026-08.zip`).

---

## Confirmações
- Nenhum dado de estabelecimento real foi utilizado (nome, telefone,
  endereço, cardápio, preços, avaliações ou fotos).
- Este projeto **não foi publicado** — é entregue apenas como arquivo local/ZIP.

Projeto: **TaskZap_Modelo_Demonstrativo_Restaurantes_v3_Final**
