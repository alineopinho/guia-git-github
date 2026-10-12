# Guia Git & GitHub — Duas contas no mesmo computador

> **Ambiente:** Windows + Git Bash + VS Code + Git Credential Manager  
> **Objetivo:** trabalhar com duas contas do GitHub, mantendo autoria, autenticação e repositórios separados.

| Conta | Usuário | Finalidade |
|---|---|---|
| Acadêmica | [`alineop120`](https://github.com/alineop120) | Faculdade e trabalhos acadêmicos |
| Profissional | [`alineopinho`](https://github.com/alineopinho) | Portfólio e projetos autorais |

## 1. Entenda as três configurações

| Conceito | O que controla |
|---|---|
| `user.name` e `user.email` | Autor registrado nos commits |
| Credenciais | Conta que autentica operações com o GitHub |
| `origin` | Repositório remoto conectado ao projeto |

Essas três configurações são **independentes**. Configurar o e-mail acadêmico não significa que a autenticação será feita pela conta acadêmica.

## 2. Configurando um computador do zero

### Passo A — Instalar as ferramentas

- [Git for Windows](https://git-scm.com/downloads/win)
- [Visual Studio Code](https://code.visualstudio.com/)

### Passo B — Configurar identidade padrão (portfólio)

```bash
git config --global user.name "Aline Oliveira de Pinho"
git config --global user.email "EMAIL_PROFISSIONAL"
```

Substitua o e-mail de exemplo pelo endereço associado à conta profissional ou seu endereço `noreply` do GitHub.

### Passo C — Separar credenciais por URL do repositório

```bash
git config --global credential.https://github.com.useHttpPath true
```

Isso permite que o gerenciador considere o caminho completo do repositório ao recuperar credenciais.

### Passo D — Verificar

```bash
git config --global --list
git config --get credential.helper
git config --get credential.https://github.com.useHttpPath
```

O `credential.helper` normalmente aparece como `manager` quando o Git Credential Manager está instalado e configurado. A última consulta deve retornar `true`.

## 3. Clonando um repositório existente

### Conta acadêmica — `alineop120`

```bash
git clone https://alineop120@github.com/Anabamorim/VetCare---Projeto-Final.git
cd VetCare---Projeto-Final

git config --local user.name "Aline Oliveira de Pinho"
git config --local user.email "EMAIL_ACADEMICO"
```

**Nota:** o repositório VetCare não pôde ser confirmado pelo acesso conectado ao GitHub na consulta; o endereço foi mantido conforme o exemplo fornecido. Caso seja privado, é necessário ter acesso autorizado.

### Conta profissional — `alineopinho`

```bash
git clone https://alineopinho@github.com/alineopinho/signal-lost.git
cd signal-lost
```

A conta profissional já está definida como identidade global. Só será necessário configurar a identidade local caso ela seja diferente.

O usuário antes de `@github.com` não é senha; ele indica a conta pretendida na autenticação. O Git Credential Manager pode abrir o navegador para login.

## 4. Criando um projeto completamente novo

### Passo A — Inicializar

```bash
mkdir meu-novo-projeto
cd meu-novo-projeto
git init
```

### Passo B — Criar um README

```bash
echo "# Meu Novo Projeto" > README.md
```

### Passo C — Fazer o primeiro commit

```bash
git status
git add README.md
git commit -m "docs: adiciona README inicial"
```

### Passo D — Criar um repositório remoto vazio

Entre em https://github.com/new pela conta profissional. Crie `meu-novo-projeto` sem inicializar um segundo README, se possível.

### Passo E — Conectar e publicar

```bash
git branch -M main
git remote add origin https://alineopinho@github.com/alineopinho/meu-novo-projeto.git
git push -u origin main
```

Para um novo projeto acadêmico, use a conta `alineop120` na URL e configure o `user.email` **local antes do primeiro commit**.

## 5. Trabalhando no dia a dia

Antes de começar:

```bash
git status
git branch --show-current
git pull
```

Após desenvolver e testar:

```bash
git status
git add .
git diff --cached
git commit -m "feat: implementa nova funcionalidade"
git push
```

O `git add .` inclui alterações não ignoradas no diretório atual e abaixo dele. Confira os arquivos e o diff preparado para evitar envio de informações indevidas. Em projetos colaborativos, confirme se você deve usar a `main` ou uma branch própria.

## 6. Problemas comuns

| Situação | O que verificar |
|---|---|
| `not a git repository` | Você está na pasta que contém `.git`? |
| `Repository not found` | URL, autenticação e permissão de acesso |
| Commit com e-mail errado | `git config --local user.email` |
| Não sabe para onde o push vai | `git remote -v` |
| Não sabe a branch | `git branch --show-current` |
| Precisa conferir alterações | `git status` |
| `Authentication failed` | Credenciais salvas e conta usada na autenticação |

### Se o Git estiver usando a conta errada

```bash
git remote -v
```

Se for o VetCare e você quiser indicar a conta acadêmica:

```bash
git remote set-url origin https://alineop120@github.com/Anabamorim/VetCare---Projeto-Final.git
```

Teste a leitura:

```bash
git ls-remote origin HEAD
```

Esse teste não comprova permissão de escrita. Caso permaneça o erro, investigue as credenciais do GitHub no **Gerenciador de Credenciais do Windows**, removendo somente a entrada problemática, se necessário. Uma nova autenticação será exigida.

## 7. Checklist de configuração

- [ ] Git e VS Code instalados
- [ ] Nome e e-mail globais configurados
- [ ] Git Credential Manager disponível
- [ ] `useHttpPath` habilitado
- [ ] Repositório clonado com a conta correta
- [ ] Nome e e-mail locais revisados
- [ ] `git status` sem erros

*Marque os itens somente após verificar o computador. O estado local não foi inspecionado.*

## 8. Comandos para consultar rapidamente

```bash
# Onde estou?
pwd

# Situação do repositório
git status

# Branch atual
git branch --show-current

# Repositório remoto
git remote -v

# Identidade efetiva no projeto
git config user.name
git config user.email

# E-mail global
git config --global user.email

# Gerenciador de credenciais
git config --get credential.helper

# De onde vieram as configurações
git config --show-origin --get-regexp 'user\.|credential\.'
```

## Referências oficiais

- [GitHub Docs — Gerenciamento de múltiplas contas](https://docs.github.com/pt/account-and-profile/how-tos/account-management/managing-multiple-accounts)
- [Git Credential Manager — Multiple Users](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/multiple-users.md)
- [Git Credential Manager — Configuration](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/configuration.md)

---

*Guia de consulta pessoal — revisão de outubro de 2026.*
