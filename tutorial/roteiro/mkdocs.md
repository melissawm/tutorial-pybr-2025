# MkDocs

Vamos seguir o tutorial [minimalmkdocs](https://minimalmkdocs.readthedocs.io/en/latest/) para entender como o MkDocs funciona.

## Instruções básicas

Note que essas instruções são muito parecidas com o que fizemos para o Sphinx!

0. Instale o [uv](https://docs.astral.sh/uv/getting-started/installation/) na sua máquina. Dependendo do seu sistema operacional, as instruções podem variar. O uv pode nos ajudar a gerenciar nossos ambientes virtuais e dependências, incluindo o próprio Python. Se você estiver criando um projeto do zero, pode executar

   ```bash
   uv init
   ```

1. Adicione as dependências básicas ao seu projeto. Eu recomendo adicionar o próprio mkdocs e qualquer tema que você deseje usar. Estamos usando [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) neste exemplo, e adicioná-lo é suficiente para começar (ele adicionará automaticamente o MkDocs, Markdown, Pygments e as extensões do Python Markdown que vamos usar à frente).

   ```bash
   $ uv add --group docs mkdocs-material
   ```

   Isso instalará o Python e todos os pacotes necessários em um ambiente virtual localizado em `.venv/`. Para ver a lista de dependências, observe a configuração do grupo de dependências "docs" no arquivo `pyproject.toml`. Para instalar essas dependências mais tarde, execute:

   ```bash
   $ uv sync --all-groups
   ```

2. Inicie seu projeto com o MkDocs

   Se você estiver começando um projeto do zero, pode fazer

   ```bash
   $ uv run mkdocs new .
   ```

   para obter uma estrutura de arquivos inicial e configuração para sua documentação. A mensagem final deve dizer:

   > INFO    -  Writing config file: ./mkdocs.yml

   > INFO    -  Writing initial docs: ./docs/index.md

   (Para o minimalmkdocs, o repositório já contém esses arquivos.)

3. Agora você pode verificar o arquivo `mkdocs.yml` para ver algumas opções de configuração padrão e a pasta `docs/` para os arquivos de documentação iniciais. Certifique-se de adicionar seu próprio nome de site, URL e definir o tema como Material. Seu arquivo `mkdocs.yml` deve conter o seguinte:

```yaml
site_name: Minimal MkDocs

site_url: https://github.com/melissawm/minimalmkdocs
theme:
  name: material
```

Para gerar o site de documentação, você pode executar

```bash
$ uv run mkdocs serve
```

Esse comando iniciará um servidor local, geralmente em `http://127.0.0.1:8000`. Você pode abrir este endereço em seu navegador para ver a documentação renderizada. Quaisquer alterações que você fizer nos arquivos Markdown serão refletidas automaticamente no navegador.

Agora vamos criar alguns documentos!

## Adicionando conteúdo narrativo

Você pode adicionar conteúdo narrativo à sua documentação criando arquivos Markdown na pasta `docs/`. Você pode então listar esses arquivos no arquivo de configuração `mkdocs.yml` na seção `nav:` para criar uma estrutura de navegação para seu site de documentação. Vamos adicionar uma página de Quickstart à nossa documentação dessa forma. Seu arquivo `mkdocs.yml` atualizado deve ficar assim:

```yaml
site_name: Minimal MkDocs

site_url: https://github.com/melissawm/minimalmkdocs
theme:
  name: material

nav:
  - Home: index.md
  - Quickstart: quickstart.md
```

## Extensões Markdown: admonitions e outros recursos extras do Markdown

Para alguns dos recursos extras que queremos adicionar à nossa documentação, usaremos plugins do MkDocs, como a [Python Markdown Admonition Extension](https://python-markdown.github.io/extensions/admonition/) e a [PyMdown Tabbed Extension](https://facelessuser.github.io/pymdown-extensions/extensions/tabbed/). Essas extensões já estão incluídas na instalação do Material for MkDocs, mas precisamos ativá-las em nosso arquivo `mkdocs.yml`. Para fazer isso, adicionamos as seguintes linhas à nossa configuração:

```yaml
markdown_extensions:
  - admonition
  - pymdownx.tabbed:
      alternate_style: true
  - pymdownx.superfences
```

(A [extensão SuperFences](https://squidfunk.github.io/mkdocs-material/setup/extensions/python-markdown-extensions/#superfences:~:text=The-,SuperFences,-extension%20allows%20for) permite a aninhamento arbitrário de blocos de código e conteúdo dentro uns dos outros, incluindo admonições, abas, listas e todos os outros elementos.)

Agora você pode adicionar admonitions aos seus arquivos Markdown usando a seguinte sintaxe:

```markdown
!!! warning

    This is a warning admonition.
```

E você pode criar conteúdo em abas usando a seguinte sintaxe:

```markdown
=== "Tab 1"
    Markdown **content**.

    Multiple paragraphs.

=== "Tab 2"
    More Markdown **content**.

    - list item a
    - list item b
```

## Plugins: incluindo outros arquivos Markdown

Você também pode incluir outros arquivos Markdown dentro de seus documentos usando o [mkdocs-include-markdown-plugin](https://github.com/mondeja/mkdocs-include-markdown-plugin).

Para usar este plugin, primeiro instale-o adicionando-o ao seu grupo de dependências `docs`:

```bash
$ uv add --group docs mkdocs-include-markdown-plugin
```

Em seguida, habilite-o em seu arquivo `mkdocs.yml` adicionando as seguintes linhas:

```yaml
plugins:
  - include-markdown
```

Agora você pode incluir outros arquivos Markdown em seus documentos usando a seguinte sintaxe:

```markdown
{% include-markdown "path/to/other/file.md" %}
```

## Documentando um módulo Python

Vamos usar o Pokédex novamente como exemplo de documentação de um módulo Python com o MkDocs.

Ao documentar código Python, é comum escrever *docstrings*. O MkDocs suporta a inclusão de docstrings de seus módulos com um plugin chamado [mkdocstrings](https://mkdocstrings.github.io/). Ele funciona para várias linguagens, mas só adicionaremos o suporte à linguagem Python. Primeiro, adicione-o ao seu grupo de dependências `docs`:

```bash
$ uv add --group docs mkdocstrings-python
```

Em seguida, habilite-o em seu arquivo `mkdocs.yml` adicionando as seguintes linhas:

```yaml
plugins:
  - mkdocstrings:
      default_handler: python
```

Agora você pode documentar classes inteiras ou até mesmo módulos automaticamente, usando opções de membro para as diretivas automáticas, como

```
# Reference

::: minha_biblioteca.meu_modulo.minha_classe
```

### Griffe para processar docstrings

O mkdocstrings usa o [Griffe](https://griffe.github.io/) para processar docstrings. Griffe suporta múltiplos estilos de docstring, incluindo Google, NumPy e reStructuredText. Você pode especificar o estilo de docstring em seu arquivo `mkdocs.yml` adicionando as seguintes linhas:

```yaml
plugins:
  - mkdocstrings:
      default_handler: python
      handlers:
        python:
          options:
              docstring_style: google
```

### doctests

Com o Sphinx, é possível executar testes incorporados na documentação usando a extensão `doctest`. O MkDocs não possui uma maneira integrada de executar doctests, mas você pode usar o [mktestdocs plugin](https://github.com/koaning/mktestdocs) para alcançar uma funcionalidade semelhante.

## Âncoras e referências internas e externas

O plugin [mkdocs-autorefs](https://mkdocstrings.github.io/autorefs/) pode ser usado para criar referências e âncoras internas e externas em sua documentação. Isso permite que você vincule facilmente a outras seções de sua documentação ou a recursos externos.

Para usar este plugin, adicione-o ao seu arquivo `mkdocs.yml`:

```yaml
plugins:
  - autorefs
```

Agora, você pode fazer o seguinte:
- Use `[title][anchor]` para criar um link para uma âncora interna.
- Use `[title](#section-id)` para vincular a uma seção dentro do mesmo documento.
- Use `[title](external-url)` para vincular a uma URL externa.
- Use `[object][]` para vincular a objetos documentados dentro do seu projeto.

Para vincular à documentação de outro projeto, você pode usar a opção `inventories` do plugin `mkdocstrings`. Por exemplo, para vincular à documentação da biblioteca padrão do Python, você pode adicionar as seguintes linhas ao seu arquivo `mkdocs.yml`:

```yaml
plugins:
  - mkdocstrings:
      handlers:
        python:
          inventories:
            - url: https://numpy.org/doc/stable/objects.inv
```

Agora, você pode vincular a objetos da biblioteca NumPy, por exemplo, usando a sintaxe `[object][]`. Por exemplo, para vincular à classe `array` no NumPy, você pode usar `[numpy.array][]` para obter [numpy.array][].
