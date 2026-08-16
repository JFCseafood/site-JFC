# Site JFC Seafood — como mexer

**Versão 16.08.2026-2** — o número aparece também no rodapé do site,
para saber sempre qual dos ficheiros está a ver.

Site estático, sem dependências externas. Basta copiar a pasta inteira para o alojamento.
Não precisa de servidor especial, base de dados nem ligação à internet para os estilos.

---

## ⚠️ ANTES DE ABRIR: descomprima o ZIP primeiro

**Não abra o `index.html` clicando nele dentro do ZIP.** O Windows abre o ficheiro
sozinho, numa pasta temporária, e deixa as pastas `css` e `imagens` para trás — o site
aparece sem estilos e sem imagens.

O que fazer:

1. Clique com o botão direito no ZIP → **"Extrair tudo…"**
2. Escolha uma pasta (por exemplo o Ambiente de Trabalho) e confirme
3. Abra a pasta extraída e só aí faça duplo clique no `index.html`

Se quiser apenas ver o site rapidamente sem descomprimir nada, use o ficheiro
**`JFC-Seafood-ficheiro-unico.html`** — tem tudo lá dentro e funciona a partir de
qualquer sítio. Mas para **publicar** o site, use sempre esta pasta: é mais leve
e carrega mais depressa.

---

## Certificado HACCP

O PDF `HACCP_JFC_SEAFOOD.pdf` está incluído, na mesma pasta do `index.html`.
O botão *Descarregar Certificado HACCP*, na secção "Compromisso", aponta para ele.

Para o substituir por uma versão mais recente, grave o novo PDF por cima, mantendo o
mesmo nome de ficheiro.

---

## Estrutura da pasta

```
index.html          → a página inteira (conteúdo + catálogo + código)
css/estilos.css     → todos os estilos, já compilados
fontes/             → tipos de letra (Inter e Playfair Display)
imagens/            → todas as fotografias e o logótipo
sitemap.xml         → mapa do site para o Google
robots.txt          → autoriza os motores de busca
```

**Importante:** manter esta estrutura. As imagens são chamadas por caminho relativo
(`imagens/polvo.jpg`), por isso a pasta `imagens` tem de ficar sempre ao lado do `index.html`.

---

## Trocar uma imagem

Basta substituir o ficheiro dentro de `imagens/` mantendo **exatamente o mesmo nome**.
Não é preciso mexer no código.

| Ficheiro | Categoria | Estado |
|---|---|---|
| `bacalhau.jpg` | Bacalhau | fotografia |
| `peixe.jpg` | Peixe | fotografia |
| `marisco.jpg` | Marisco | fotografia |
| `polvo.jpg` | Polvo & Cefalópodes (principal) | fotografia |
| `cefalopodes.jpg` | Polvo & Cefalópodes (segunda imagem no detalhe) | fotografia |
| `carnes.jpg` | Carnes | fotografia |
| `salgados.jpg` | Salgados & Pastelaria | fotografia |
| `legumes.jpg` | Legumes & Frutas | fotografia |
| `aves.jpg` | Aves | fotografia |
| `panados.jpg` | Panados | fotografia |
| `gelados.jpg` | Gelados & Sobremesas | fotografia |
| `logo-jfc.png` | logótipo (cabeçalho, favicon) | — |
| `logo-jfc-branco.png` | logótipo em branco (rodapé) | — |

**Formato recomendado:** quadrada, 1000×1000 px, JPEG a ~85% de qualidade
(as fotos atuais têm cerca de 100–180 KB cada).

---

## Alterar produtos

Todos os artigos estão numa única lista dentro do `index.html`.
Procure por `const catalogData` (perto do fim do ficheiro).

Cada categoria segue este molde:

```js
{
    id: "bacalhau",
    title: "Bacalhau",
    icon: "set_meal",
    image: "imagens/bacalhau.jpg",
    description: "Texto que aparece no cartão e no detalhe.",
    produtos: [
        { n: "Bacalhau Posta 250/350 Demolhado", ref: "JFC-BAC-003",
          f: "Caixa 8 kg", u: "Kg", c: "250/350", a: ["Demolhado","Posta"] },
    ]
}
```

| Campo | Significado | Obrigatório |
|---|---|---|
| `n` | nome do artigo | sim |
| `ref` | referência interna | sim |
| `f` | formato / embalagem (ex.: `Caixa 8 kg`) | sim |
| `u` | unidade de venda (`Kg`, `Caixa`, `Unidade`) | sim |
| `o` | origem (ex.: `Noruega`) | não |
| `c` | calibre (ex.: `250/350`) | não |
| `a` | lista de características (ex.: `["Demolhado"]`) | não |

Para **acrescentar** um artigo, copie uma linha existente e altere os valores.
Para **remover**, apague a linha inteira. Os contadores ("11 artigos", "161+")
atualizam-se sozinhos.

> Não há preços em lado nenhum do site — foi intencional.

---

## Globo dos pontos de origem

A secção "Logística" tem um globo terrestre interativo, desenhado em `<canvas>` sem
bibliotecas externas. Arrasta-se para rodar, clica-se num ponto para ver o país e o
produto, e as teclas de setas também funcionam. Roda sozinho e pára quando se lhe toca.

Para **alterar os países**, procure por `const ORIGENS` no `index.html`. Cada linha é:

```js
{ cod: "NO", nome: "Noruega", prod: "Bacalhau", lon: 9.0, lat: 61.0 },
```

`lon`/`lat` são as coordenadas no globo (longitude e latitude em graus). Se acrescentar
ou remover países, actualize também o número no selo **"23 Fontes"**, logo acima do
globo — esse valor está escrito à mão no HTML.

> Nota: o catálogo tem artigos da **Bélgica** (perninha de frango) mas a Bélgica não
> consta da lista dos 23 pontos de origem. Se quiser acrescentá-la:
> `{ cod: "BE", nome: "Bélgica", prod: "Frango", lon: 4.5, lat: 50.6 },`

O contorno dos continentes está no bloco `<script type="application/json" id="dados-terra">`
— não é preciso mexer-lhe.

---

## Pedido com vários artigos

Em cada artigo do catálogo há um botão **Adicionar ao pedido**. O cliente pode juntar
quantos quiser, indicar as quantidades e enviar tudo de uma vez.
Aparece um botão flutuante no canto com o número de artigos escolhidos.

Ao clicar em **Enviar pedido por e-mail** abre-se uma janela com quatro caminhos:

1. **Copiar texto do pedido** — funciona sempre, em qualquer computador ou telemóvel
2. **Gmail** — abre a janela de composição do Gmail já preenchida
3. **Outlook** — o mesmo, no Outlook web
4. **E-mail do PC** — o programa instalado (Outlook, Mail, Thunderbird)

> Porquê tantas opções: o botão dependia antes só do `mailto:`, que **não faz nada** em
> computadores sem programa de e-mail configurado — o caso de quem usa Gmail ou Outlook
> pelo browser. E o Windows corta endereços `mailto:` acima de ~2000 caracteres, pelo
> que pedidos grandes falhavam em silêncio. Agora, quando o pedido é longo, a janela
> avisa e aponta para o "Copiar texto".

O WhatsApp continua a funcionar num clique, tanto no botão principal como dentro da janela.

A lista vive só no browser do visitante e desaparece quando ele fecha a página — não
há base de dados nem nada guardado no servidor.

---

## Ligações diretas para categorias

Cada categoria tem endereço próprio. Pode enviar a um cliente:

```
https://www.jfcseafood.com/#cat-bacalhau
```

e a página abre já com essa categoria aberta. Os códigos são: `bacalhau`, `peixe`,
`marisco`, `cefalopodes`, `carnes`, `aves`, `panados`, `salgados`, `legumes`, `gelados`.

No telemóvel, o botão Voltar fecha a janela do produto em vez de sair do site.

---

## Imprimir o catálogo

O botão **Imprimir catálogo**, ao lado da pesquisa, gera uma listagem completa dos
161 artigos organizada por categoria, com referência, formato, unidade e origem —
sem preços. Dá 6 páginas A4. Na janela de impressão pode escolher "Guardar como PDF"
para enviar por e-mail.

---

## Google e motores de busca

O `index.html` traz os dados da empresa em formato estruturado (morada, telefone,
e-mail, redes sociais), que é o que o Google usa para mostrar a ficha da empresa nos
resultados. O `sitemap.xml` e o `robots.txt` completam o conjunto.

> Confirmado: o domínio **www.jfcseafood.com** já está online e é o da empresa, por isso
> não é preciso alterar nada. Se um dia mudar, troque-o em três sítios: no bloco
> `application/ld+json` do `index.html`, no `sitemap.xml` e no `robots.txt`.

---

## As duas gamas (premium e económica)

A mensagem de que a JFC trabalha em **duas gamas** aparece em cinco pontos do site:

| Onde | O quê |
|---|---|
| Herói | parágrafo de abertura + dois selos "Gama Premium" / "Gama Económica" |
| Sobre Nós | uma frase no texto de apresentação |
| Catálogo | dois cartões a explicar cada gama, antes da pesquisa |
| Setores | um ponto em cada cartão de setor |
| Contactos | convite a indicar qual das gamas procura |

Se quiser mudar a forma como isto é descrito, procure por **"Gama Premium"** e
**"Gama Económica"** no `index.html` — os textos estão todos escritos à mão, não são
gerados por código.

> Os artigos do catálogo **não** estão marcados individualmente como premium ou
> económicos, porque essa informação não vinha no ficheiro Word. Se um dia quiser essa
> distinção artigo a artigo, é preciso indicá-la no catálogo de origem.

---

## Mapa dos contactos

O mapa principal é o **Google Maps**, no formato de incorporação clássico (sem chave
de API). No canto do mapa há um botão discreto — **"O mapa não aparece?"** — que troca
para o OpenStreetMap. Serve de rede de segurança: o formato clássico do Google falha
nalgumas situações (por exemplo ao abrir o site como ficheiro local, sem alojamento,
ou com certos bloqueadores de conteúdo). Por baixo do mapa há ainda a morada escrita,
para nunca ficar um quadrado vazio.

Os botões **Abrir no Google Maps** e **Como chegar** abrem a app/site do Google num
separador novo e funcionam sempre, independentemente do mapa incorporado.

Para afinar a posição, procure `maps.google.com/maps?q=` no `index.html` (o pino do
Google) e `marker=` / `bbox=` na linha `data-osm` (o pino do mapa alternativo,
coordenadas `38.8918, -9.1892`).

---

## Alterar contactos

No `index.html`:

- **E-mail e WhatsApp do formulário** — procure por `EMAIL_COMERCIAL` e `TELEFONE_WA`,
  logo no início do bloco de código.
- **Morada, telefone e e-mail visíveis** — na secção "Fale Connosco".
- **Mapa** — o endereço está no `src` do `<iframe>` do Google Maps.

---

## O que ficou incluído

- Catálogo completo: **161 artigos em 10 categorias**, com formato, unidade de venda,
  calibre, origem e características.
- **Pesquisa** por nome, calibre, origem ou referência, em todo o catálogo de uma vez.
  Ignora acentos, por isso "camarao" encontra "Camarão".
- **Filtro** dentro de cada categoria.
- **Pedido de orçamento** em cada artigo: preenche o formulário de contacto
  automaticamente e envia por e-mail ou WhatsApp.
- Tudo alojado localmente — nenhum pedido a servidores externos além do mapa e das
  redes sociais.

---

## Publicar

Copie o conteúdo desta pasta para a raiz do alojamento (`public_html`, `www`, ou
equivalente). O `index.html` tem de ficar na raiz.

Para ver no computador antes de publicar, abra o `index.html` no browser — funciona,
mas os tipos de letra só carregam corretamente através de um servidor. Se quiser ver
exatamente como fica publicado, abra a linha de comandos dentro da pasta e execute:

```
python3 -m http.server 8000
```

depois abra `http://localhost:8000` no browser.
