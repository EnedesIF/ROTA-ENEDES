# CHANGELOG — ENEDES · Metodologia 5D

Todas as mudanças significativas do sistema são documentadas aqui.

---

## [3.0] — 07/04/2026

### Adicionado
- Botão **"🤖 Importar Estratégias SWOT"** no Plano de Ação
- Cada estratégia SWOT importada como ação individual separada
- Seleção automática do `selRec` para consultores ao logar (busca por `mentor = USER_NAME`)
- Campo `sw_est_ia` adicionado ao `COLS_VALIDAS` — estratégias agora persistem no banco

### Corrigido
- Estratégias SWOT sumiam ao reentrar no sistema (`sw_est_ia` não era salvo)
- Fase 2 não carregava para consultor sem atendimento vinculado
- `emp_nome` do Luiz Felipe Neves Barboza corrigido na `fase2v2`
- Labels dos quadrantes SWOT: "Melhoria" → "Confrontativa", "Sobrevivência" → "Reforço"
- Prompt da IA corrigido para retornar estratégias como array JSON (itens separados)

### Migração de banco
- Tabela `fase2_dados` → `fase2v2` (estrutura corrigida com tipos corretos)
- Registro de atendimento criado para Luiz Felipe Neves Barboza
- `sw_est_ia = true` setado para empreendedores com estratégias já geradas

---

## [2.5] — Março/Abril 2026

### Adicionado
- Separação de projetos Supabase: GESTAO e ROTA em projetos distintos
- Subdomínio `empreendedor.enedesifb.com.br` configurado
- Integração com API Anthropic para análise de Persona com IA
- Exportação do Relatório Executivo em PDF

### Corrigido
- Erros de cache do PostgREST após recriação de tabelas
- Políticas RLS na `fase2v2`
- Login não carregava após migração de tabelas

---

## [2.0] — Março 2026

### Adicionado
- **Fase 2 completa** — Programa 12 Meses com 8 módulos:
  - Negócio (missão, visão, valores, meta SMART)
  - Financeiro (capital, receita, custos, margem)
  - Persona (buyer persona + IA)
  - 4Ps + 4Cs (mix de marketing)
  - SWOT (análise + geração de estratégias com IA)
  - Plano de Marketing (12 meses)
  - Plano de Ação (vinculado ao SWOT)
  - Farol (12 sessões de acompanhamento)
- Relatório Executivo completo com exportação PDF
- Integração com API Anthropic (Claude Sonnet) para geração de estratégias SWOT
- Importação de ações do Diagnóstico 5D para o Plano de Ação da Fase 2
- Gráficos de evolução dos pilares no relatório

---

## [1.5] — 2025

### Adicionado
- Dashboard com métricas e gráficos (Chart.js)
- Exportação em Excel (atendimentos, pilares, programa)
- Filtros avançados no dashboard (por consultor, campus, segmento, período)
- Relatório gerencial em PDF
- Perfis adicionais: `coord_abdi`, `coord_prog`

### Corrigido
- Sincronização de dados entre dispositivos
- Problemas de cache localStorage

---

## [1.0] — 2025

### Lançamento inicial
- Sistema de atendimentos (Fase 1 — Diagnóstico 5D)
- 5 pilares de avaliação (Produto, Vendas, Finanças, Gestão, Processos)
- Ferramentas: Matriz GUT, 5 Porquês, Meta SMART, Plano de Ação
- NPS e encaminhamentos
- Gestão de usuários com 3 perfis: `coord`, `consul`, `emp`
- Autenticação simples via Supabase
- Hospedagem GitHub Pages
