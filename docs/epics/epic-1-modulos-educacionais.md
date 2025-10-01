# Epic 1: Sistema de Módulos Educacionais

**ID:** EPIC-001
**Status:** Planejado
**Prioridade:** Must Have (MVP)
**Estimativa:** 3 semanas

## Objetivo

Implementar a biblioteca de módulos reutilizáveis que formam o core das atividades educacionais do Dimidui.

## Valor de Negócio

Permite ao professor criar atividades personalizadas combinando módulos, seguindo o princípio de "Modularidade Radical" definido na PRD. Esta funcionalidade é fundamental para diferenciação pedagógica e personalização do ensino.

## User Stories Incluídas

- **US-001:** M1 - BateriaRápida (fluência matemática)
- **US-002:** M5 - IdentificaOperação (interpretação de problemas)
- **US-003:** M14 - AutoAvaliação (metacognição)

## Critérios de Aceitação

- [ ] Sistema de configuração JSON para cada módulo implementado
- [ ] Runtime que renderiza módulos a partir da configuração
- [ ] Integração com sistema de progressão conceitual (thresholds BNCC)
- [ ] Feedback imediato e formativo sem elementos punitivos
- [ ] Coleta de dados de interação para analytics do professor
- [ ] Suporte às 10 habilidades BNCC prioritárias
- [ ] Integração com banco Supabase para persistência

## Dependências

- **Pré-requisitos:** Sistema de banco de dados, autenticação básica
- **Bloqueia:** Epic 2 (Sistema Jeremias), Epic 5 (Editor Professor)

## Definição de Pronto

- [ ] Todos os 3 módulos MVP implementados e testados
- [ ] Documentação técnica de como criar novos módulos
- [ ] Testes automatizados para cada módulo
- [ ] Integração completa com fluxo de atividades
- [ ] Performance: carregamento de módulo <2s
- [ ] Acessibilidade: suporte a leitores de tela e navegação por teclado

## Riscos

| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| Complexidade do sistema JSON | Média | Alto | Prototipagem inicial, validação com professor |
| Performance com múltiplos módulos | Baixa | Médio | Lazy loading, otimização React |
| Integração com Supabase | Baixa | Alto | Testes de integração contínuos |

## Notas Técnicas

- Framework: React 18 + Tailwind CSS
- Estado: React Context API
- Animações: Framer Motion (transições sutis)
- Dados: Supabase PostgreSQL + Real-time
- Tipografia: Atkinson Hyperlegible + toggle OpenDyslexic