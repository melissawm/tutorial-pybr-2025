# Sphinx

Vamos seguir o tutorial [minimalsphinx](https://minimalsphinx.readthedocs.io/en/latest/) para entender como o Sphinx funciona.

## rsT

O formato [reStructuredText](https://www.sphinx-doc.org/pt_BR/master/usage/restructuredtext/basics.html), ou reST, é uma sintaxe de marcação que permite gerar documentação automaticamente, incluindo marcação inline, conteúdo customizado e recursos poderosos para referências e links.

A marcação inline padrão do reST consiste em
- um asterisco: `*text*` para ênfase (itálico),
- dois asteriscos: `**text**` para forte ênfase (negrito), e
- duas crases: ` ``text`` ` para amostras de código.

Além disso, o reST também implementa [diretivas](https://www.sphinx-doc.org/pt_BR/master/usage/restructuredtext/basics.html#directives), que são blocos de marcação explícita que podem ter argumentos, opções e conteúdo. Veremos alguns exemplos disso em nossa documentação de código.

## Extensões

O Sphinx pode ser estendido com várias extensões para adicionar funcionalidades extras. Algumas extensões populares incluem:

- `sphinx.ext.doctest` para testar docstrings
- `sphinx.ext.intersphinx` criar referências para documentação de outros projetos
- `sphinx-design` para layouts avançados
- `sphinx-gallery` para criar galerias de exemplos
- `sphinx.ext.autodoc` para gerar documentação de APIs automaticamente a partir de docstrings

Veremos algumas dessas extensões em ação mais adiante.

## Inicialização de um projeto Sphinx

Vamos criar um projeto Sphinx do zero. Siga os passos abaixo:

0. Instale o [uv](https://docs.astral.sh/uv/getting-started/installation/) na sua máquina. Dependendo do seu sistema operacional, as instruções podem variar. O uv pode nos ajudar a gerenciar nossos ambientes virtuais e dependências, incluindo o próprio Python. Se você estiver criando um projeto do zero, pode executar

   ```bash
   uv init
   ```

1. Adicione as dependências básicas ao seu projeto. Por enquanto, vamos adicionar o Sphinx, o [tema furo](https://pradyunsg.me/furo/) e o [sphinx-design](https://sphinx-design.readthedocs.io/en/latest/) para algumas opções extras de layout.

   ```bash
   $ uv add --group docs sphinx furo sphinx-design
   ```

   Esse comando vai instalar o Python e todos os pacotes necessários em um ambiente virtual localizado em `.venv/`. Para ver a lista de dependências, observe a configuração do grupo de dependências "docs" no arquivo `pyproject.toml`. Para instalar essas dependências mais tarde, execute:

   ```bash
   $ uv sync --all-groups
   ```

2. Inicie seu projeto com o Sphinx

   Se você estiver começando um projeto do zero, pode fazer

   ```bash
   $ uv run sphinx-quickstart
   ```

   para obter uma estrutura de arquivos inicial e configuração para sua documentação. Você pode definir valores padrão para a maioria das respostas, mas deve preencher seu nome e o nome do projeto. A mensagem final deve dizer:

   > Finished: An initial directory structure has been created.

   > You should now populate your master file minimalsphinx/index.rst and create other documentation source files. Use the Makefile to build the docs, like so:
   >     make builder
   > where "builder" is one of the supported builders, e.g. html, latex or linkcheck

   Isso significa que, uma vez que você tenha sua documentação, pode escolher em qual formato gerar a sua saída. Por exemplo, para gerar uma versão html da documentação, você pode navegar até a pasta `docs/` e usar

   ```bash
   $ uv run make html
   ```

   No entanto, se você executar este comando agora, pode ver alguns WARNINGS - isso é esperado, pois ainda não criamos documentos para compilar.

   (Para o minimalsphinx, essa configuração inicial já foi executada.)

3. Você pode inspecionar a pasta atual para ver vários arquivos e pastas gerados automaticamente. Se você abrir o arquivo `_build/html/index.html`, pode ver um ponto de partida automaticamente gerado para sua documentação.

## O arquivo `index.rst` e o formato reStructuredText

Por enquanto, você pode ver algumas diretivas no arquivo `index.rst`: `toctree` é uma diretiva reStructuredText; `maxdepth` e `caption` são opções para esta diretiva, e seus valores são `2` e `Contents:`. Além disso, você pode ver o papel `:ref:` neste arquivo. O Sphinx implementa [_roles_ interpretados](https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html) para inserir marcação semântica nos documentos. Por exemplo, ``:ref:`search` `` implementa o role `ref` com o conteúdo `search` - neste caso, estamos fazendo referência cruzada a uma localização, neste caso criando um link para o documento com o rótulo `search`.

Veremos exemplos concretos disso na documentação do NumPy, mas você pode conferir um [resumo interessante da sintaxe do reST](https://sphinx-tutorial.readthedocs.io/step-1/) e um [primer mais longo sobre reST](https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html).

## O arquivo `conf.py`

Para gerar automaticamente nossa documentação, precisamos editar um arquivo de configuração chamado `conf.py`, que é criado pelo `sphinx-quickstart` com valores padrão na pasta raiz do seu projeto.

Por enquanto, a primeira coisa a personalizar é o nome do projeto e o autor, se necessário. Depois disso, podemos adicionar `'.venv'` à lista `exclude_patterns`, por exemplo. Com essa configuração básica, criamos a documentação com `uv run make html`.

## Criando documentação para um módulo Python

Vamos criar um módulo Python muito simples para testar nossa configuração. É um Pokédex inicial, contendo alguns Pokémon incluindo os três Pokémon iniciais da região de Kanto.

Quando se trata de documentar código Python, é comum escrever *docstrings*. O Sphinx suporta a inclusão de docstrings de seus módulos com uma extensão chamada [autodoc](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#module-sphinx.ext.autodoc).

Para usar o autodoc, você precisa ativá-lo em `conf.py` escrevendo

```
extensions = ['sphinx.ext.autodoc']
```

Em seguida, você pode documentar classes inteiras ou até módulos inteiros automaticamente. Por exemplo,

```
.. automodule:: io
   :members:
```

vai extrair todas as docstrings do módulo `io` e documentar todas as suas classes e funções.

Note que o autodoc precisa importar seus módulos para extrair as docstrings. Portanto, você deve adicionar o caminho apropriado ao `sys.path` em seu `conf.py`. No nosso caso, vamos adicionar o seguinte ao topo do arquivo `conf.py`:

```python
import os
import sys
sys.path.insert(0, os.path.abspath('../src'))
```

Em seguida, adicionamos o seguinte ao nosso arquivo `index.rst` para documentar nossa API automaticamente usando o autodoc:

```rst
.. automodule:: pokedex
   :members:
```

### doctests

Outra extensão útil é a [sphinx.ext.doctest](https://www.sphinx-doc.org/en/master/usage/extensions/doctest.html). Ela permite que você adicione testes *às suas docstrings*. Tudo o que você precisa fazer é adicionar `'sphinx.ext.doctest'` à sua variável `extensions` em `conf.py`. Depois disso, você pode adicionar doctests como exemplos em suas docstrings. Você pode então executar

```
$ uv run make doctest
```

para ver os resultados. No nosso pacote, observe o método `who_is_that_pokemon` na classe `StarterPokemon` para ver um exemplo.

## Adicionando mais páginas

Como queremos adicionar mais conteúdo à nossa documentação do que apenas docstrings, vamos criar páginas customizadas.

1. Mova a documentação da API para uma página separada.

   Primeiro, vamos criar um novo arquivo `apidocs.rst` e mover a diretiva `automodule` para este novo arquivo. Depois disso, adicionaremos `apidocs` à nossa ToC Tree - nossa Tabela de Conteúdos no arquivo principal `index.rst`.

2. Adicione uma página de Quickstart à nossa documentação

   Vamos escrever a página *Quickstart* separadamente.

### Intersphinx

Por fim, vamos verificar a extensão `sphinx.ext.intersphinx`. Adicionando-a à nossa lista de extensões, podemos nos referir facilmente aos links e rótulos da documentação de outros projetos usando seu *mapeamento intersphinx*. Para um exemplo, verifique a seção **Base Stats** no documento Quickstart deste pacote.

## MyST

- [Documentação oficial do MyST](https://myst-parser.readthedocs.io/en/latest/)

O MyST (Markedly Structured Text) é uma extensão do Sphinx que permite escrever documentos em Markdown, mantendo a capacidade de usar diretivas e roles do Sphinx. Isso é útil para quem prefere a sintaxe do Markdown, mas ainda quer aproveitar os recursos avançados do Sphinx.

### rst vs md: diferenças

```rst
Quickstart
==========

The Pokédex package contains basic information about the three `starter Pokémon <https://bulbapedia.bulbagarden.net/wiki/Starter_Pok%C3%A9mon>`_ from the `core series`_. These are detailed in :doc:`apidocs`.

.. _starter:

About starter Pokémon	
---------------------

.. _startpoke:

.. figure:: starter_pokemon.png

   Starter Pokémon from the core series.

As you can see in the :ref:`startpoke` image, these starter Pokémon have different types and abilities, and will evolve into different creatures as their level progresses.
```

````md
# Quickstart

The Pokédex package contains basic information about the three [starter Pokémon](https://bulbapedia.bulbagarden.net/wiki/Starter_Pok%C3%A9mon) from the [core series](#core-series). These are detailed in {doc}`apidocs`.

(starter)=
## About starter Pokémon

(startpoke)=

```{figure} starter_pokemon.png

   Starter Pokémon from the core series.
```

As you can see in the {ref}`startpoke` image, these starter Pokémon have different types and abilities, and will evolve into different creatures as their level progresses.
````

### MyST-NB

- [Documentação oficial do MyST-NB](https://myst-nb.readthedocs.io/en/latest/)

O MyST-NB é uma extensão do MyST que permite incluir notebooks Jupyter diretamente na documentação Sphinx. Isso é especialmente útil para projetos em que documentação executável e/ou interativa é importante.

## Como o Sphinx funciona?

O Sphinx implementa uma [máquina de estados](https://pt.wikipedia.org/wiki/M%C3%A1quina_de_estados_finita), usada para processar documentos. A máquina de estados do Sphinx é composta por várias fases, cada uma responsável por uma parte do processo de construção da documentação.

### Eventos

- [Fase 0: Inicialização] O diretório fonte é escaneado e as extensões são inicializadas. Um ambiente pode ser desserializado.
- [Fase 1: Leitura] Ler e parsear todos os documentos identificados como novos ou modificados
Diretivas e roles são encontradas e o código correspondente é executado.
Cada documento gera uma doctree, feita de nodes do docutils. Nodes temporários podem ser criados para arquivos que ainda não são totalmente conhecidos.
Dados de referências cruzadas são atualizados (labels, headings, index)
- [Fase 2: Verificações de consistência]
- [Fase 3: Resolução]
Agora que os metadados e as referências cruzadas de todos os documentos são conhecidos, todos os nodes temporários são substituidos por referências concretas.
- [Fase 4: Escrita]
Conversão de todas as doctrees na saída desejada (HTML, LaTeX).

```
1. event.config-inited(app,config)
2. event.builder-inited(app)
3. event.env-get-outdated(app, env, added, changed, removed)
4. event.env-before-read-docs(app, env, docnames)

for docname in docnames:
   5. event.env-purge-doc(app, env, docname)

   if doc changed and not removed:
      6. source-read(app, docname, source)
      7. run source parsers: text -> docutils.document
         - parsers can be added with the app.add_source_parser() API
      8. apply transforms based on priority: docutils.document -> docutils.document
         - event.doctree-read(app, doctree) is called in the middle of transforms, transforms come before/after this event depending on their priority.

9. event.env-merge-info(app, env, docnames, other)
   - if running in parallel mode, this event will be emitted for each process

10. event.env-updated(app, env)
11. event.env-get-updated(app, env)
12. event.env-check-consistency(app, env)

# The updated-docs list can be builder dependent, but generally includes all new/changed documents, plus any output from `env-get-updated`, and then all "parent" documents in the ToC tree. For builders that output a single page, they are first joined into a single doctree before post-transforms or the doctree-resolved event is emitted
for docname in updated-docs:
   13. apply post-transforms (by priority): docutils.document -> docutils.document
   14. event.doctree-resolved(app, doctree, docname)
       - In the event that any reference nodes fail to resolve, the following may emit:
       - event.missing-reference(env, node, contnode)
       - event.warn-missing-reference(domain, node)

15. Generate output files
16. event.build-finished(app, exception)
```

(Veja mais em https://www.sphinx-doc.org/en/master/extdev/event_callbacks.html)

### O que é uma extensão?

Uma extensão pode:
- Adicionar novos builders
- Registrar diretivas e roles novos (estender a linguagem reST)
- Adicionar código pra ser executado em eventos específicos

Uma extensão é um módulo Python com uma funcão `setup()`.

Exemplo: [sphinx-tags](https://github.com/melissawm/sphinx-tags)

```python
def setup(app):
   # Create config keys (with default values)
   # These values will be updated after config-inited
   app.add_config_value("tags_create_tags", False, "html")
   app.add_config_value("tags_output_dir", "_tags", "html")
   app.add_config_value("tags_overview_title", "Tags overview", "html")
   app.add_config_value("tags_extension", ["rst"], "html")
   app.add_config_value("tags_intro_text", "Tags:", "html")
   app.add_config_value("tags_page_title", "My tags", "html")
   app.add_config_value("tags_page_header", "With this tag", "html")
   app.add_config_value("tags_index_head", "Tags", "html")

   ... 

   # Update tags
   app.connect("builder-inited", update_tags)
   app.add_directive("tags", TagLinks)

   return {
       "version": __version__,
       "parallel_read_safe": True,
       "parallel_write_safe": True,
   }
```
