# Guia prático de Git e GitHub

Guia de consulta rápida para usar Git e GitHub no desenvolvimento de projetos.

> A ideia deste repositório não é substituir a documentação oficial, mas reunir os comandos e conceitos que mais aparecem no dia a dia.

---

## Sumário

- [Git x GitHub](#git-x-github)
- [Configuração inicial](#configuração-inicial)
- [Criando ou baixando um projeto](#criando-ou-baixando-um-projeto)
- [Fluxo básico do dia a dia](#fluxo-básico-do-dia-a-dia)
- [Entendendo os principais comandos](#entendendo-os-principais-comandos)
- [Branches](#branches)
- [Merge](#merge)
- [Como desfazer alterações](#como-desfazer-alterações)
- [Conflitos](#conflitos)
- [.gitignore](#gitignore)
- [Conventional Commits](#conventional-commits)
- [GitHub Pages](#github-pages)
- [Comandos úteis](#comandos-úteis)
- [Boas práticas](#boas-práticas)
- [Referências oficiais](#referências-oficiais)

---

## Git x GitHub

### Git

O **Git** é um sistema de controle de versão.

Ele registra o histórico das alterações feitas em um projeto e permite trabalhar com versões diferentes do código.

O Git funciona localmente no computador.

### GitHub

O **GitHub** é uma plataforma que hospeda repositórios Git na nuvem.

Ele permite:

- armazenar projetos remotamente;
- sincronizar código entre computadores;
- colaborar com outras pessoas;
- trabalhar com branches e Pull Requests;
- publicar páginas com GitHub Pages;
- acompanhar Issues, Actions e outras ferramentas.

Uma forma simples de lembrar:

```text
Git = controla as versões
GitHub = hospeda e compartilha o repositório
```

---

## Configuração inicial

Verifique se o Git está instalado:

```bash
git --version
```

Configure seu nome:

```bash
git config --global user.name "Seu Nome"
```

Configure seu e-mail:

```bash
git config --global user.email "seu-email@example.com"
```

Verifique as configurações:

```bash
git config --list
```

---

## Criando ou baixando um projeto

### Criar um repositório Git em uma pasta existente

Entre na pasta do projeto e execute:

```bash
git init
```

Isso cria o repositório Git local.

### Clonar um repositório do GitHub

```bash
git clone URL_DO_REPOSITORIO
```

Exemplo:

```bash
git clone https://github.com/usuario/projeto.git
```

O Git cria uma cópia local do repositório com o histórico de commits.

---

## Fluxo básico do dia a dia

O fluxo mais comum é:

```text
editar arquivos
    ↓
git status
    ↓
git add
    ↓
git commit
    ↓
git pull
    ↓
git push
```

Exemplo:

```bash
git status

git add .

git commit -m "feat: adiciona validação do formulário"

git pull

git push
```

### Antes de começar a trabalhar

Se o projeto também é alterado em outro computador ou por outras pessoas:

```bash
git pull
```

Assim você atualiza sua cópia local antes de começar.

---

## Entendendo os principais comandos

### `git status`

Mostra o estado atual do repositório.

```bash
git status
```

Use com frequência. Ele informa:

- arquivos modificados;
- arquivos novos;
- arquivos preparados para commit;
- branch atual.

---

### `git add`

Adiciona alterações à área de preparação (*staging area*).

Adicionar um arquivo:

```bash
git add index.html
```

Adicionar vários arquivos:

```bash
git add .
```

O `git add` **não cria um commit**. Ele apenas seleciona o que fará parte do próximo commit.

---

### `git commit`

Registra uma versão das alterações preparadas.

```bash
git commit -m "mensagem do commit"
```

Exemplo:

```bash
git commit -m "feat: adiciona navegação entre etapas"
```

---

### `git push`

Envia os commits locais para o repositório remoto.

```bash
git push
```

---

### `git pull`

Busca alterações do repositório remoto e tenta integrá-las à branch atual.

```bash
git pull
```

É especialmente importante quando o mesmo projeto é utilizado em mais de um computador.

---

### `git log`

Exibe o histórico de commits.

```bash
git log
```

Versão resumida:

```bash
git log --oneline
```

---

### `git diff`

Mostra alterações ainda não adicionadas ao staging:

```bash
git diff
```

Para visualizar o que já foi adicionado com `git add`:

```bash
git diff --staged
```

---

## Branches

Branches permitem desenvolver alterações sem modificar imediatamente a branch principal.

A branch principal costuma se chamar:

```text
main
```

### Ver branches

```bash
git branch
```

### Criar uma nova branch

```bash
git branch nome-da-branch
```

### Criar e entrar na branch

```bash
git switch -c nome-da-branch
```

Exemplo:

```bash
git switch -c feat-validacao-formulario
```

### Trocar de branch

```bash
git switch main
```

### Excluir uma branch local

```bash
git branch -d nome-da-branch
```

---

## Merge

O merge integra alterações de uma branch em outra.

Exemplo: integrar uma feature à `main`.

Primeiro, vá para a branch que receberá as alterações:

```bash
git switch main
```

Atualize:

```bash
git pull
```

Depois:

```bash
git merge nome-da-branch
```

---

## Como desfazer alterações

> Antes de usar comandos para desfazer alterações, verifique o estado do projeto com `git status`.

### Descartar uma alteração ainda não adicionada

Para restaurar um arquivo para a versão do último commit:

```bash
git restore arquivo.txt
```

⚠️ A alteração local desse arquivo será descartada.

---

### Retirar um arquivo do staging

Se você executou `git add`, mas ainda não fez commit:

```bash
git restore --staged arquivo.txt
```

O arquivo continua modificado, mas sai da área de preparação.

---

### Corrigir a mensagem do último commit

Se o commit ainda não foi enviado ou você sabe o impacto da alteração:

```bash
git commit --amend -m "nova mensagem"
```

---

### Reverter um commit já compartilhado

Uma opção segura para desfazer um commit que já foi enviado é:

```bash
git revert HASH_DO_COMMIT
```

Isso cria **um novo commit** que desfaz as alterações do commit anterior.

---

## Conflitos

Um conflito pode ocorrer quando duas versões modificam a mesma região de um arquivo e o Git não consegue decidir automaticamente qual deve prevalecer.

O arquivo pode apresentar marcações semelhantes a:

```text
<<<<<<< HEAD
sua versão
=======
outra versão
>>>>>>> branch
```

Para resolver:

1. leia as duas versões;
2. escolha ou combine o conteúdo correto;
3. remova as marcações do conflito;
4. salve o arquivo;
5. adicione o arquivo novamente;
6. finalize o commit.

Exemplo:

```bash
git add arquivo.html
git commit -m "fix: resolve conflito no arquivo HTML"
```

Sempre confira:

```bash
git status
```

---

## .gitignore

O arquivo `.gitignore` informa ao Git quais arquivos ou pastas não devem ser versionados.

Exemplo:

```gitignore
node_modules/
.env
dist/
*.log
```

### Arquivos que normalmente não devem ir para o GitHub

- senhas;
- tokens;
- chaves de API;
- arquivos `.env` com credenciais;
- dependências que podem ser reinstaladas, como `node_modules/`;
- arquivos temporários;
- dados pessoais ou institucionais sensíveis.

> Nunca coloque credenciais em um repositório, mesmo que ele seja privado.

---

# Conventional Commits

Conventional Commits é uma convenção para escrever mensagens de commit de forma mais organizada.

Formato básico:

```text
tipo: descrição
```

Exemplo:

```text
feat: adiciona validação do formulário
```

A mensagem deve explicar **o que aquele commit fez**.

---

## Tipos mais comuns

### `feat`

Nova funcionalidade.

```text
feat: adiciona navegação entre etapas
```

```text
feat: implementa alternância entre planos
```

---

### `fix`

Correção de erro.

```text
fix: corrige validação do campo de e-mail
```

```text
fix: impede avanço com campos vazios
```

---

### `docs`

Alterações em documentação.

```text
docs: atualiza instruções de execução
```

```text
docs: adiciona informações ao README
```

---

### `style`

Alterações visuais ou de formatação que não mudam a lógica da aplicação.

```text
style: ajusta espaçamento do formulário
```

```text
style: melhora layout em dispositivos móveis
```

> Em Conventional Commits, `style` não significa necessariamente CSS. O termo originalmente também cobre formatação de código. Em projetos pessoais de front-end, é comum utilizá-lo para alterações puramente visuais, desde que o padrão seja mantido de forma consistente.

---

### `refactor`

Mudança na estrutura interna do código sem adicionar funcionalidade nem corrigir diretamente um bug.

```text
refactor: separa validações em funções
```

---

### `test`

Adição ou modificação de testes.

```text
test: adiciona testes para validação de e-mail
```

---

### `chore`

Tarefas de manutenção ou configuração.

```text
chore: adiciona arquivos iniciais do projeto
```

```text
chore: atualiza dependências
```

---

### `build`

Mudanças relacionadas ao processo de build ou dependências.

```text
build: adiciona configuração do Vite
```

---

### `ci`

Mudanças em integração contínua.

```text
ci: adiciona workflow de testes no GitHub Actions
```

---

### `perf`

Melhorias de desempenho.

```text
perf: reduz carregamento de imagens
```

---

## Exemplos rápidos

| Alteração | Commit sugerido |
|---|---|
| Criou uma funcionalidade | `feat: adiciona resumo do pedido` |
| Corrigiu um erro | `fix: corrige navegação para etapa anterior` |
| Alterou o README | `docs: atualiza README do projeto` |
| Ajustou o layout | `style: ajusta layout responsivo` |
| Reorganizou o JavaScript | `refactor: reorganiza lógica das etapas` |
| Criou testes | `test: adiciona testes do formulário` |
| Adicionou arquivos/configuração | `chore: adiciona configuração inicial` |

---

## Como escrever uma boa mensagem de commit

Prefira:

```text
feat: adiciona validação do telefone
```

em vez de:

```text
alterações
```

Evite mensagens vagas como:

```text
update
teste
mudanças
corrigi coisas
novo
final
final agora vai
```

Um commit deve representar uma alteração coerente.

Melhor:

```text
feat: adiciona seleção de plano
```

Depois:

```text
style: ajusta cartões dos planos
```

Do que colocar várias mudanças diferentes em um único commit chamado:

```text
update geral
```

---

## Descrição estendida do commit

Além do título, um commit pode ter uma descrição explicando melhor a alteração.

Exemplo:

```text
docs: substitui README pelo arquivo fornecido pelo Frontend Mentor

Substitui o README criado automaticamente pelo GitHub pelo arquivo
original disponibilizado no starter do desafio.
```

A descrição estendida é opcional.

---

## GitHub Pages

GitHub Pages permite publicar sites estáticos diretamente a partir de um repositório.

É útil para projetos feitos com:

- HTML;
- CSS;
- JavaScript;
- sites estáticos gerados por ferramentas compatíveis.

### Ativar Pages pela branch

No GitHub:

```text
Repository
→ Settings
→ Pages
→ Build and deployment
→ Source: Deploy from a branch
→ Branch: main
→ Folder: / (root)
→ Save
```

Se o projeto possui um `index.html` na raiz, ele normalmente será usado como página inicial.

Para um repositório chamado:

```text
meu-projeto
```

o endereço costuma seguir o formato:

```text
https://usuario.github.io/meu-projeto/
```

---

## Comandos úteis

### Mostrar repositórios remotos

```bash
git remote -v
```

### Ver a branch atual

```bash
git branch --show-current
```

### Ver histórico resumido

```bash
git log --oneline
```

### Ver histórico com representação das branches

```bash
git log --oneline --graph --decorate --all
```

### Ver diferenças

```bash
git diff
```

### Atualizar referências remotas sem integrar alterações

```bash
git fetch
```

---

## Boas práticas

- execute `git status` frequentemente;
- antes de começar a trabalhar, sincronize o projeto;
- faça commits pequenos e coerentes;
- escreva mensagens que expliquem a mudança;
- não envie senhas, tokens ou dados sensíveis;
- utilize `.gitignore` quando necessário;
- evite trabalhar muito tempo sem criar commits;
- antes de executar comandos destrutivos, confira o que eles fazem;
- em projetos colaborativos, prefira branches e Pull Requests;
- mantenha o README atualizado quando o projeto evoluir.

---

## Fluxo rápido para lembrar

### Projeto já existente no GitHub

```bash
git clone URL
cd projeto

# trabalhar...

git status
git add .
git commit -m "tipo: descreve a alteração"
git pull
git push
```

### Projeto já clonado em outro computador

```bash
git pull

# trabalhar...

git status
git add .
git commit -m "tipo: descreve a alteração"
git push
```

---

## Cola de commits

```text
feat:      nova funcionalidade
fix:       correção de bug
docs:      documentação
style:     estilo/formatação
refactor:  reorganização de código
test:      testes
chore:     manutenção/configuração
build:     build/dependências
ci:        integração contínua
perf:      desempenho
```

---

## Referências oficiais

- [Documentação do Git](https://git-scm.com/doc)
- [Documentação do GitHub](https://docs.github.com/)
- [Conventional Commits](https://www.conventionalcommits.org/)

---

## Observação

Este guia pode ser atualizado conforme novos comandos, situações e dúvidas surgirem durante o desenvolvimento de projetos.
