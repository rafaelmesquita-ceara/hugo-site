---
title: "Criando sites estáticos com Hugo"
date: 2021-05-05T14:34:07-03:00
draft: true
featured_image: "./images/hugo.png"

slug: hugo

summary: "Hugo é uma estrutura de site de uso geral. Tecnicamente falando, Hugo é um gerador de sites estáticos. Ao contrário de sistemas que constroem dinamicamente uma página a cada solicitação de visitante, o Hugo constrói páginas quando você cria ou atualiza seu conteúdo." # Remove this if you want Hugo to just use the first 70 (configurable) characters of the post as the summary.
description: "Aprenda a criar sites estáticos poderosos e escaláveis com a incrível ferramenta Hugo"

# Lists
keywords:
tags:
categories:
---

# Hugo é um rápido e moderno gerador de site estático escrito em Go, e projetado para tornar a criação do site divertida novamente.

Hugo é uma estrutura de site de uso geral. Tecnicamente falando, Hugo é um gerador de local estático. Ao contrário de sistemas que constroem dinamicamente uma página a cada solicitação de visitante, o Hugo constrói páginas quando você cria ou atualiza seu conteúdo. Uma vez que os sites são vistos com muito mais frequência do que são editados, o Hugo foi projetado para fornecer uma experiência de visualização ideal para os usuários finais do seu site e uma experiência ideal de escrita para os autores do site.
Hugo é um rápido e moderno gerador de site estático escrito em Go, e projetado para tornar a criação do site divertida novamente.

# Iniciando

```cmd
hugo new site <nome-do-site>
cd <nome-do-site>
hugo server -D --gc
```

# Estrutura do Diretório

- **archetypes**
  - Arquivos de template pré-configurados. Não é recomendado mexer nisso
- **assets**
  - Guarda todos os arquivos processados pelo Hugo Pipes
- **config**
  - Diretório opcional para guardar arquivos de configuração, recomendado utilziar para sites grandes
- **content**
  - Onde todo o conteúdo é armazenado, posts e páginas. As pastas de nivel mais alto são  tratadas como *seção*
- **data**
  - Contém arquivos de configuração usadas pelo Hugo. Não é recomendado mexer nesse diretório
- **layouts**
  - Guarda páginas HTML de tema completas ou parciais para o site. Tudo que for sobrescrito correspondente ao seu tema, será customizado sem que haja necessidade de modificar os arquivos atuais do tema
- **static** 
  - Guarda todo o conteúdo estático, como imagens, CSS ou scripts. Tudo nessa página é copiada do jeito que está, sem nenhuma modificação ou interpretação por parte do Hugo. É aqui que você irá guardar sua mídia, como imagens para referenciar seus posts em seu blog
- **themes**
  - É o diretório onde você irá instalar todos os temas do Hugo
- **config.toml**
  - É o arquivo de configuração padrão, é possível usar diferentes diretórios se você quiser separar diferentes ambientes de desenvolvimento

# Adicionando Temas

Para adicionar temas utilizando o git, rode o seguinte comando (exemplo do tema **ananke**)

```bash
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

# Configuração do Tema

No arquivo config.toml é onde fica todas as configurações gerais relacionadas ao site, qual tema usar, quais parâmetros globais do tema serão aplicados, etc.

```
title = "Notre-Dame de Paris" 
baseURL = "https://example.com"
languageCode = "en-us"
theme = "ananke"

[params]
  Author = "Aaron Katz" # Add the name of the author (this theme only supports one author)
[author]
  name = "Caliburn Security" # Used by the foot copyright
```

# Modificando Arquivos de Temas

Os arquivos HTML que são utilizados pelo tema ficam em  .\themes\<nome-do-tema></nome-do-tema>\layouts\
Caso queira modificá-los, o ideal é trazer esses arquivos para a pasta .\layouts do seu site, o Hugo estabelecerá como padrão o layout do seu site, depois o layout do tema.

# Como Escrever Conteúdo

Vamos escrever uma página de Descrição do site

```
  hugo new about.md
```

Para essa página aparecer no menu principal, é necessário adicionar essa linha no front matter da página recém criada em .\content\about.mb 

```menu: main```

# Front Matter

São metadados que existem em sua página ou post criados pelo Hugo

Quando a página é criada, por padrão são preenchidos alguns metadados
- title 
- date
- draft
  
Existem alguns metadados úteis que podem ser adicionados manualmente
- description (Descrição do conteúdo)
- expiryDate (Data em que o conteúdo não poderá ser publicado)
- keywords (Palavras-chave para acessar o conteúdo)
- lastmod (Data na qual o conteúdo foi modificado por último)
- markup (Por padrão é md, mas pode ser definido como "rst" para usar o reStructuredText)
- publishDate (Data de publicação do conteúdo)
- slug (A URL de acesso ao conteúdo, por padrão é o nome do arquivo)
- summary (Um texto usado para prover um sumário para o artigo)
- <taxonomies> (Usado para o índice de taxonomy, como tags ou categorias)

# Como criar archetypes para os Posts do blog

O arquivo ./archetypes/default.md define qual o comportamento de metadados vai ter numa nova página automaticamente, pode ser definido tanto as chaves dos metadados quanto também os valores deles, como é feito no title, date e draft

Para criar um novo archetype, criaremos o arquivo posts.md na pasta archetypes com a seguinte codificação:

```
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true

slug: {{ .File.BaseFileName }} # Will take the filename as the slug. Feel free to change this to any format you like.  I like including this, so that I remind myself I have the option to change if I want.

summary: "" # Remove this if you want Hugo to just use the first 70 (configurable) characters of the post as the summary.
description: ""

# Lists
keywords:
tags:
categories:
---
```

Agora, todo post que criarmos com o comando 
```hugo new posts/<nome-do-post>.md``` 
Serão gerados os metadados definidos no archetype que criamos