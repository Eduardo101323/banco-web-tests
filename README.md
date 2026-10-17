# banco-web-tests

Projeto de testes automatizados com Cypress e JavaScript, criado para mostrar aos alunos da **Mentoria 2.0** como estruturar e escrever testes end-to-end usando Cypress e Custom Commands.

Os testes exercitam os fluxos de login e transferência da aplicação web do banco. Para executá-los, a API e a aplicação web precisam estar em execução e integradas entre si.

## Componentes do projeto

- **Cypress**: ferramenta de automação e execução dos testes end-to-end.
- **Testes em `cypress/e2e/`**: cenários de login e transferência.
- **Custom Commands em `cypress/support/commands/`**: ações reutilizáveis de login, seleção de opções, transferência e validação de mensagens.
- **Fixtures em `cypress/fixtures/`**: dados usados pelos testes, incluindo as credenciais em `credenciais.json`.
- **Configuração em `cypress.config.js`**: define a URL base da aplicação (`http://localhost:4000`) e o reporter.
- **cypress-mochawesome-reporter**: gera relatórios dos resultados dos testes.

## Pré-requisitos

- Node.js e npm instalados.
- A API [banco-api](https://github.com/juliodelimas/banco-api) em execução.
- A aplicação web [banco-web](https://github.com/juliodelimas/banco-web) em execução e configurada para utilizar a API.

A aplicação web deve estar disponível em `http://localhost:4000`, conforme a `baseUrl` configurada no Cypress. Siga as instruções de instalação e inicialização nos repositórios da API e da aplicação. Inicie ambos antes de abrir ou executar os testes.

## Instalação

Clone este repositório e instale as dependências:

```bash
git clone https://github.com/Eduardo101323/banco-web-tests.git
cd banco-web-tests
npm install
```

## Executar os testes

Abra o Cypress em modo interativo:

```bash
npm run cy:open
```

Na interface do Cypress, selecione **E2E Testing** e execute os arquivos de teste.

Para executar todos os testes em modo headless:

```bash
npm test
```

Também estão disponíveis:

```bash
npm run cy:headed
npm run test-qa
npm run test-prod
```

`cy:headed` executa os testes com o navegador visível. Atualmente, `test`, `test-qa` e `test-prod` executam o mesmo comando (`cypress run`); os nomes `qa` e `prod` não configuram ambientes diferentes.

## Testes automatizados

| Arquivo | Cenários |
| --- | --- |
| `cypress/e2e/login.cy.js` | Login com credenciais válidas, verificando o acesso à tela de transferência; login com credenciais inválidas, verificando a mensagem de erro. |
| `cypress/e2e/transferencia.cy.js` | Transferência de valor válido, verificando a confirmação; tentativa de transferência acima de R$ 5.000,00 sem token, verificando a mensagem de autenticação necessária. |

Os testes de transferência fazem login antes de cada cenário e usam Maria como conta de origem e João como conta de destino.

## Custom Commands

Os comandos são registrados por `cypress/support/commands.js`, que importa os arquivos da pasta `cypress/support/commands/`. O suporte é carregado automaticamente por `cypress/support/e2e.js`.

| Comando | Arquivo | Descrição |
| --- | --- | --- |
| `cy.fazerLoginComCredenciaisValidas()` | `login.js` | Lê `credenciais.valida` da fixture `credenciais.json`, preenche usuário e senha e envia o formulário. |
| `cy.fazerLoginComCredenciaisInvalidas()` | `login.js` | Lê `credenciais.invalida` da mesma fixture, preenche o formulário e tenta entrar. |
| `cy.selecionarOpcaoNaCombobox(labelDoCampo, opcao)` | `common.js` | Abre a combobox identificada pelo `label` e seleciona a opção informada. |
| `cy.verificarMensagemNoToast(mensagem)` | `common.js` | Verifica se o elemento `.toast` exibe exatamente a mensagem esperada. |
| `cy.realizarTransferencia(contaOrigem, contaDestino, valor)` | `transferencias.js` | Seleciona as contas de origem e destino, preenche o valor e aciona a transferência. |

As credenciais dos cenários ficam em `cypress/fixtures/credenciais.json`. Se os dados ou o estado do ambiente da aplicação forem diferentes, atualize a fixture ou os dados dos cenários conforme necessário.

## Relatórios

O reporter Mochawesome é registrado na configuração do Cypress e no arquivo de suporte dos testes. Após executar os testes, consulte o relatório HTML gerado em:

```text
cypress/reports/html/index.html
```
