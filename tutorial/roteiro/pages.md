# Publicação

## GitHub Pages

Para publicar sua documentação no GitHub Pages, siga estas etapas:

1. Crie um repositório no GitHub e faça o push do seu código.
2. No repositório, vá para a aba "Settings".
3. Role para baixo até a seção "GitHub Pages".
4. Selecione a branch que contém sua documentação (geralmente `main` ou `gh-pages`).
5. Clique em "Save".

Após alguns minutos, sua documentação estará disponível em `https://<seu-usuario>.github.io/<seu-repositorio>/`.

## ReadTheDocs

Para publicar sua documentação no ReadTheDocs, siga estas etapas:

1. Crie um repositório no GitHub e faça o push do seu código.
2. Acesse [ReadTheDocs](https://readthedocs.org/) e crie uma conta.
3. Após fazer login, clique em "Import a Project".
4. Selecione o repositório que você criou no GitHub.
5. Siga as instruções para configurar o projeto e escolher a branch que contém sua documentação.

Após a configuração, sua documentação será gerada automaticamente e estará disponível em `https://<seu-projeto>.readthedocs.io/`.

## CircleCI para Previews

Para configurar o CircleCI para gerar previews da sua documentação, siga estas etapas:
1. Crie uma conta no [CircleCI](https://circleci.com/) e conecte-a ao seu repositório do GitHub.
2. Adicione um arquivo de configuração `.circleci/config.yml` ao seu repositório
com o seguinte conteúdo básico:

```yaml
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/python:3.9
    steps:
        - checkout
        - run:
            name: Install dependencies
            command: |
              python -m venv venv
              . venv/bin/activate
              pip install mkdocs mkdocs-material
        - run:
            name: Build documentation
            command: |
              . venv/bin/activate
              mkdocs build
workflows:
  version: 2
  build_and_deploy:
    jobs:
      - build
```

3. Faça commit e push do arquivo de configuração para o seu repositório.
4. No CircleCI, configure o projeto para que ele execute o job de build a cada
push ou pull request.
