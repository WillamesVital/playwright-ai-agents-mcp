# 🚀 Guia Prático de Comandos e Prompts: Playwright + Agentes MCP

Material complementar da série **Playwright na Prática** do canal Willames Vital QA.

Ambiente:
- VS Code
- Extensão do Codex
- Playwright MCP Server

## Comando de inicialização

Execute na pasta raiz do projeto:

```bash
npx playwright init-agents --loop=vscode
```

Esse comando cria a estrutura necessária para trabalhar com agentes Playwright e MCP.

Estrutura esperada:

```text
specs/
tests/seed.spec.ts
.github/agents/
  playwright-test-generator.agent.md
  playwright-test-healer.agent.md
  playwright-test-planner.agent.md
.vscode/mcp.json
```

## Prompts para o Codex

### Prompt 1 — Inicialização do projeto e ativação do MCP

```text
Inicialize um novo projeto Playwright com agentes MCP para integração no VS Code. Instale as dependências necessárias, incluindo os navegadores do Playwright. Execute o teste inicial para validar a configuração e inicie o servidor MCP do Playwright para as ferramentas de testes locais.
```

### Prompt 2 — Orquestração Planner → Generator → Healer

```text
Preciso escrever um novo teste para o login do site OrangeHRM (http://opensource-demo.orangehrmlive.com). Por favor, atue como o orquestrador e siga estritamente o ciclo de vida:

1. Leia as regras em .github/agents/playwright-test-planner.agent.md, analise a página de destino via MCP e gere uma estratégia passo a passo para o teste em Markdown na pasta specs/. Pare e aguarde a minha aprovação.

2. Assim que eu aprovar, mude para .github/agents/playwright-test-generator.agent.md e escreva o código do Playwright em TypeScript no diretório tests/.

3. Execute os casos de teste. Se algum teste falhar, mude para .github/agents/playwright-test-healer.agent.md para analisar a falha e corrigir o código de forma autônoma.
```

### Prompt 3 — Aprovação do plano

```text
Aprovado. Prossiga com a geração do arquivo de teste em Playwright TypeScript.
```

## Exemplo de teste gerado

```typescript
import { test, expect } from '@playwright/test';

test.describe('Autenticação no Sistema OrangeHRM', () => {
  test('Deve realizar login com sucesso utilizando credenciais válidas', async ({ page }) => {
    await page.goto('https://opensource-demo.orangehrmlive.com/web/index.php/auth/login');

    await page.getByPlaceholder('Username').fill('Admin');
    await page.getByPlaceholder('Password').fill('admin123');

    await page.getByRole('button', { name: 'Login' }).click();

    await expect(page).toHaveURL(/.*dashboard/);
    await expect(page.getByRole('heading', { name: 'Dashboard' })).toBeVisible();
  });
});
```

## Testando o Healer

Para simular uma falha:

1. Abra o arquivo de teste.
2. Altere propositalmente o locator:

```typescript
await page.getByPlaceholder('Sign In').fill('Admin');
```

3. Execute o teste.
4. Utilize o fluxo do Healer para analisar a falha e corrigir o locator via MCP.

---

Este documento será atualizado conforme a evolução do projeto durante a playlist Playwright na Prática.
