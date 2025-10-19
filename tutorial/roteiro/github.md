# Workflow básico com Git e GitHub

Para começar, como vamos usar uma abordagem de "docs as code", vamos revisar um fluxo de trabalho básico com Git e GitHub para documentar seu projeto.

1. Crie um repositório no GitHub para o seu projeto, se ainda não tiver um.
2. **Clone o repositório localmente**: Para modificar seu repositório localmente, crie um clone local usando:

   ```bash
   git clone <URL do repositório>
   ```
3. Vamos usar um fluxo de Pull Request para gerenciar mudanças na documentação, mas você pode também trabalhar diretamente num branch específico, se preferir. Crie um "feature branch" para suas mudanças:

   ```bash
   git checkout -b minha-documentacao
   ```
4. **Faça mudanças na documentação**: Adicione ou modifique arquivos de documentação no seu repositório local. Se ainda não tiver um sistema de documentação configurado, você pode começar com um simples `README.md`.
5. **Adicione e faça commit das mudanças**:
    ```bash
    git add .
    git commit -m "Adiciona nova seção de documentação"
    ```
6. **Envie suas mudanças para o GitHub**:
    ```bash
    git push origin minha-documentacao
    ```
    (lembre-se de verificar que o seu repositório remoto está configurado corretamente com `git remote -v`)
7. **Crie um Pull Request**: Vá até o GitHub e crie uma Pull Request do seu branch `minha-documentacao` para o branch principal (geralmente `main` ou `master`).
8. **Revise e incorpore a Pull Request**: Após a revisão, incorpore (merge) a Pull Request ao branch principal.
