> ⚠️ **Documento histórico.** Plano do CLI npm e da estrutura `.onion/`, ambos **formalmente abandonados em 2026-05-18** (sem CLI standalone, sem produto npm). O Sistema Onion migrou para o **Google Antigravity** (`.agents/`) em 2026-06 — ver [ADR-001](../analysis/onion-antigravity-migration-adr-2026-06.md). Mantido apenas como registro histórico.

# Sistema Onion - Plano de Implementação CLI + Wizard

> **Versão**: 1.0.0 | **Última atualização**: 2025-12-20  
> Plano completo de implementação do CLI Onion inspirado na estratégia do OpenSpec

---

## 📋 Metadata

| Campo | Valor |
|-------|-------|
| **Versão** | 1.0.0 |
| **Data de Criação** | 2025-12-20 |
| **Última Atualização** | 2025-12-20 |
| **Status** | Pronto para Execução |
| **Prioridade** | 🔴 CRÍTICA |

---

## 🎯 Objetivo

Criar CLI interativo `@onion/cli` que implementa wizard de setup similar ao OpenSpec, gerando estrutura `.onion/` com suporte multi-IDE e multi-contexto.

---

## 🏗️ Arquitetura do CLI

### Estrutura do Projeto

```
packages/onion-cli/
├── package.json
├── README.md
├── bin/
│   └── onion.js                 # Entry point CLI
├── src/
│   ├── index.js                 # Main exports
│   ├── cli.js                   # CLI orchestrator
│   ├── commands/
│   │   ├── init.js              # onion init (wizard)
│   │   ├── add.js               # onion add context|ide
│   │   ├── migrate.js           # onion migrate
│   │   ├── validate.js          # onion validate
│   │   └── help.js              # onion help
│   ├── wizard/
│   │   ├── steps.js             # Wizard steps definitions
│   │   ├── prompts.js           # Inquirer prompts
│   │   └── detector.js          # IDE/tool detector
│   ├── generator/
│   │   ├── structure.js         # Generate .onion/ structure
│   │   ├── loaders.js           # Generate IDE loaders
│   │   ├── config.js            # Generate .onion-config.yml
│   │   └── templates.js         # Template renderer
│   ├── templates/               # File templates
│   │   ├── config.yml.hbs       # .onion-config.yml template
│   │   ├── context-config.yml.hbs
│   │   ├── loaders/
│   │   │   ├── claude.js.hbs    # Claude Code loader template
│   │   │   ├── windsurf.ts.hbs  # Windsurf loader template
│   │   │   ├── claude.py.hbs    # Claude adapter template
│   │   │   └── universal.md.hbs # Universal AGENTS.md
│   │   ├── agents/
│   │   │   ├── onion.md.hbs
│   │   │   └── context-router.md.hbs
│   │   ├── commands/
│   │   │   └── starter/
│   │   │       ├── business-spec.md.hbs
│   │   │       ├── technical-plan.md.hbs
│   │   │       └── cs-guide.md.hbs
│   │   └── knowledge-base/
│   │       └── getting-started.md.hbs
│   ├── utils/
│   │   ├── filesystem.js        # File operations
│   │   ├── git.js               # Git operations
│   │   └── logger.js            # Pretty console output
│   └── constants.js             # Constants and defaults
└── tests/
    └── ...
```

---

## 📦 Fase 1: Setup do Projeto CLI

### Passo 1.1: Criar Estrutura do Package

```bash
# Na raiz do projeto onion-v4
mkdir -p packages/onion-cli
cd packages/onion-cli

# Inicializar package
npm init -y

# Criar estrutura de diretórios
mkdir -p bin src/{commands,wizard,generator,templates,utils} tests
mkdir -p src/templates/{loaders,agents,commands/starter,knowbase}
```

### Passo 1.2: Instalar Dependências

```bash
npm install --save \
  inquirer \
  commander \
  chalk \
  ora \
  fs-extra \
  yaml \
  handlebars \
  glob \
  execa

npm install --save-dev \
  jest \
  eslint \
  prettier
```

### Passo 1.3: Configurar `package.json`

```json
{
  "name": "@onion/cli",
  "version": "1.0.0",
  "description": "CLI para Sistema Onion - Multi-Context Development Orchestrator",
  "main": "src/index.js",
  "bin": {
    "onion": "bin/onion.js"
  },
  "scripts": {
    "test": "jest",
    "lint": "eslint src/",
    "format": "prettier --write src/",
    "dev": "node bin/onion.js",
    "build": "echo 'No build needed for now'",
    "prepublishOnly": "npm test && npm run lint"
  },
  "keywords": [
    "onion",
    "cli",
    "ai-assistant",
    "claude-code",
    "windsurf",
    "claude",
    "spec-driven-development",
    "multi-context"
  ],
  "author": "Sistema Onion",
  "license": "MIT",
  "repository": {
    "type": "git",
    "url": "https://github.com/your-org/onion-v4"
  },
  "engines": {
    "node": ">=16.0.0"
  },
  "dependencies": {
    "inquirer": "^9.2.12",
    "commander": "^11.1.0",
    "chalk": "^5.3.0",
    "ora": "^7.0.1",
    "fs-extra": "^11.2.0",
    "yaml": "^2.3.4",
    "handlebars": "^4.7.8",
    "glob": "^10.3.10",
    "execa": "^8.0.1"
  },
  "devDependencies": {
    "jest": "^29.7.0",
    "eslint": "^8.55.0",
    "prettier": "^3.1.1"
  }
}
```

---

## 📝 Fase 2: Implementar CLI Core

### Passo 2.1: Entry Point (`bin/onion.js`)

```bash
cat > bin/onion.js << 'EOF'
#!/usr/bin/env node

/**
 * Onion CLI - Entry Point
 * Sistema Onion: Multi-Context Development Orchestrator
 */

const { program } = require('commander');
const chalk = require('chalk');
const pkg = require('../package.json');

// Import commands
const initCommand = require('../src/commands/init');
const addCommand = require('../src/commands/add');
const migrateCommand = require('../src/commands/migrate');
const validateCommand = require('../src/commands/validate');
const helpCommand = require('../src/commands/help');

// ASCII Art
const logo = `
${chalk.magenta('   🧅')}
${chalk.cyan('╔═══════════════════════════════════════╗')}
${chalk.cyan('║')}  ${chalk.bold.white('ONION SYSTEM CLI')}                  ${chalk.cyan('║')}
${chalk.cyan('║')}  ${chalk.gray('Multi-Context Development')}        ${chalk.cyan('║')}
${chalk.cyan('╚═══════════════════════════════════════╝')}
`;

// Program setup
program
  .name('onion')
  .description('Sistema Onion - Multi-Context Development Orchestrator')
  .version(pkg.version)
  .addHelpText('beforeAll', logo);

// Commands
program
  .command('init')
  .description('Inicializar Sistema Onion no projeto atual')
  .option('-y, --yes', 'Usar defaults (skip wizard)')
  .option('-t, --template <name>', 'Usar template predefinido')
  .action(initCommand);

program
  .command('add <type> <name>')
  .description('Adicionar contexto ou IDE')
  .option('-c, --config <path>', 'Path para config customizada')
  .action(addCommand);

program
  .command('migrate')
  .description('Migrar de .claude/ para .onion/')
  .option('--dry-run', 'Mostrar o que seria feito sem executar')
  .option('--backup', 'Criar backup antes de migrar')
  .action(migrateCommand);

program
  .command('validate')
  .description('Validar estrutura .onion/')
  .option('-v, --verbose', 'Output detalhado')
  .option('--fix', 'Tentar corrigir problemas automaticamente')
  .action(validateCommand);

program
  .command('help [command]')
  .description('Mostrar ajuda')
  .action(helpCommand);

// Parse and execute
program.parse(process.argv);

// Show help if no command
if (!process.argv.slice(2).length) {
  program.outputHelp();
}
EOF

chmod +x bin/onion.js
```

### Passo 2.2: Constants (`src/constants.js`)

```bash
cat > src/constants.js << 'EOF'
/**
 * Constantes do Sistema Onion
 */

module.exports = {
  // Versões
  ONION_VERSION: '3.0.0',
  CONFIG_VERSION: '1.0.0',
  
  // Paths
  ONION_ROOT: '.onion',
  CONFIG_FILE: '.onion-config.yml',
  
  // Contextos disponíveis
  AVAILABLE_CONTEXTS: [
    {
      id: 'business',
      name: 'Business',
      description: 'Product specs, features, tasks',
      default: true
    },
    {
      id: 'technical',
      name: 'Technical',
      description: 'Development, architecture, PR',
      default: true
    },
    {
      id: 'customer-success',
      name: 'Customer Success',
      description: 'Support, docs, onboarding',
      default: false
    },
    {
      id: 'compliance',
      name: 'Compliance',
      description: 'Audit, security, legal',
      default: false
    },
    {
      id: 'sales',
      name: 'Sales',
      description: 'Proposals, demos, pipeline',
      default: false
    },
    {
      id: 'marketing',
      name: 'Marketing',
      description: 'Campaigns, content, analytics',
      default: false
    }
  ],
  
  // IDEs suportados
  SUPPORTED_IDES: [
    {
      id: 'claude-code',
      name: 'Claude Code',
      detector: '.claude',
      loader: 'claude.js',
      configFile: 'settings.json'
    },
    {
      id: 'windsurf',
      name: 'Windsurf',
      detector: '.windsurf',
      loader: 'windsurf.ts',
      configFile: 'settings.yml'
    },
    {
      id: 'claude-code',
      name: 'Claude Code',
      detector: '.claude',
      loader: 'claude.py',
      configFile: 'config.json'
    },
    {
      id: 'vscode',
      name: 'VS Code (GitHub Copilot)',
      detector: '.vscode',
      loader: 'vscode.js',
      configFile: 'settings.json'
    },
    {
      id: 'cline',
      name: 'Cline (VS Code)',
      detector: '.vscode/extensions',
      loader: 'cline.js',
      configFile: 'cline-config.json'
    }
  ],
  
  // Integrações opcionais
  OPTIONAL_INTEGRATIONS: {
    taskManager: [
      { id: 'clickup', name: 'ClickUp' },
      { id: 'asana', name: 'Asana' },
      { id: 'linear', name: 'Linear' },
      { id: 'jira', name: 'Jira' },
      { id: 'none', name: 'None (skip)' }
    ],
    transcription: [
      { id: 'whisper-local', name: 'Whisper (local)' },
      { id: 'assemblyai', name: 'AssemblyAI (API)' },
      { id: 'deepgram', name: 'Deepgram (API)' },
      { id: 'none', name: 'None (skip)' }
    ]
  },
  
  // Templates
  PROJECT_TYPES: [
    {
      id: 'monorepo',
      name: 'Monorepo',
      description: 'Multiple contexts (business, technical, CS)',
      defaultContexts: ['business', 'technical']
    },
    {
      id: 'single-app',
      name: 'Single App',
      description: 'One context only',
      defaultContexts: ['technical']
    },
    {
      id: 'library',
      name: 'Library',
      description: 'No contexts, just core',
      defaultContexts: []
    }
  ]
};
EOF
```

### Passo 2.3: Utils - Logger (`src/utils/logger.js`)

```bash
cat > src/utils/logger.js << 'EOF'
/**
 * Pretty console logger
 */
const chalk = require('chalk');
const ora = require('ora');

class Logger {
  constructor() {
    this.spinner = null;
  }
  
  // Títulos
  title(text) {
    console.log('\n' + chalk.cyan.bold('━'.repeat(60)));
    console.log(chalk.cyan.bold(`  ${text}`));
    console.log(chalk.cyan.bold('━'.repeat(60)));
  }
  
  // Seções
  section(text) {
    console.log('\n' + chalk.white.bold(text));
  }
  
  // Sucesso
  success(text) {
    console.log(chalk.green('✅ ') + text);
  }
  
  // Info
  info(text) {
    console.log(chalk.blue('ℹ️  ') + text);
  }
  
  // Warning
  warn(text) {
    console.log(chalk.yellow('⚠️  ') + text);
  }
  
  // Erro
  error(text) {
    console.log(chalk.red('❌ ') + text);
  }
  
  // Spinner
  startSpinner(text) {
    this.spinner = ora(text).start();
  }
  
  stopSpinner(success = true, text = null) {
    if (!this.spinner) return;
    
    if (success) {
      this.spinner.succeed(text);
    } else {
      this.spinner.fail(text);
    }
    this.spinner = null;
  }
  
  // Lista
  list(items, prefix = '  ∟') {
    items.forEach(item => {
      console.log(chalk.gray(prefix) + ' ' + item);
    });
  }
  
  // Quebra
  break() {
    console.log();
  }
}

module.exports = new Logger();
EOF
```

---

## 🧙 Fase 3: Implementar Wizard

### Passo 3.1: Detector de IDEs (`src/wizard/detector.js`)

```bash
cat > src/wizard/detector.js << 'EOF'
/**
 * Detector de IDEs e ferramentas instaladas
 */
const fs = require('fs-extra');
const path = require('path');
const { SUPPORTED_IDES } = require('../constants');

class Detector {
  constructor(projectRoot = process.cwd()) {
    this.projectRoot = projectRoot;
  }
  
  // Detectar IDEs instalados
  detectIDEs() {
    const detected = [];
    
    for (const ide of SUPPORTED_IDES) {
      const detectorPath = path.join(this.projectRoot, ide.detector);
      
      if (fs.existsSync(detectorPath)) {
        detected.push({
          ...ide,
          path: detectorPath
        });
      }
    }
    
    return detected;
  }
  
  // Verificar se já existe estrutura Onion
  hasOnionStructure() {
    return fs.existsSync(path.join(this.projectRoot, '.onion'));
  }
  
  // Verificar se existe estrutura legacy (.claude/)
  hasLegacyStructure() {
    const legacyPaths = [
      '.claude/commands',
      '.claude/agents',
      '.claude/rules'
    ];
    
    return legacyPaths.some(p => 
      fs.existsSync(path.join(this.projectRoot, p))
    );
  }
  
  // Detectar package.json
  getProjectInfo() {
    const pkgPath = path.join(this.projectRoot, 'package.json');
    
    if (fs.existsSync(pkgPath)) {
      return fs.readJsonSync(pkgPath);
    }
    
    return null;
  }
  
  // Verificar Git
  isGitRepository() {
    return fs.existsSync(path.join(this.projectRoot, '.git'));
  }
}

module.exports = Detector;
EOF
```

### Passo 3.2: Wizard Steps (`src/wizard/steps.js`)

```bash
cat > src/wizard/steps.js << 'EOF'
/**
 * Wizard steps para onion init
 */
const inquirer = require('inquirer');
const chalk = require('chalk');
const Detector = require('./detector');
const {
  PROJECT_TYPES,
  AVAILABLE_CONTEXTS,
  SUPPORTED_IDES,
  OPTIONAL_INTEGRATIONS
} = require('../constants');

class WizardSteps {
  constructor(projectRoot = process.cwd()) {
    this.projectRoot = projectRoot;
    this.detector = new Detector(projectRoot);
    this.answers = {};
  }
  
  // Logo
  showWelcome() {
    console.clear();
    console.log(chalk.magenta.bold('\n   🧅 ONION SYSTEM SETUP\n'));
    console.log(chalk.cyan('━'.repeat(60)));
    console.log();
  }
  
  // Step 1/4: Project Type
  async askProjectType() {
    console.log(chalk.white.bold('Step 1/4: Configure Project Type\n'));
    console.log('What type of project is this?\n');
    
    const answer = await inquirer.prompt([
      {
        type: 'list',
        name: 'projectType',
        message: 'Select project type:',
        choices: PROJECT_TYPES.map(pt => ({
          name: `${pt.name} - ${chalk.gray(pt.description)}`,
          value: pt.id
        }))
      }
    ]);
    
    this.answers.projectType = answer.projectType;
    
    // Separador
    console.log('\n' + chalk.cyan('━'.repeat(60)) + '\n');
    
    return answer;
  }
  
  // Step 2/4: Contexts
  async askContexts() {
    console.log(chalk.white.bold('Step 2/4: Select Contexts\n'));
    console.log('Which contexts will your team use?\n');
    
    // Defaults baseados no project type
    const projectTypeData = PROJECT_TYPES.find(
      pt => pt.id === this.answers.projectType
    );
    
    const answer = await inquirer.prompt([
      {
        type: 'checkbox',
        name: 'contexts',
        message: 'Select contexts:',
        choices: AVAILABLE_CONTEXTS.map(ctx => ({
          name: `${ctx.name} - ${chalk.gray(ctx.description)}`,
          value: ctx.id,
          checked: projectTypeData.defaultContexts.includes(ctx.id)
        })),
        validate: (input) => {
          if (this.answers.projectType === 'library' && input.length > 0) {
            return 'Library projects should not have contexts';
          }
          if (this.answers.projectType !== 'library' && input.length === 0) {
            return 'Please select at least one context';
          }
          return true;
        }
      }
    ]);
    
    this.answers.contexts = answer.contexts;
    
    console.log(chalk.green(`\n> Selected: ${answer.contexts.join(', ')}`));
    console.log('\n' + chalk.cyan('━'.repeat(60)) + '\n');
    
    return answer;
  }
  
  // Step 3/4: IDEs
  async askIDEs() {
    console.log(chalk.white.bold('Step 3/4: Detect AI Tools\n'));
    console.log('Which AI assistants do you use?\n');
    
    // Detectar IDEs automaticamente
    const detectedIDEs = this.detector.detectIDEs();
    
    if (detectedIDEs.length > 0) {
      console.log(chalk.green('Detected automatically:'));
      detectedIDEs.forEach(ide => {
        console.log(chalk.green(`  ☑ ${ide.name}`) + chalk.gray(` (found: ${ide.detector})`));
      });
      console.log();
    }
    
    // Perguntar por adicionais
    const availableIDEs = SUPPORTED_IDES.filter(
      ide => !detectedIDEs.find(d => d.id === ide.id)
    );
    
    const answer = await inquirer.prompt([
      {
        type: 'checkbox',
        name: 'additionalIDEs',
        message: 'Select additional IDEs:',
        choices: [
          ...availableIDEs.map(ide => ({
            name: ide.name,
            value: ide.id
          })),
          new inquirer.Separator(),
          {
            name: 'Universal AGENTS.md (fallback for all IDEs)',
            value: 'universal',
            checked: true
          }
        ]
      }
    ]);
    
    this.answers.ides = [
      ...detectedIDEs.map(ide => ide.id),
      ...answer.additionalIDEs
    ];
    
    console.log('\n' + chalk.cyan('━'.repeat(60)) + '\n');
    
    return answer;
  }
  
  // Step 4/4: Integrations
  async askIntegrations() {
    console.log(chalk.white.bold('Step 4/4: Configure Integrations\n'));
    
    const answers = await inquirer.prompt([
      {
        type: 'list',
        name: 'taskManager',
        message: 'Task Manager (optional):',
        choices: OPTIONAL_INTEGRATIONS.taskManager.map(tm => ({
          name: tm.name,
          value: tm.id
        })),
        default: 'none'
      },
      {
        type: 'list',
        name: 'transcription',
        message: 'Transcription Service (optional):',
        choices: OPTIONAL_INTEGRATIONS.transcription.map(ts => ({
          name: ts.name,
          value: ts.id
        })),
        default: 'none'
      }
    ]);
    
    this.answers.taskManager = answers.taskManager;
    this.answers.transcription = answers.transcription;
    
    console.log('\n' + chalk.cyan('━'.repeat(60)) + '\n');
    
    return answers;
  }
  
  // Executar todos os steps
  async run() {
    this.showWelcome();
    
    await this.askProjectType();
    await this.askContexts();
    await this.askIDEs();
    await this.askIntegrations();
    
    return this.answers;
  }
}

module.exports = WizardSteps;
EOF
```

---

## 🏭 Fase 4: Generator

### Passo 4.1: Structure Generator (`src/generator/structure.js`)

```bash
cat > src/generator/structure.js << 'EOF'
/**
 * Gerador de estrutura .onion/
 */
const fs = require('fs-extra');
const path = require('path');
const logger = require('../utils/logger');

class StructureGenerator {
  constructor(projectRoot, config) {
    this.projectRoot = projectRoot;
    this.config = config;
    this.onionRoot = path.join(projectRoot, '.onion');
  }
  
  // Criar estrutura completa
  async generate() {
    logger.title('🎉 Creating Onion System structure...');
    logger.break();
    
    await this.createCoreStructure();
    await this.createContexts();
    await this.createIDEStructure();
    await this.createDocs();
    
    logger.break();
    logger.success('Structure created successfully!');
  }
  
  // Core structure
  async createCoreStructure() {
    const corePaths = [
      'core/knowledge-base/concepts',
      'core/knowledge-base/frameworks',
      'core/knowledge-base/tools',
      'core/knowledge-base/learnings',
      'core/agents',
      'core/commands/meta',
      'core/rules',
      'core/utils'
    ];
    
    logger.startSpinner('Creating core structure...');
    
    for (const p of corePaths) {
      await fs.ensureDir(path.join(this.onionRoot, p));
    }
    
    logger.stopSpinner(true, 'Created .onion/core/');
  }
  
  // Context structures
  async createContexts() {
    for (const context of this.config.contexts) {
      logger.startSpinner(`Creating context: ${context}...`);
      
      const contextPaths = [
        `contexts/${context}/knowbase`,
        `contexts/${context}/agents`,
        `contexts/${context}/commands/starter`,
        `contexts/${context}/commands/intermediate`,
        `contexts/${context}/commands/advanced`,
        `contexts/${context}/sessions`
      ];
      
      for (const p of contextPaths) {
        await fs.ensureDir(path.join(this.onionRoot, p));
      }
      
      logger.stopSpinner(true, `Created .onion/contexts/${context}/`);
    }
  }
  
  // IDE structure
  async createIDEStructure() {
    logger.startSpinner('Creating IDE integrations...');
    
    const idePath = path.join(this.onionRoot, 'ide');
    await fs.ensureDir(idePath);
    
    // Universal fallback
    await fs.ensureDir(path.join(idePath, 'universal'));
    
    for (const ide of this.config.ides) {
      if (ide === 'universal') continue;
      await fs.ensureDir(path.join(idePath, ide));
    }
    
    logger.stopSpinner(true, 'Created .onion/ide/');
  }
  
  // Docs structure
  async createDocs() {
    logger.startSpinner('Creating documentation structure...');
    
    const docsPaths = [
      'docs/onion',
      ...this.config.contexts.map(ctx => `docs/${ctx}-context`)
    ];
    
    for (const p of docsPaths) {
      await fs.ensureDir(path.join(this.projectRoot, p));
    }
    
    logger.stopSpinner(true, 'Created docs/ structure');
  }
}

module.exports = StructureGenerator;
EOF
```

---

## ⚙️ Fase 5: Command - Init

### Passo 5.1: Init Command (`src/commands/init.js`)

```bash
cat > src/commands/init.js << 'EOF'
/**
 * onion init - Inicializar Sistema Onion
 */
const Detector = require('../wizard/detector');
const WizardSteps = require('../wizard/steps');
const StructureGenerator = require('../generator/structure');
const ConfigGenerator = require('../generator/config');
const LoadersGenerator = require('../generator/loaders');
const TemplatesGenerator = require('../generator/templates');
const logger = require('../utils/logger');

async function initCommand(options) {
  try {
    const projectRoot = process.cwd();
    const detector = new Detector(projectRoot);
    
    // Verificar se já existe estrutura Onion
    if (detector.hasOnionStructure()) {
      logger.warn('Onion structure already exists in this project!');
      logger.info('Use "onion validate" to check structure');
      logger.info('Use "onion migrate" to migrate from legacy');
      process.exit(1);
    }
    
    // Verificar estrutura legacy
    if (detector.hasLegacyStructure()) {
      logger.warn('Detected legacy .claude/ structure');
      logger.info('Consider running "onion migrate" instead');
      logger.break();
      
      // Perguntar se quer continuar ou migrar
      const { shouldMigrate } = await require('inquirer').prompt([
        {
          type: 'confirm',
          name: 'shouldMigrate',
          message: 'Run migration instead?',
          default: true
        }
      ]);
      
      if (shouldMigrate) {
        const migrateCommand = require('./migrate');
        return await migrateCommand(options);
      }
    }
    
    // Executar wizard
    const wizard = new WizardSteps(projectRoot);
    const config = await wizard.run();
    
    // Gerar estrutura
    const structureGen = new StructureGenerator(projectRoot, config);
    await structureGen.generate();
    
    // Gerar configurações
    const configGen = new ConfigGenerator(projectRoot, config);
    await configGen.generate();
    
    // Gerar loaders
    const loadersGen = new LoadersGenerator(projectRoot, config);
    await loadersGen.generate();
    
    // Gerar templates
    const templatesGen = new TemplatesGenerator(projectRoot, config);
    await templatesGen.generate();
    
    // Adicionar ao Git
    if (detector.isGitRepository()) {
      logger.startSpinner('Adding to git...');
      const { execa } = require('execa');
      await execa('git', ['add', '.onion/']);
      logger.stopSpinner(true, 'Added .onion/ to git');
    }
    
    // Success message
    logger.break();
    logger.title('🚀 Setup complete!');
    logger.break();
    
    logger.section('Next steps:');
    logger.list([
      'Restart your IDE (Claude Code/Windsurf)',
      'Try a starter command:',
      `  - Business: ${require('chalk').cyan('/business/spec "my-feature"')}`,
      `  - Technical: ${require('chalk').cyan('/technical/plan "my-feature"')}`,
      'Read docs: .onion/README.md'
    ]);
    
    logger.break();
    logger.info(`Need help? Run: ${require('chalk').cyan('onion help')}`);
    logger.break();
    
  } catch (error) {
    logger.error(`Failed to initialize: ${error.message}`);
    if (options.verbose) {
      console.error(error);
    }
    process.exit(1);
  }
}

module.exports = initCommand;
EOF
```

---

## 🎬 FLUXO DE EXECUÇÃO COMPLETO

### Comandos Para Você Executar Agora

```bash
# ═══════════════════════════════════════════════════════════════
# FASE 1: Setup do Projeto CLI
# ═══════════════════════════════════════════════════════════════

# Passo 1: Criar estrutura
cd /home/marciocar/onion-v4
mkdir -p packages/onion-cli
cd packages/onion-cli

# Passo 2: Inicializar package
npm init -y

# Passo 3: Criar diretórios
mkdir -p bin src/{commands,wizard,generator,templates,utils} tests
mkdir -p src/templates/{loaders,agents,commands/starter,knowbase}

# Passo 4: Instalar dependências
npm install --save inquirer commander chalk ora fs-extra yaml handlebars glob execa
npm install --save-dev jest eslint prettier

# ═══════════════════════════════════════════════════════════════
# FASE 2: Criar arquivos base (já fornecidos acima)
# ═══════════════════════════════════════════════════════════════

# Execute os comandos cat > ... fornecidos acima para criar:
# - bin/onion.js
# - src/constants.js
# - src/utils/logger.js
# - src/wizard/detector.js
# - src/wizard/steps.js
# - src/generator/structure.js
# - src/commands/init.js

# ═══════════════════════════════════════════════════════════════
# FASE 3: Atualizar package.json
# ═══════════════════════════════════════════════════════════════

# Substituir package.json pelo fornecido acima

# ═══════════════════════════════════════════════════════════════
# FASE 4: Testar localmente
# ═══════════════════════════════════════════════════════════════

# Tornar executável
chmod +x bin/onion.js

# Link local para testar
npm link

# Testar CLI
onion --help
onion --version

# ═══════════════════════════════════════════════════════════════
# FASE 5: Implementar comandos restantes (próximos passos)
# ═══════════════════════════════════════════════════════════════

# Continuar com:
# - src/generator/config.js
# - src/generator/loaders.js
# - src/generator/templates.js
# - src/commands/add.js
# - src/commands/migrate.js
# - src/commands/validate.js
# - src/commands/help.js
```

---

## 📋 Checklist de Implementação

### Fase 1: CLI Core ✅
- [ ] Criar estrutura packages/onion-cli/
- [ ] Instalar dependências
- [ ] Implementar bin/onion.js
- [ ] Implementar src/constants.js
- [ ] Implementar src/utils/logger.js
- [ ] Testar `onion --help`

### Fase 2: Wizard ⏳
- [ ] Implementar src/wizard/detector.js
- [ ] Implementar src/wizard/steps.js
- [ ] Testar wizard isoladamente

### Fase 3: Generators ⏳
- [ ] Implementar src/generator/structure.js
- [ ] Implementar src/generator/config.js
- [ ] Implementar src/generator/loaders.js
- [ ] Implementar src/generator/templates.js

### Fase 4: Commands ⏳
- [ ] Implementar src/commands/init.js
- [ ] Implementar src/commands/add.js
- [ ] Implementar src/commands/migrate.js
- [ ] Implementar src/commands/validate.js
- [ ] Implementar src/commands/help.js

### Fase 5: Templates ⏳
- [ ] Criar templates Handlebars
- [ ] Criar templates de loaders
- [ ] Criar templates de comandos starter
- [ ] Criar templates de agentes

### Fase 6: Testes ⏳
- [ ] Testes unitários (Jest)
- [ ] Testes de integração
- [ ] Testar em projeto real

### Fase 7: Publish ⏳
- [ ] Atualizar README.md
- [ ] Configurar CI/CD
- [ ] Publicar no NPM
- [ ] Documentar em docs/onion/

---

## 🎯 Próximo Passo Imediato

Execute agora:

```bash
cd /home/marciocar/onion-v4
mkdir -p packages/onion-cli
cd packages/onion-cli
npm init -y
```

Depois me avise que continuo fornecendo os próximos arquivos! 🧅✨

