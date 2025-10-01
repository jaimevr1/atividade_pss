# Epic 3: Plataforma Base e Dashboard

**ID:** EPIC-003
**Status:** Planejado
**Prioridade:** Must Have (MVP)
**Estimativa:** 2 semanas

## Objetivo

Criar a infraestrutura base do sistema incluindo autenticação simplificada, dashboards e gestão de progresso para ambiente escolar supervisionado.

## Valor de Negócio

Viabiliza o uso supervisionado em laboratório escolar (5 computadores desktop) e fornece visibilidade clara de progresso tanto para alunos quanto professores, essencial para o modelo pedagógico proposto.

## User Stories Incluídas

- **US-006:** Sistema de login simplificado (seleção de nome)
- **US-007:** Dashboard do aluno com progresso visual
- **US-008:** Sistema de progressão conceitual BNCC
- **US-009:** Persistência de dados no Supabase
- **US-010:** Preferências de acessibilidade (fonte/tema)

## Critérios de Aceitação

### Autenticação
- [ ] Login sem senha para ambiente escolar controlado
- [ ] Lista de nomes pré-cadastrados para seleção
- [ ] Sessão persistente durante uso escolar
- [ ] Logout manual disponível

### Dashboard Aluno
- [ ] Visão clara de progresso conceitual (X/10 dominados)
- [ ] Atividade da semana destacada
- [ ] Estado atual do Jeremias visível
- [ ] Histórico de atividades completadas
- [ ] Tempo estimado para próxima atividade

### Sistema de Progressão
- [ ] Mapeamento das 10 habilidades BNCC prioritárias
- [ ] Threshold de domínio (80% acerto em 15+ tentativas)
- [ ] Desbloqueio de atividades baseado em pré-requisitos
- [ ] Grafo de dependências conceituais

### Acessibilidade
- [ ] Toggle Atkinson Hyperlegible ↔ OpenDyslexic
- [ ] Toggle tema claro ↔ escuro
- [ ] Navegação por teclado
- [ ] Contraste adequado (WCAG AA)

## Modelo de Dados (Supabase)

```sql
-- Tabelas principais
students (id, name, grade_level, preferences)
activities (id, name, type, modules, bncc_codes)
student_progress (student_id, activity_id, status, interactions)
concept_mastery (student_id, bncc_code, mastery_level)
jeremias_state (student_id, maturity_level, concepts_taught)
```

## Layout Dashboard (Desktop 1024px+)

```
┌─────────────────────────────────────────┐
│ Header: "Olá, [Nome]" | ⚙️ Settings     │
├─────────────────────────────────────────┤
│ 📊 MEU PROGRESSO                        │
│ ├─ Conceitos dominados: 7/10           │
│ └─ Atividades completadas: 12          │
│                                         │
│ 🎯 ATIVIDADE DESTA SEMANA               │
│ ┌───────────────────────────────────┐   │
│ │ [Atividade] | [Tipo] | [Duração] │   │
│ │ [Começar] ──────────────────►     │   │
│ └───────────────────────────────────┘   │
│                                         │
│ 🤝 JEREMIAS                             │
│ ├─ Nível: X | "Diálogo atual"         │
│                                         │
│ 📚 ATIVIDADES ANTERIORES                │
│ └─ [Lista histórico]                    │
└─────────────────────────────────────────┘
```

## Dependências

- **Pré-requisitos:** Configuração Supabase projeto
- **Bloqueia:** Todos os outros épicos

## Definição de Pronto

- [ ] Login funcional em ambiente de laboratório
- [ ] Dashboard responsivo (1024px+)
- [ ] Sistema de progressão BNCC implementado
- [ ] Preferências persistidas corretamente
- [ ] Performance: carregamento dashboard <3s
- [ ] Backup automático a cada 2min
- [ ] Zero perda de dados em testes

## Riscos

| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| Complexidade modelo de dados | Baixa | Alto | Revisão com DBA, prototipagem |
| Performance com múltiplos usuários | Média | Médio | Testes de carga, otimização queries |
| Downtime Supabase | Baixa | Médio | Plano backup em papel |

## Métricas de Sucesso

- **Técnica:** 0 perda de dados, <3s carregamento
- **Usabilidade:** Professor consegue ver progresso turma <5min
- **Adoção:** >90% alunos fazem login independentemente