# Inumeráveis

> Não há quem goste de ser número. Gente merece existir em prosa.

Memorial dedicado à história de cada uma das vítimas do coronavírus no Brasil.

**Site:** https://inumeraveis.com.br

## Sobre

Inumeráveis é uma obra do artista **Edson Pavoni** em colaboração com Rogério Oliveira, Rogério Zé, Alana Rizzo, Guilherme Bullejos, Gabriela Veiga, Giovana Madalosso, Rayane Urani, Jonathan Querubina e os jornalistas e voluntários que continuamente adicionam histórias a este memorial.

Cada entrada no memorial é uma pessoa — um nome, uma idade, uma história — que existiu antes de virar estatística. O projeto nasceu em 2020 em resposta à forma como vítimas da pandemia eram reportadas no Brasil: pelo número, não pela vida.

## Estrutura do repositório

```
public/                    # Site estático servido pelo Firebase Hosting
  index.html               # Homepage — carrega names.json e renderiza a lista
  names.json               # Dados das ~9.200 histórias (gerado a partir do índice)
  <slug-do-nome>/          # Uma pasta por pessoa, com seu index.html
  wp-content/              # Assets herdados da migração WordPress (CSS/JS/fontes)
firebase.json              # Config do Firebase Hosting (cache headers, cleanUrls)
.firebaserc                # Projeto Firebase ativo (inumeraveis-ab320)
.github/workflows/         # Deploy automático via GitHub Actions
```

## Como o site é construído

O site é 100% estático. Cada memorial é um HTML pré-renderizado. A **homepage** carrega as ~9.200 histórias a partir de `names.json` e as renderiza no cliente — isso reduziu a página inicial de 2,5 MB para ~10 KB de HTML + um JSON cacheável por um ano.

## Deploy

O deploy acontece **automaticamente** via GitHub Actions a cada push em `main`:

1. O workflow em `.github/workflows/firebase-hosting-merge.yml` é acionado
2. Ele autentica no Firebase via um service account guardado como secret do repositório (`FIREBASE_SERVICE_ACCOUNT`)
3. Publica o conteúdo de `public/` no projeto `inumeraveis-ab320`

### Deploy manual (quando necessário)

```bash
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account.json
firebase deploy --only hosting --project inumeraveis-ab320
```

## Otimizações aplicadas

Para caber na cota gratuita do Firebase Hosting (10 GB de transferência/mês):

- **Homepage dinâmica**: 9.200 entradas extraídas para `names.json`, carregadas via `fetch` no cliente. HTML inicial: 34 KB (era 2,5 MB).
- **Cache agressivo**: assets estáticos (`css`, `js`, imagens, fontes) servidos com `Cache-Control: max-age=31536000, immutable`. Páginas HTML com `max-age=86400, stale-while-revalidate=604800`.
- **robots.txt**: bloqueia crawlers de IA e SEO de alto volume (GPTBot, ClaudeBot, CCBot, PerplexityBot, Bytespider, SemrushBot, AhrefsBot etc.). Demais bots recebem `Crawl-delay: 10`.
- **URLs relativas**: referências internas na homepage passam por caminhos relativos para reduzir bytes e viabilizar testes locais.

## Como adicionar uma história

Histórias são adicionadas através do formulário em [inumeraveis.com.br/adicionar](https://inumeraveis.com.br/adicionar). Após revisão editorial, a nova página é gerada e publicada.

## Jornalistas e voluntários

A curadoria e revisão das histórias é mantida por uma rede contínua de voluntários:

Ana Macarini · Phydia de Athayde · Ticiana Werneck · Lígia Franzin · Irion Martins · Flávia Campos · Priscilla Fernandes · Alessandra Capella Dias · Andressa Cunha · Vitória Freire · Julio Casimiro · Larissa Reis · Noêmia Maués · Bianca Ramos · Mariana Coelho · Denise Pereira · Danylo Martins · Eduardo Frumento · Beatriz Bevilaqua · Carolina Caires · Lucas Moreira · Jullia Cassia · Josué Seixas · Daniela Buono · Sandra Maia · Emmanuelle Alves Santos · Laura Capanema · Allanis Carolina · Mariana Carvalho · Lelê Pereira · Malu Marinho · Samara Lopes · Ana Laura Menegat de Azevedo · Gabriela Monteiro · Sonia Ferreira · Danielle Lorencini Gazoni Rangel · Marcelle Trote · Hélida Matta · Giovana Madalosso · Cristina Piasentini · Eleonora Marques · Aline Khouri · Agnes Vitoriano · Cintia Honorato de Santana · Claudia Saviolo · Gabriel Yudi Gati Isii · Andressa Vieira · Edson Lira · Fabiana Colturato Aidar · Amanda Queiros Gondim Bezerra · Luciana de Assunção · Silmara Magnabosco · Chico Bicudo · Cristina Marcondes · Letícia Sansão · Ana Clara Costa · Larissa Maria Sanches Cirino · Fabiana Vieira · Patrícia Scótolo · Ana Helena Alves Franco · Raul Galhardi · Bruna Borges · Danielle Zanoncini · Milena Gama · Jesiel Zerbo · Paula Cristina Sampaio Gato Cardim · Rafael Giraldo · Cairo Martins · Beatriz Brito · Lícia Zanol · Mayza Gabriele de Melo Rosa · Pablo Marcelo Rodrigues · Juliana Almeida de Souza · Gabriele Ramos Maciel · Roxanna Naddyne Ramaciotti Mires · Marina Marques · Cassio de Campos Neto · Francyne Nunes · Lucas Gonçalves Rangel · Rebeca Anzelotti · Rafaella Moura Teixeira · Rosana Forner · Ligia Carvalho · Acsa Tayane · Renata Meffe · Hortência Maia Silveira · Marina Mazzoni · Jéssica Avelar · Ana Beatriz Fonseca Braga · Bárbara Mendonça · Fernanda Rivelli · Júlia Paranhos da Vitória · Thais Oliveira · Dalva Chaves Pereira · Marina Machado Pereira Lins — e muitos outros.

## Licença

Todo o conteúdo deste repositório está sob uma licença **restrita a uso jornalístico**, com citação obrigatória da fonte. Veja [LICENSE.md](LICENSE.md) para os termos completos.

**Resumo:**

- ✅ Uso jornalístico, acadêmico-jornalístico e de memória pública — permitido, desde que com atribuição visível: *"Fonte: Memorial Inumeráveis — https://inumeraveis.com.br"*
- ❌ Uso comercial fora do escopo jornalístico — proibido
- ❌ Treinamento de modelos de IA com os dados deste memorial — proibido
- ❌ Redistribuição em massa do dataset (`names.json`) sem autorização — proibido

O **código técnico** (HTML/CSS/JS/infra) é livre para estudo e adaptação, desde que não replique as histórias, nomes ou imagens das vítimas.

---

*gente merece existir em prosa*
