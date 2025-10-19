# Motores de documentação

Quando o seu conjunto de documentação é grande ou você sente a necessidade de maior organização e estrutura, pode ser
útil usar um motor de documentação ao invés de trabalhar apenas com arquivos de texto.

Um motor de documentação (documentation engine) recebe arquivos de texto puro em um formato especial (por exemplo,
_reStructuredText_ ou _markdown_) e gera uma saída legível (HTML, PDF, ePub, etc). Ele também pode fornecer
funcionalidades adicionais, como navegação, busca, indexação, temas visuais, entre outros.)

Vamos discutir dois motores populares na comunidade Python: Sphinx e MkDocs.

## Sphinx

[Documentação oficial do Sphinx](https://www.sphinx-doc.org/en/master/)

Pontos fortes:
- Muito usado na comunidade Python (documentação do Python, NumPy, SciPy, Pandas, Matplotlib, [etc](https://www.sphinx-doc.org/en/master/examples.html))
- Flexível e extensível, com [muitas extensões disponíveis](https://www.sphinx-doc.org/en/master/usage/extensions/index.html)
- Suporte nativo para docstrings e geração automática de documentação de APIs com autodoc
- Suporte para múltiplos formatos de saída (HTML, PDF, ePub, etc)
- Poderoso e robusto: projeto maduro e estável

Pontos fracos:
- Curva de aprendizado mais íngreme
- Baseado em [docutils](https://docutils.sourceforge.io/) e reStructuredText, que podem ser menos familiares para novos usuários

Exemplo:
- [minimalsphinx](https://github.com/melissawm/minimalsphinx)

## MkDocs

[Documentação oficial do MkDocs](https://www.mkdocs.org/)

Moderno, bonito e fácil de usar, focado em documentação escrita em Markdown.

Pontos fortes:
- Fácil de configurar e usar, com uma curva de aprendizado suave
- Focado em Markdown, um formato amplamente usado e fácil de aprender
- Temas visuais modernos e atraentes, como o [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- Suporte para plugins para estender funcionalidades

Pontos fracos:
- Menos flexível que o Sphinx para projetos complexos
- Menos extensões disponíveis em comparação com o Sphinx

Exemplo:
- [minimalmkdocs](https://github.com/melissawm/minimalmkdocs)

## Bônus: MyST

- [Documentação oficial do MyST-md, também conhecido como MyST JS](https://mystmd.org/)
- [Documentação oficial do MyST Parser](https://myst-parser.readthedocs.io/en/latest/) (usado com Sphinx)

## Bônus: Jupyter Book

- [Documentação oficial do Jupyter Book](https://jupyterbook.org/)
- Esse tutorial foi escrito com o Jupyter Book! Veja o [repositório do tutorial](https://github.com/melissawm/tutorial-pybr-2025)

## Bônus: Quarto

- [Documentação oficial do Quarto](https://quarto.org/)
- Suporta múltiplas linguagens (R, Python, Julia, Observable)
- Focado em ciência de dados e publicação acadêmica, mas pode ser usado para documentação geral
- Inclui ferramentas para documentação executável

## Bônus: Docusaurus

- [Documentação oficial do Docusaurus](https://docusaurus.io/)
- Baseado em React, focado em documentação de projetos JavaScript/TypeScript, mas pode ser usado para qualquer projeto.

## Bônus: Marimo notebooks

- [Documentação oficial do Marimo](https://marimo.io)
