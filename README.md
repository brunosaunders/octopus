# 🐙 Octopus - Multi-Repository Manager

Ferramenta CLI para gerenciar múltiplos repositórios React Native de forma eficiente e cross-platform.

## 🚀 Instalação e Uso

### ✅ Universal (Windows/macOS/Linux)
```bash
# 1. Clone e setup do Octopus
git clone https://github.com/drbf17/octopus.git
cd octopus
yarn install

# 2. Setup completo automatizado (OBRIGATÓRIO antes de start)
yarn oct init       # Clona repos + instala dependências
yarn oct install    # Se precisar reinstalar dependências

# 3. Verificar se tudo está instalado
yarn oct list       # Lista status dos repositórios

# 4. Uso diário
yarn oct start      # Inicia todos os servidores
yarn oct android    # Build Android + logs
yarn oct ios        # Build iOS + logs
```

> **💡 Simples:** Não precisa de `yarn link`, `PATH` ou privilégios administrativos!

## 📋 Comandos Disponíveis

| Comando | Descrição |
|---------|-----------|
| `yarn oct init` | **Setup completo**: clone + install + VS Code workspace |
| `yarn oct clone` | Clona repositórios em falta |
| `yarn oct install` | yarn install em paralelo (todos repos) |
| `yarn oct start` | Inicia todos os servidores em terminais separados |
| `yarn oct lint` | Lint em terminais separados por projeto |
| `yarn oct test` | Testes em terminais separados por projeto |
| `yarn oct android` | 🤖 Build Android + logs em terminais separados |
| `yarn oct ios` | 🍎 Build iOS + logs em terminais separados |
| `yarn oct update-sdk <version>` | 🔄 Atualiza SDK configurado em todos os módulos |
| `yarn oct checkout <branch>` | Checkout + pull em todos os repositórios |
| `yarn oct checkout <branch> --local -l` | Checkout local sem pull do remoto |
| `yarn oct new-branch <name> [base]` | Cria nova branch em todos os repos |
| `yarn oct delete-branch <name>` | Deleta branch local em todos os repos (com confirmação) |
| `yarn oct pull` | Pull das mudanças remotas em todos os repos na branch ativa |
| `yarn oct status` | Status Git de todos os repositórios |
| `yarn oct list` | Lista repositórios configurados |

## � Exemplos de Uso

### Setup inicial completo
```bash
yarn oct init
# ✅ Seleciona repositórios interativamente
# ✅ Clona automaticamente
# ✅ Instala dependências em paralelo  
# ✅ Cria workspace VS Code
```

### Desenvolvimento diário (IMPORTANTE: sempre instalar antes de iniciar!)
```bash
# 1. PRIMEIRO: Instalar/atualizar dependências
yarn oct install

# 2. DEPOIS: Iniciar todos os servidores
yarn oct start
# ✅ Executa todos em paralelo no terminal
```

### Comandos Git úteis
```bash
yarn oct checkout develop           # Checkout + pull em todos para develop
yarn oct checkout -l feature/login  # Checkout local sem pull do remoto
yarn oct pull                       # Pull da branch ativa em todos os repos
yarn oct delete-branch old-feature  # Deleta branch local (com confirmação)
# Use VS Code Tasks: Cmd+Shift+P → "Tasks: Run Task"
```

### Desenvolvimento nativo (Host app)
```bash
yarn oct android            # 🤖 Abre 2 terminais: Build + Logs Android
yarn oct ios               # 🍎 Abre 2 terminais: Build + Logs iOS
```

### Atualização de SDK
```bash
yarn oct update-sdk 0.3.0  # 🔄 Atualiza SDK em todos os módulos
# ✅ Atualiza package.json → yarn install → yarn fix-dependencies → yarn install
```

### Workflow de desenvolvimento
```bash
# Criar nova feature (faz pull da branch de referência antes de criar a nova branch)
yarn oct new-branch feature/login develop  # Cria branch em todos
yarn oct checkout feature/login            # Muda para a branch (com pull)

# Desenvolvimento local - branches já criadas anteriormente e deseja-se continuar o trabalho (se branch não existir, equivale ao git checkout -b)
yarn oct checkout -l feature/login       # Checkout sem pull (antes e depois) (mais rápido)
yarn oct pull                             # Pull quando necessário

# ... desenvolvimento ...
yarn oct lint                             # Lint por projeto
yarn oct test                             # Testes por projeto

# Limpeza após merge
yarn oct delete-branch feature/login      # Deleta branch local (com confirmação) - equivale ao git branch -d <branch>
```

### 🖥️ VS Code Integration
Após `oct init`:
- **Workspace**: `{projeto}-workspace.code-workspace`
- **Tasks**: `Cmd+Shift+P` → `Tasks: Run Task`
- **Keybindings**: `Cmd+Shift+R` → Start All

## 📁 Estrutura de Projeto
```
meu-projeto/
├── .vscode/tasks.json                    # Tasks automáticas
├── meu-projeto-workspace.code-workspace  # VS Code workspace  
├── octopus/                             # CLI tool
├── Host/                                # Repositório clonado
├── Auth/                                # Repositório clonado
└── Home/                                # Repositório clonado
```

## 🔧 Troubleshooting

### ❌ Erro: "Unable to find React Native files"
```bash
# Problema: Dependências não instaladas nos micro apps
# Solução:
yarn oct install        # Instala dependências em todos os repos
yarn oct list          # Verifica status dos repositórios

# Se persistir:
cd ../Auth && yarn install
cd ../Home && yarn install  
cd ../Contas && yarn install
cd ../Host && yarn install
```

### ⚠️ Erro: "yarn start exited with code 1"
```bash
# Geralmente indica dependências faltando ou corrompidas
yarn oct install       # Reinstala tudo
yarn oct start         # Tenta novamente

# Alternativa: modo separado para debug
yarn oct start --mode separate
```

### 🔍 Verificar se tudo está funcionando
```bash
yarn oct list          # Status de todos os repos
yarn oct status        # Status Git de todos os repos
```

## 🔧 Sobre o Projeto

### Princípios
- **Simplicidade**: Um comando (`yarn oct init`) configura tudo
- **Cross-platform**: funciona no macOS, Windows e Linux  
- **Universal**: usa `yarn oct` que funciona em qualquer terminal
- **Integrado**: gera workspace e tasks do VS Code
- **Reutilizável**: facilmente adaptável para outros projetos

### Configuração do SDK (`config/sdk-config.json`)
```json
{
  "sdkDependency": "@drbf17/react-native-webview",
  "updateCommands": {
    "yarn": ["yarn install", "yarn fix-dependencies", "yarn install"],
    "npm": ["npm install", "npm run fix-dependencies", "npm install"]
  }
}
```

### Objetivos
- Eliminar setup manual repetitivo de múltiplos repositórios
- Centralizar operações Git em todos os repos simultaneamente
- Otimizar workflow de desenvolvimento React Native/micro apps
- Fornecer experiência consistente entre diferentes plataformas

### Configuração (`config/default-repos.json`)
```json
{
  "repositories": [
    {
      "name": "Host",
      "url": "https://github.com/user/host.git",
      "localPath": "Host", 
      "port": 8081,
      "active": true,
      "isHost": true,
      "description": "App principal React Native"
    }
  ],
  "settings": {
    "defaultBranch": "main",
    "autoInstall": true,
    "createVSCodeTasks": true
  }
}
```

### Suporte a Monorepos

Para projetos que são monorepos e precisam de comandos específicos como `yarn workspace`, use o campo `prefix`:

```json
{
  "name": "Host",
  "url": "https://github.com/user/host-repo.git",
  "localPath": "../Host",
  "prefix": "host",
  "active": true,
  "port": 8081
}
```

Com isso, os comandos serão executados como:
- `yarn oct install` → `yarn host install` 
- `yarn oct start` → `yarn host start`
- `yarn oct android` → `yarn host android`

### Compatibilidade
- ✅ **Package Manager**: Auto-detecção yarn/npm
- ✅ **Terminais**: AppleScript (macOS), CMD (Windows), gnome-terminal (Linux)
- ✅ **Git**: Simple-git para operações cross-platform  
- ✅ **Node.js**: Execa para execução robusta de processos
- ✅ **Monorepos**: Campo `prefix` para comandos personalizados

---

**🐙 Octopus** - Simplifique o gerenciamento de múltiplos repositórios React Native