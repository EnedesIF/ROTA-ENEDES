# ENEDES · Metodologia 5D
> Sistema de Gestão de Consultoria Empresarial — ENEDES / IFB

**URL de Produção:** [empreendedor.enedesifb.com.br](https://empreendedor.enedesifb.com.br)  
**Repositório:** [github.com/enedesif/ROTA-ENEDES](https://github.com/enedesif/ROTA-ENEDES)  
**Infraestrutura:** Single-file HTML + Supabase (PostgreSQL) + GitHub Pages  
**Financiamento:** Convênio ABDI/FINATEC — ENEDES/IFB Campus Brasília

---

## Índice

1. [Visão Geral](#visão-geral)
2. [Arquitetura](#arquitetura)
3. [Perfis de Usuário](#perfis-de-usuário)
4. [Módulos do Sistema](#módulos-do-sistema)
5. [Banco de Dados — Supabase](#banco-de-dados--supabase)
6. [Gestão de Usuários](#gestão-de-usuários)
7. [Fase 1 — Diagnóstico 5D](#fase-1--diagnóstico-5d)
8. [Fase 2 — Programa 12 Meses](#fase-2--programa-12-meses)
9. [Integrações com IA](#integrações-com-ia)
10. [Deploy e Manutenção](#deploy-e-manutenção)
11. [Problemas Conhecidos e Soluções](#problemas-conhecidos-e-soluções)
12. [Histórico de Versões](#histórico-de-versões)

---

## Visão Geral

O **ENEDES 5D** é um sistema web de gestão de consultoria empresarial baseado na **Metodologia 5D** (Diagnosticar, Definir, Desenvolver, Direcionar, Desdobrar), desenvolvido para o programa **Rota Empreendedora** do IFB.

O sistema permite que consultores acompanhem empreendedores em duas fases estruturadas:
- **Fase 1:** Diagnóstico empresarial com os 5 pilares (Produto, Vendas, Finanças, Gestão, Processos)
- **Fase 2:** Programa de aceleração de 12 meses com plano de negócios completo

---

## Arquitetura

```
┌─────────────────────────────────────────────────────┐
│                  GitHub Pages                        │
│         empreendedor.enedesifb.com.br                │
│                  index.html                          │
│         (Single-file — HTML + CSS + JS)              │
└─────────────────────┬───────────────────────────────┘
                      │ REST API
┌─────────────────────▼───────────────────────────────┐
│                   Supabase                           │
│         Projeto: ROTA EMPREENDEDORA                  │
│    cvjqdryogehpjfdihcmm.supabase.co                  │
│                                                      │
│  Tabelas principais:                                 │
│  • atendimentos       • usuarios_5d                  │
│  • fase2v2            • (outras auxiliares)          │
└─────────────────────────────────────────────────────┘
```

**Stack tecnológica:**
- Frontend: HTML5 + CSS3 + JavaScript (vanilla, sem framework)
- Banco de dados: PostgreSQL via Supabase (REST API)
- Hospedagem: GitHub Pages + domínio customizado
- IA: API Anthropic (Claude Sonnet)
- Bibliotecas: Chart.js, html2pdf.js, XLSX.js

---

## Perfis de Usuário

| Perfil | Código | Descrição | Acesso |
|--------|--------|-----------|--------|
| Coordenador Geral | `coord` | Acesso total ao sistema | Todos os atendimentos, usuários, relatórios |
| Coordenador ABDI | `coord_abdi` | Coordenação institucional | Dashboard, relatórios, programa |
| Coordenador Programa | `coord_prog` | Gestão do programa 12M | Fase 2 de todos os empreendedores |
| Consultor | `consul` | Consultores da Rota | Seus próprios atendimentos + Fase 2 |
| Empreendedor | `emp` | Empreendedores atendidos | Apenas sua própria Fase 2 |

---

## Módulos do Sistema

### Dashboard
- Métricas gerais: total de atendimentos, consultores ativos, NPS médio
- Gráficos: segmentos atendidos, estágio dos negócios, desafios relatados
- Filtros: por consultor, campus, segmento, período

### Atendimentos (Fase 1)
- Lista de todos os atendimentos com busca e filtros
- Formulário completo de diagnóstico 5D
- Visualização em card com histórico

### Programa 12 Meses (Fase 2)
- Seletor de empreendedor (para coord/consul)
- 8 abas de conteúdo + relatório executivo

### Relatórios
- Exportação em Excel (atendimentos, pilares, programa)
- Exportação em PDF (plano de ação, relatório executivo)
- Relatório gerencial completo

### Usuários
- Cadastro e gestão de usuários
- Reset de senha
- Ativação/desativação

---

## Banco de Dados — Supabase

**Projeto:** ROTA EMPREENDEDORA  
**URL:** `https://cvjqdryogehpjfdihcmm.supabase.co`

### Tabela: `usuarios_5d`
Armazena os usuários do sistema.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `id` | int8 | Chave primária |
| `usuario` | text | Login (único) |
| `senha_hash` | text | Senha (texto simples — sem hash real) |
| `perfil` | text | `coord`, `coord_abdi`, `coord_prog`, `consul`, `emp` |
| `nome` | text | Nome completo |
| `email` | text | E-mail |
| `ativo` | boolean | Se o usuário está ativo |

### Tabela: `atendimentos`
Registros de atendimentos da Fase 1.

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `id` | uuid | Chave primária (gerado automaticamente) |
| `nome` | text | **Obrigatório.** Nome do empreendedor |
| `negocio` | text | Nome do negócio |
| `mentor` | text | Usuário do consultor responsável |
| `campus` | text | Campus do IFB |
| `segmento` | text | Segmento do negócio |
| `vinculado_programa` | boolean | Se está no Programa 12 Meses |
| `pilar_produto_nota` | text | Nota do pilar Produto (1-5) |
| `pilar_vendas_nota` | text | Nota do pilar Vendas (1-5) |
| `pilar_financas_nota` | text | Nota do pilar Finanças (1-5) |
| `pilar_gestao_nota` | text | Nota do pilar Gestão (1-5) |
| `acoes_json` | text | JSON com plano de ação |
| `nps` | text | NPS do atendimento (0-10) |
| `saved_at` | text | Data/hora do último save |

### Tabela: `fase2v2`
Dados completos do Programa 12 Meses por empreendedor.

| Grupo | Colunas | Descrição |
|-------|---------|-----------|
| Identificação | `emp_nome`, `saved_at` | Chave de busca e timestamp |
| Negócio | `negocio_nome`, `cnpj`, `fundacao`, `segmento`, `modelo`, `local`, `colaboradores`, `mercado`, `missao`, `visao`, `valores`, `proposito`, `meta_smart` | Identidade e missão |
| Financeiro | `cap_social`, `cap_integralizado`, `cap_giro`, `investimento`, `reserva`, `ticket`, `clientes`, `receita_calc`, `cfixos`, `cvariaveis`, `total_custos`, `margem` | Saúde financeira |
| Persona | `p_nome`, `p_idade`, `p_genero`, `p_escolaridade`, `p_ocupacao`, `p_renda`, `p_compra`, `p_pagamento`, `p_online`, `p_objetivos`, `p_desafios`, `p_motivacoes`, `p_objecoes` | Buyer Persona |
| 4Ps / 4Cs | `p4_produto`, `p4_preco`, `p4_praca`, `p4_promocao`, `c4_cliente`, `c4_custo`, `c4_conv`, `c4_comunic` | Mix de marketing |
| SWOT | `sw_forcas`, `sw_fraquezas`, `sw_oportunidades`, `sw_ameacas` | Análise SWOT |
| Estratégias SWOT IA | `sw_est_of`, `sw_est_mel`, `sw_est_def`, `sw_est_sob`, `sw_est_ia` | Estratégias geradas pela IA |
| Marketing | `mkt_m1` … `mkt_m12` | Plano de marketing mensal (jsonb) |
| Ações | `acoes` | Plano de ação (jsonb array) |
| Sessões | `sessoes` | Farol 12 meses (jsonb array) |
| Custos | `custos_fixos`, `custos_variaveis` | Detalhamento de custos (jsonb) |
| Outros | `p_redes`, `p_ia_resultado`, `sw_est_ia` | Campos auxiliares |
| Timestamps | `created_at`, `updated_at` | Controle de versão |

> **Importante:** A busca na `fase2v2` é feita pelo campo `emp_nome`, que deve ser **idêntico** ao campo `nome` na tabela `atendimentos` e ao campo `nome` na tabela `usuarios_5d`.

---

## Gestão de Usuários

### Criar novo usuário
```sql
INSERT INTO usuarios_5d (usuario, senha_hash, perfil, nome, email, ativo)
VALUES ('novouser', 'senha123', 'consul', 'Nome Completo', 'email@dominio.com', true);
```

### Resetar senha
```sql
UPDATE usuarios_5d 
SET senha_hash = 'novasenha'
WHERE usuario = 'nomeuser';
```

### Ativar/desativar usuário
```sql
UPDATE usuarios_5d SET ativo = true WHERE usuario = 'nomeuser';
UPDATE usuarios_5d SET ativo = false WHERE usuario = 'nomeuser';
```

### Criar atendimento mínimo para consultor acessar Fase 2
```sql
INSERT INTO atendimentos (nome, negocio, segmento, vinculado_programa, mentor, campus)
VALUES ('Nome do Empreendedor', 'Nome do Negócio', 'Segmento', true, 'usuarioconsultor', 'Brasília');
```

> **Atenção:** O campo `nome` deve ser **exatamente igual** ao `emp_nome` na tabela `fase2v2`.

---

## Fase 1 — Diagnóstico 5D

O diagnóstico avalia o negócio em **5 pilares** com notas de 1 a 5:

| Pilar | Foco |
|-------|------|
| Produto/Serviço | Proposta de valor, diferencial, MVP |
| Vendas/Marketing | Canais, processo de venda, carteira |
| Finanças | Fluxo de caixa, precificação, capital |
| Gestão/Processos | Operação, time, processos internos |
| *(campo livre)* | Desafio específico do negócio |

**Ferramentas adicionais:**
- Matriz GUT (Gravidade × Urgência × Tendência)
- Análise de 5 Porquês
- Meta SMART
- Plano de ação (até 3 ações imediatas)
- NPS e encaminhamentos

---

## Fase 2 — Programa 12 Meses

### Abas disponíveis

| # | Aba | Conteúdo |
|---|-----|----------|
| 1 | Negócio | Identidade, missão, visão, valores, meta SMART |
| 2 | Financeiro | Capital, investimento, receita, custos, margem |
| 3 | Persona | Buyer persona com análise de IA |
| 4 | 4Ps + 4Cs | Mix de marketing e perspectiva do cliente |
| 5 | SWOT | Análise SWOT + geração de estratégias com IA |
| 6 | Plano de Marketing | Planejamento mensal de ações de marketing |
| 7 | Plano de Ação | Ações vinculadas às estratégias SWOT |
| 8 | Farol | 12 sessões de acompanhamento mensal |
| 9 | Relatório | Resumo executivo completo (exportável em PDF) |

### Fluxo de acesso por perfil

```
Coordenador/Consultor:
  Login → Dashboard → Atendimentos → Seleciona empreendedor → Fase 2

Empreendedor (perfil emp):
  Login → Fase 2 (carrega automaticamente seus dados)

Consultor com atendimento vinculado:
  Login → selRec preenchido automaticamente (mentor = usuario) → Fase 2
```

### Importação de ações

**"Importar do Diagnóstico 5D"** — importa as ações do plano da Fase 1 (atendimento vinculado).

**"Importar Estratégias SWOT"** — converte as estratégias geradas pela IA em ações individuais no Plano de Ação, com pilar identificado (Ofensiva, Defensiva, Confrontativa, Reforço).

---

## Integrações com IA

O sistema utiliza a **API Anthropic (Claude Sonnet)** para:

### 1. Geração de Estratégias SWOT
Localização: Aba SWOT → "Gerar Estratégias com IA"

Gera 4 blocos de estratégias cruzadas:
- **Ofensiva:** Forças × Oportunidades
- **Defensiva:** Forças × Ameaças
- **Confrontativa:** Fraquezas × Oportunidades
- **Reforço:** Fraquezas × Ameaças

Retorno em formato JSON array — cada estratégia salva como linha separada.

### 2. Análise de Persona com IA
Localização: Aba Persona → "Analisar Persona com IA"

Gera insights sobre o perfil do cliente ideal com base nos dados preenchidos.

> **Configuração:** A chave da API Anthropic é inserida pelo usuário no campo 🔑 no topo da página. Não é armazenada no banco.

---

## Deploy e Manutenção

### Atualizar o sistema
1. Editar o arquivo `index.html` localmente
2. Fazer commit no repositório GitHub
3. GitHub Pages atualiza automaticamente em ~3 minutos

### Estrutura do repositório
```
ROTA-ENEDES/
├── index.html          # Sistema completo (single-file)
├── README.md           # Esta documentação
└── CHANGELOG.md        # Histórico de versões
```

### Variáveis de configuração (no início do index.html)
```javascript
const SU = 'https://cvjqdryogehpjfdihcmm.supabase.co';  // URL Supabase
const SB_KEY = '...';                                     // Chave pública Supabase
```

### Backup do banco
Para exportar todos os dados da Fase 2:
```sql
SELECT * FROM fase2v2 ORDER BY id;
-- Exportar via Supabase Dashboard → Export CSV
```

---

## Problemas Conhecidos e Soluções

### Fase 2 não carrega para consultor
**Causa:** `selRec` é null — não há atendimento vinculado ao consultor.  
**Solução:** Criar registro na tabela `atendimentos` com `mentor = usuario_do_consultor` e `vinculado_programa = true`.

### Estratégias SWOT somem ao reentrar
**Causa:** Campo `sw_est_ia` não estava sendo salvo (não estava no `COLS_VALIDAS`).  
**Solução:** Corrigido na versão atual. Para registros antigos, rodar:
```sql
UPDATE fase2v2 SET sw_est_ia = true WHERE emp_nome = 'Nome do Empreendedor';
```

### Dados da Fase 2 não sincronizam entre dispositivos
**Causa:** Cache local (`FASE2_CACHE`) em memória não persiste entre sessões.  
**Solução:** O sistema sempre busca do banco ao abrir. Se persistir, limpar cache no console:
```javascript
localStorage.clear(); location.reload();
```

### Nome do empreendedor não encontra dados na fase2v2
**Causa:** O `emp_nome` na `fase2v2` não é idêntico ao `nome` no `atendimentos` ou `usuarios_5d`.  
**Solução:**
```sql
UPDATE fase2v2 
SET emp_nome = 'Nome Exato Completo'
WHERE emp_nome = 'Nome Incorreto';
```

### Erro 400 ao salvar dados
**Causa:** Coluna não existe na tabela ou tipo incompatível.  
**Solução:** Verificar se a coluna está na lista `COLS_VALIDAS` no código e se existe na tabela `fase2v2`.

---

## Histórico de Versões

### v3.0 — Abril 2026
- Migração da tabela `fase2_dados` para `fase2v2`
- Correção do `selRec` automático para consultores
- Adição do botão "Importar Estratégias SWOT" no Plano de Ação
- Estratégias SWOT geradas como itens separados (array JSON)
- Correção da persistência do `sw_est_ia`
- Labels corrigidos: Confrontativa e Reforço
- Correção do `emp_nome` para Luiz Felipe Neves Barboza

### v2.0 — Março 2026
- Implementação completa da Fase 2 (Programa 12 Meses)
- Integração com API Anthropic para geração de estratégias SWOT
- Análise de Persona com IA
- Farol de 12 sessões de acompanhamento
- Relatório executivo com exportação PDF
- Dashboard com gráficos e métricas

### v1.0 — 2025
- Sistema base de atendimentos (Fase 1)
- Diagnóstico 5D com 5 pilares
- Dashboard gerencial
- Exportação Excel/PDF
- Gestão de usuários e perfis

---

## Contato e Suporte

**Coordenação:** Profa. Dra. Carla Simone Castro da Silva  
**Unidade:** ENEDES — Escola de Negócios e Desenvolvimento Social  
**Instituição:** Instituto Federal de Brasília — Campus Brasília  
**Programa:** Rota Empreendedora (Convênio ABDI/FINATEC)
