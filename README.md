# banco-api-performance

## Introdução

Este repositório contém testes de performance desenvolvidos em **JavaScript** com **Grafana k6** para validar o comportamento da API do projeto Banco API em cenários de carga.

Os scripts foram organizados para facilitar a reutilização de dados, helpers de autenticação e variáveis compartilhadas. A URL-base da API deve ser informada por meio da variável de ambiente `BASE_URL` no momento da execução dos testes.

---

## Tecnologias utilizadas

- **JavaScript** — linguagem utilizada na implementação dos scripts de teste.
- **Grafana k6** — ferramenta de teste de carga e performance.
- **JSON** — utilizado para configuração local e dados de apoio aos testes.

---

## Estrutura do repositório

```text
banco-api-performance/
├── config/
│   └── config.local.json
├── fixtures/
│   └── postLogin.json
├── helpers/
│   └── autenticacao.js
├── tests/
│   ├── login.test.js
│   └── transferencias.test.js
├── utils/
│   └── variaveis.js
└── README.md
```

---

## Objetivo de cada grupo de arquivos

### `config/`
Contém arquivos de configuração do projeto.

- **`config.local.json`**: arquivo de apoio para configuração local da URL-base da API. Pode ser usado como fallback em ambiente local.

### `fixtures/`
Armazena massas de dados reutilizáveis pelos testes.

- **`postLogin.json`**: payload utilizado no endpoint de login.

### `helpers/`
Centraliza funções auxiliares reutilizadas por diferentes scripts.

- **`autenticacao.js`**: responsável por executar o login e obter o token de autenticação utilizado em outros testes.

### `tests/`
Contém os cenários de teste de performance executados com o k6.

- **`login.test.js`**: teste de carga do endpoint de login.
- **`transferencias.test.js`**: teste do endpoint de transferências, reutilizando autenticação prévia.

### `utils/`
Agrupa utilitários compartilhados entre os scripts.

- **`variaveis.js`**: centraliza a leitura da `BASE_URL`, priorizando a variável de ambiente informada na execução do teste.

---

## Instalação

### 1. Clonar o repositório

```bash
git clone https://github.com/samuelssaquino/banco-api-performance.git
cd banco-api-performance
```

### 2. Instalar o k6

Instale o **Grafana k6** na sua máquina de acordo com o seu sistema operacional.

#### Windows (winget)
```bash
winget install k6 --source winget
```

#### Windows (Chocolatey)
```bash
choco install k6
```

#### Linux (Debian/Ubuntu)
```bash
sudo gpg -k
sudo gpg --no-default-keyring --keyring /usr/share/keyrings/k6-archive-keyring.gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69
echo "deb [signed-by=/usr/share/keyrings/k6-archive-keyring.gpg] https://dl.k6.io/deb stable main" | sudo tee /etc/apt/sources.list.d/k6.list
sudo apt-get update
sudo apt-get install k6
```

### 3. Configurar a URL da API

Este projeto utiliza a variável de ambiente **`BASE_URL`** para definir a URL-base da API durante a execução.

Exemplo:

```bash
BASE_URL=http://localhost:3000
```

> Observação: o projeto possui um arquivo `config/config.local.json` como apoio local, mas a forma recomendada neste repositório é informar a `BASE_URL` na execução do k6.

---

## Execução do projeto

### Executar o teste de login

#### Linux/macOS
```bash
BASE_URL=http://localhost:3000 k6 run tests/login.test.js
```

#### Windows PowerShell
```powershell
$env:BASE_URL="http://localhost:3000"
k6 run .\tests\login.test.js
```

#### Windows CMD
```cmd
set BASE_URL=http://localhost:3000 && k6 run tests\login.test.js
```

### Executar o teste de transferências

#### Linux/macOS
```bash
BASE_URL=http://localhost:3000 k6 run tests/transferencias.test.js
```

#### Windows PowerShell
```powershell
$env:BASE_URL="http://localhost:3000"
k6 run .\tests\transferencias.test.js
```

#### Windows CMD
```cmd
set BASE_URL=http://localhost:3000 && k6 run tests\transferencias.test.js
```

### Executar com dashboard web em tempo real

O k6 permite acompanhar a execução em tempo real em um dashboard web local.

#### Linux/macOS
```bash
BASE_URL=http://localhost:3000 K6_WEB_DASHBOARD=true k6 run tests/login.test.js
```

#### Windows PowerShell
```powershell
$env:BASE_URL="http://localhost:3000"
$env:K6_WEB_DASHBOARD="true"
k6 run .\tests\login.test.js
```

### Executar com exportação do relatório HTML

Para exportar automaticamente um relatório HTML ao final da execução:

#### Linux/macOS
```bash
BASE_URL=http://localhost:3000 K6_WEB_DASHBOARD=true K6_WEB_DASHBOARD_EXPORT=html-report.html k6 run tests/login.test.js
```

#### Windows PowerShell
```powershell
$env:BASE_URL="http://localhost:3000"
$env:K6_WEB_DASHBOARD="true"
$env:K6_WEB_DASHBOARD_EXPORT="html-report.html"
k6 run .\tests\login.test.js
```

#### Exemplo equivalente com variáveis do próprio k6
```bash
K6_WEB_DASHBOARD=true K6_WEB_DASHBOARD_EXPORT=html-report.html BASE_URL=http://localhost:3000 k6 run tests/login.test.js
```

### Saídas esperadas

- **Resumo no terminal** ao final da execução.
- **Dashboard web local** durante a execução, quando `K6_WEB_DASHBOARD=true`.
- **Arquivo HTML** com o relatório, quando `K6_WEB_DASHBOARD_EXPORT=html-report.html` for informado.

---

## Observações finais

- A função utilitária responsável por recuperar a URL-base prioriza `BASE_URL` via variável de ambiente.
- O helper de autenticação encapsula a obtenção de token para reutilização em cenários autenticados.
- Os arquivos em `fixtures/` ajudam a evitar duplicação de payloads nos testes.

