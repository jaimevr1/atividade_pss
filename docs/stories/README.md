# User Stories - Dimidui MVP

Este diretório contém as user stories organizadas por épico e módulo para implementação sistemática do Dimidui.

## Estrutura Organizacional

### 📁 **Epic-Based Stories** (Core Systems)
Stories que implementam a infraestrutura central de cada épico:

#### [epic-1/](./epic-1/) - Sistema de Módulos Educacionais
- **[US-001](./epic-1/us-001-sistema-runtime-modulos.md)** - Sistema Runtime de Módulos
- **[US-002](./epic-1/us-002-sistema-atividades-compostas.md)** - Sistema de Atividades Compostas
- **[US-003](./epic-1/us-003-feedback-formativo.md)** - Sistema de Feedback Formativo

#### [epic-2/](./epic-2/) - Sistema do Personagem Jeremias
- **[US-004](./epic-2/us-004-sistema-estado-personagem.md)** - Sistema de Estado do Personagem

#### [epic-3/](./epic-3/) - Plataforma Base e Dashboard
- **[US-005](./epic-3/us-005-autenticacao-escolar.md)** - Sistema de Autenticação Escolar
- **[US-006](./epic-3/us-006-dashboard-aluno.md)** - Dashboard do Aluno
- **[US-007](./epic-3/us-007-progressao-conceitual.md)** - Sistema de Progressão Conceitual

#### [epic-4/](./epic-4/) - Sistema Visual e Experiência
- **[US-008](./epic-4/us-008-design-system.md)** - Sistema de Design e Tokens

#### [epic-5/](./epic-5/) - Editor de Atividades (Professor)
- **[US-009](./epic-5/us-009-editor-atividades.md)** - Editor de Atividades para Professor
- **[US-010](./epic-5/us-010-dashboard-professor.md)** - Dashboard do Professor

### 📁 **Module-Based Stories** (Individual Implementation)
Stories específicas para implementação individual de cada módulo:

#### [modules/](./modules/) - Módulos Educacionais
- **[US-M1](./modules/us-m1-bateria-rapida.md)** - Módulo BateriaRápida
- **[US-M5](./modules/us-m5-identifica-operacao.md)** - Módulo IdentificaOperação
- **[US-M10](./modules/us-m10-jeremias-resolver.md)** - Módulo JeremiasResolver
- **[US-M14](./modules/us-m14-auto-avaliacao.md)** - Módulo AutoAvaliação
- **[US-M20](./modules/us-m20-estado-jeremias.md)** - Módulo EstadoJeremias

## Estratégia de Implementação

### Fase 1: Core Systems (Épicos)
**Prioridade:** Implementar primeiro as stories dos épicos para estabelecer infraestrutura base

```
1. Sistema Runtime de Módulos (US-001) ← BLOQUEANTE CRÍTICO
2. Sistema de Atividades Compostas (US-002)
3. Sistema de Feedback Formativo (US-003)
4. Sistema de Estado do Personagem (US-004)
```

### Fase 2: Individual Modules
**Prioridade:** Implementar módulos individuais paralelamente após core estar estável

```
Paralelo A: US-M1 (BateriaRápida) + US-M5 (IdentificaOperação)
Paralelo B: US-M14 (AutoAvaliação) + US-M20 (EstadoJeremias)
Sequencial: US-M10 (JeremiasResolver) ← Mais complexo, após outros
```

## Estimativas e Dependencies

### Epic Stories (Core)
| Story | Estimativa | Tipo | Bloqueia |
|-------|------------|------|----------|
| US-001 | 5 pts | Infrastructure | Todos os módulos |
| US-002 | 3 pts | Core System | Atividades compostas |
| US-003 | 4 pts | Core Pedagogy | Feedback em módulos |
| US-004 | 4 pts | Character System | Módulos Jeremias |
| US-005 | 2 pts | Authentication | Dashboard acesso |
| US-006 | 4 pts | Student Interface | Experiência aluno |
| US-007 | 5 pts | Academic Core | Progressão BNCC |
| US-008 | 3 pts | Visual Foundation | Interface consistente |
| US-009 | 5 pts | Teacher Tools | Criação atividades |
| US-010 | 4 pts | Teacher Analytics | Insights pedagógicos |

**Total Core:** 39 Story Points

### Module Stories (Individual)
| Story | Estimativa | Tipo | Dependencies |
|-------|------------|------|--------------|
| US-M1 | 5 pts | Educational | US-001, US-003 |
| US-M5 | 3 pts | Educational | US-001, US-003 |
| US-M14 | 2 pts | Educational | US-001, US-003 |
| US-M20 | 3 pts | Character | US-001, US-004 |
| US-M10 | 8 pts | Teaching | US-001, US-003, US-004 |

**Total Modules:** 21 Story Points

**TOTAL MVP:** 60 Story Points

## Critérios de Aceitação Globais

### Técnicos
- [ ] **Performance**: Carregamento <3s, interações <100ms
- [ ] **Responsividade**: Desktop 1024px+ otimizado
- [ ] **Testes**: 90%+ cobertura unitária, integração end-to-end
- [ ] **Acessibilidade**: WCAG AA compliance

### Pedagógicos
- [ ] **BNCC**: Alinhamento com habilidades prioritárias
- [ ] **Feedback**: Formativo, não punitivo (Princípio P2)
- [ ] **Idade**: Apropriado para 9-12 anos
- [ ] **Progresso**: Transparente e motivador

### Experiência
- [ ] **Design System**: Consistente com PRD
- [ ] **Animações**: Sutis e propositadas
- [ ] **Acessibilidade**: Fontes, temas, navegação
- [ ] **Economia Atenção**: Elementos decorativos mínimos

## Fluxos de Atividades MVP

### Fluência (20min)
```
US-M20 (EstadoJeremias) → US-M1 (BateriaRápida) → US-M14 (AutoAvaliação)
```

### Integração (20min)
```
US-M20 (EstadoJeremias) → US-M5 (IdentificaOperação) →
US-M10 (JeremiasResolver) → US-M14 (AutoAvaliação)
```

## Definição de Pronto (Epic Level)

Para considerar um épico implementado:

- [ ] Todas as core stories do épico completas
- [ ] Módulos relacionados integrados e funcionais
- [ ] Testes automatizados passando
- [ ] Performance validada em ambiente real
- [ ] Acessibilidade validada
- [ ] User testing com público-alvo realizado
- [ ] Documentação técnica completa

## Próximos Passos

### Imediato (Semana 1-2)
1. **Setup de Desenvolvimento**
   - Repository, CI/CD, environment setup
   - Design system base (Tailwind + tokens)
   - Supabase configuração

2. **Core crítico**
   - US-001: Sistema Runtime de Módulos
   - Foundational architecture decisions

### Médio Prazo (Semana 3-5)
1. **Core Systems**
   - US-002, US-003, US-004 implementation
   - Integration testing framework

2. **Module Development**
   - Parallel implementation de US-M1, US-M5, US-M14, US-M20

### Final (Semana 6-8)
1. **Advanced Features**
   - US-M10: JeremiasResolver (módulo mais complexo)
   - End-to-end integration testing

2. **Polish & Launch**
   - User testing iterations
   - Performance optimization
   - Teacher onboarding preparation

---

## Observações Importantes

### 🚨 Dependency Critical Path
**US-001 (Sistema Runtime)** é absolutely critical - nenhum módulo pode ser implementado sem este sistema estar funcional.

### 🎯 MVP Focus
Estas stories representam **apenas o MVP**. Features como editor visual, analytics avançado, e módulos adicionais estão em versões futuras.

### 📊 Measurement
Cada story inclui métricas específicas de sucesso pedagógico e técnico para validação contínua.