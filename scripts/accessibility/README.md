# Avaliação automatizada de acessibilidade

Os scripts desta pasta apoiam a avaliação de acessibilidade do Inclusivo,
registrando verificações automatizadas e evidências para análise no TCC.

## Ferramentas utilizadas

- **Playwright:** reproduz interações e verifica o comportamento da interface em perfis de desktop e celular.
- **axe-core:** identifica violações automatizáveis de regras associadas à WCAG.
- **ESLint com jsx-a11y:** analisa práticas de acessibilidade no código dos componentes.
- **Lighthouse:** complementa a avaliação de páginas públicas.

## Cenários avaliados

Os testes abrangem páginas públicas e autenticadas, navegação por teclado,
foco, preferências de acessibilidade, formulários, mensagens e notificações.
Também contemplam estados de carregamento, erro e recuperação.

Os cenários utilizam respostas de API controladas ou um backend real com banco
descartável, conforme o objetivo de cada teste.

## Organização dos scripts

- `run.mjs`: coordena as etapas da avaliação e registra o ambiente utilizado.
- `report.mjs`: reúne os resultados e gera os relatórios.
- `compare.mjs`: compara achados e cobertura entre avaliações.
- `lighthouse.mjs`: realiza as verificações com Lighthouse.
- `sandbox.mjs` e `sandbox.py`: preparam o backend e os dados isolados.
- `../frontend/e2e/accessibility/` e `../frontend/e2e/evaluation/`: contêm os cenários de teste.
- `../frontend/e2e/helpers/accessibility-audit.ts`: reúne funções de auditoria e verificação de foco.

## Evidências e limites

A avaliação produz relatórios, arquivos JSON e capturas de tela para apoiar a
análise dos achados. Violações automáticas, avisos de código, falhas de teste e
resultados inconclusivos são apresentados separadamente.

A ausência de violações automáticas não comprova conformidade integral com WCAG
ou ABNT. A avaliação deve ser complementada por revisão humana, incluindo o uso
de leitores de tela e a análise da adequação do conteúdo.
