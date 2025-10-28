**Propósito:** Documento que define, planeja, executa e mantém os testes (funcionais, regressivos, integração e automação) alinhados ao CI/CD e rastreabilidade no Xray.

> Versão: 1.0
> Autor: Yann Brenno Da Costa Ramos
> Data: 28/10/2025

---

## 1. Sumário executivo

1.1 Objetivo do documento

* Definir escopo, responsabilidades e critérios para garantir qualidade contínua.

1.2 Público-alvo

* QA, Devs, Product Owner, DevOps, Scrum Master.

---

## 2. Escopo e limites

2.1 Escopo

1. Testes funcionais (UI/Fluxo)
2. Testes de integração (serviços, APIs)
3. Testes de regressão (suites liberáveis)
4. Automação (UI + API)

2.2 Exclusões

* Testes de performance e segurança (podem ser projetos separados)

---

## 3. Objetivos de qualidade (KPIs)

1. Cobertura por requisito: ≥ X% (definir alvo)
2. Tempo médio de execução da pipeline: ≤ Y minutos
3. Taxa de flaky: < Z%
4. Tempo médio de reparo de testes falhos: ≤ W dias

---

## 4. Tipos de testes e critérios

4.1 Unitários

* Responsável: Devs
* Quando executar: em cada PR (pipeline)

4.2 Integração

* Responsável: Devs/QAs
* Quando executar: em PR e nightly

4.3 Funcionais (E2E)

* Responsável: QA
* Quando executar: em PR (smoke) e em pipeline de release (regressão completa)

4.4 Regressão

* Suite estabilizada, executada antes do deploy em produção

4.5 Automação

* Critérios para automatizar um caso: repetibilidade, risco, valor de negócio, tempo economizado

---

## 5. Integração com CI/CD

5.1 Políticas de branch

* Branches: `main`, `develop`, `feature/*`, `hotfix/*`
* Política de PR: revisão, checks de build e testes automatizados

5.2 Pipeline (exemplo mínimo)

1. Build
2. Unit tests
3. Deploy em ambiente de teste
4. Integration tests
5. Smoke tests (PR)
6. Full regression (release)
7. Publicar resultados no Xray

5.3 Artefatos gerados

* Relatórios (HTML/JSON)
* Logs e screenshots
* Evidências anexadas ao Xray

---

## 6. Rastreabilidade no Xray (Jira)

6.1 Estrutura recomendada

1. Requisito (Story/Issue)
2. Test Case (Xray Test)
3. Test Execution
4. Defect-link

6.2 Regras práticas

* Todo requisito deve ter pelo menos 1 Test ligado.
* Testes automatizados devem possuir mapeamento entre o arquivo/ID de teste e o Test issue do Xray.
* Upload automático dos resultados via Xray REST API a cada execução de pipeline.

6.3 Exemplo de mapeamento

* `Story-123` → `Test-456` (manual) → `TestExec-789` (executions)

---

## 7. Dados de teste e ambientes

7.1 Ambientes

* dev, qa, homolog, prod (apenas leitura/evidência)

7.2 Gestão de dados

* Fixtures compartilhadas no repositório
* Mock vs dados reais: quando usar cada um
* Limpeza e reset entre execuções

---

## 8. Critérios de entrada e saída

8.1 Definition of Ready (DoR) para testar

* História aceita pelo PO
* Critérios de aceitação definidos
* Build estável

8.2 Definition of Done (DoD) para liberação

* Todos os testes críticos passaram
* Bugs críticos bloqueando resolvidos
* Evidências no Xray anexadas

---

## 9. Boas práticas e padrões de escrita de casos

9.1 Padronização (recomendado)

* Use Gherkin (Given / When / Then) para testes funcionais
* IDs claros: `CT01 - LOGIN ABC`

9.2 Nível de detalhe

* Passos objetivos, pré-condições, dados esperados, evidências

9.3 Organização

* Tags por tipo: `@smoke`, `@regression`, `@api`

---

## 10. Estratégia de automação

10.1 Critérios de automação (prioridade)

1. Testes de alto valor/risco
2. Testes repetitivos
3. Testes que aceleram pipelines

10.2 Arquitetura sugerida

* Padrão POM para UI
* Camada de serviços/clients para APIs
* Reutilização de fixtures e factories

10.3 Execução

* Paralelização em CI
* Retentativas controladas para flaky detection

---

## 11. Métricas e relatórios

11.1 Relatórios obrigatórios

* Cobertura por requisito (Xray)
* Pass/Fail por execução
* Tendência de flaky

11.2 Frequência

* Dashboards em tempo real no Jira/Xray
* Relatório semanal de saúde de testes

---

## 12. Plano de manutenção e governança

12.1 Revisão periódica

* Revisão trimestral de suites de regressão

12.2 Responsabilidades

* QA Lead: governança do conjunto de testes
* Devs: manutenção dos unit/integration
* DevOps: manter pipelines

12.3 Processo de aposentadoria de testes

* Critérios para remoção/refactor de testes obsoletos

---

## 13. Checklists e templates (anexos)

13.1 Template resumo da história → casos de teste

* ID Teste:
* Título:
* Tipo: (funcional/api/regressão)
* Pré-condição:
* Passos:
* Resultado esperado:
* Tags:
* Evidência:

13.2 Checklist de PR

* Build verde
* Unit tests pass
* Smoke tests (se aplicável)
* Atualizar mapeamento Xray (se novos testes)

---

## 14. Riscos conhecidos e mitigação

* Flaky tests: estratégia de isolamento e retrys
* Ambiente instável: usar containers e mocks
* Falta de cobertura: priorizar requisitos críticos

---

## 15. Próximos passos (curto prazo)

1. Preencher metas de KPI (valores X/Y/Z/A)
2. Mapear 10 requisitos críticos e criar 1 smoke test para cada
3. Configurar CI para upload de resultados no Xray (exemplo de script)
---

## Anexos

* Exemplos de scripts de upload Xray (placeholder)
* Link para templates de casos de teste
https://yannbrenno.atlassian.net/jira/software/projects/PROJ/boards/2
https://yannbrenno.atlassian.net/wiki/spaces/~638502298fd2d2d5f12f9428/overview
