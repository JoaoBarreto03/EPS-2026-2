# Relatório de Prontidão Técnica: Onboarding SecOps

**Disciplina:** Engenharia de Produto de Software (FGA0316) - 2026.1
**Aluno:** João Manoel Barreto Neto | **Matrícula:** 211039519

---

## 1. Configuração do Ambiente (Zero Trust & Isolamento)

Conforme as diretrizes de Soberania Técnica, as seguintes configurações foram aplicadas:

- [x] **Python 3.12:** Instalado e verificado.
- [x] **Poetry:** Configurado para criar `.venv` dentro do projeto (`virtualenvs.in-project true`).
- [x] **Determinismo:** Arquivos `pyproject.toml` e `poetry.lock` gerados com sucesso.

> **Comandos executados para configuração:**
> ```bash
> python --version          # Python 3.12.x
> poetry config virtualenvs.in-project true
> poetry init
> poetry install
> ```

---

## 2. Logs de Auditoria e Qualidade (Security Gate)

Abaixo constam os resumos das execuções dos comandos de segurança:

### 2.1. Auditoria Estática (Bandit)

*Comando: `poetry run bandit -r .`*

```
Run started:2026-04-01 23:51:12.404137+00:00

Test results:
>> Issue: [B101:assert_used] Use of assert detected. The enclosed code will be removed when compiling to optimised byte code.
   Severity: Low   Confidence: High
   CWE: CWE-703 (https://cwe.mitre.org/data/definitions/703.html)
   More Info: https://bandit.readthedocs.io/en/1.9.4/plugins/b101_assert_used.html
   Location: ./tests/test_placeholder.py:3:4

--------------------------------------------------

Code scanned:
	Total lines of code: 3
	Total lines skipped (#nosec): 0
	Total potential issues skipped due to specifically being disabled (e.g., #nosec BXXX): 0

Run metrics:
	Total issues (by severity):
		Undefined: 0
		Low: 1
		Medium: 0
		High: 0
	Total issues (by confidence):
		Undefined: 0
		Low: 0
		Medium: 0
		High: 1
Files skipped (0):
```

> Vulnerabilidades encontradas: **0 críticas / 0 médias / 1 baixa** (uso de `assert` em arquivo de teste — comportamento esperado em suítes de teste com `pytest`)

---

### 2.2. Verificação de Dependências (Safety)

*Comando: `poetry run safety check`*

```
Safety v3.7.0 is scanning for Vulnerabilities...
Scanning dependencies in your environment:

  -> .venv/lib/python3.12/site-packages

  Using open-source vulnerability database
  Found and scanned 51 packages
  Timestamp 2026-04-01 21:01:23
  0 vulnerabilities reported
  0 vulnerabilities ignored

No known security vulnerabilities reported.
```

---

### 2.3. Qualidade e Conformidade (Ruff)

*Comando: `poetry run ruff check .`*

```
All checks passed!
```

---

## 3. Evidência de Integração Contínua (CI)

O pipeline automatizado foi executado com sucesso no GitHub Actions:

- **Link da Action de Sucesso:** https://github.com/JoaoBarreto03/EPS-2026-2/actions/runs/23876438179

---

## 4. Declaração de Soberania Técnica (CISSP Domain 8)

Eu, João Manoel Barreto Neto, declaro que auditei manualmente as ferramentas e dependências deste projeto. Confirmo que o código gerado via IA (GitHub Copilot) passou pela minha revisão humana (*Human-in-the-loop*), garantindo que não há vazamento de segredos ou falhas lógicas críticas antes da migração para o ecossistema da PCDF.

---

**Data de Entrega:** 01/04/2026

---

## 5. Observações Gerais e Específicas

### 5.1. Aspectos identificados e recomendados para o futuro

**Gestão de segredos:**
Nenhuma credencial, token ou chave de API deve ser versionada no repositório. Recomenda-se o uso de variáveis de ambiente via `.env` (com `.gitignore` configurado) e, em ambiente de produção, um vault dedicado (ex.: HashiCorp Vault ou AWS Secrets Manager).

**Cobertura de testes:**
A ausência de testes automatizados no estado atual do projeto é um risco de segurança e qualidade. Recomenda-se adicionar testes unitários com `pytest` e incluir a cobertura mínima de 80% como gate no CI/CD.

**Atualização contínua de dependências:**
O arquivo `poetry.lock` garante determinismo, mas as dependências precisam ser auditadas periodicamente. Recomenda-se integrar o `Dependabot` ou `Renovate` no repositório para alertas automáticos de CVEs.

**Pipeline CI/CD:**
O workflow do GitHub Actions deve ser expandido com as seguintes etapas em sprints futuras:
- Execução de testes unitários e de integração
- Geração de relatório de cobertura
- Build e push de imagem Docker (se aplicável)
- Deploy automatizado em ambiente de homologação

**Conformidade com LGPD / dados sensíveis:**
Qualquer dado pessoal processado pelo sistema deve ser mapeado e ter sua finalidade declarada, em conformidade com a LGPD. Logs de auditoria devem ser anonimizados quando contiverem dados de cidadãos.

**Revisão humana de código gerado por IA:**
Todo código sugerido por ferramentas como GitHub Copilot deve passar por revisão manual antes do merge. O checklist mínimo de revisão deve incluir: ausência de hardcoded secrets, lógica de autorização correta, e ausência de injeção de dependências maliciosas (supply chain attack).
