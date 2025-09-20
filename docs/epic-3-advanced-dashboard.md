# Epic 3: EduDignity Advanced Features & Teacher Dashboard - Brownfield Enhancement

**Data de Criação:** 20/09/2025
**Épico:** 3 de 3
**Status:** Planejamento
**Dependência:** Epic 1 + Epic 2 (Infraestrutura + UX completos)
**Estimativa:** 2-3 sprints

---

## Epic Goal

Implementar recursos finais de concentração (música de fundo) e dashboard web completo para professores, finalizando ecossistema educacional EduDignity com visibilidade de progresso e ambiente otimizado para aprendizagem em vulnerabilidade socioeconômica.

## Epic Description

### Existing System Context

- **Current relevant functionality:** Sistema educacional core operacional + UX personalizada + coleta de dados granulares implementados (Epics 1-2)
- **Technology stack:** PWA completa funcionando + analytics robustos + sync bidirecional ativo + configurações personalizadas
- **Integration points:** Sistema de exercícios estabelecido, dados coletados e estruturados, backend de sincronização, web interface foundation

### Enhancement Details

- **What's being added/changed:** Sistema de música de fundo configurável para concentração + dashboard web completo para análise educacional por professores
- **How it integrates:** Extensão final do PWA com recursos de bem-estar + interface web separada consumindo dados educacionais via API
- **Success criteria:** Música não impacta performance, dashboard carrega < 5s em conexões escolares limitadas, relatórios exportáveis funcionais

---

## Stories Detalhadas

### Story 3.1: Sistema de Música de Fundo Configurável

**Como** criança que precisa de ambiente de concentração em lar com múltiplas distrações,
**Eu quero** música de fundo instrumental opcional e totalmente controlável,
**Para que** eu possa criar um ambiente que me ajude a focar nos estudos.

#### Acceptance Criteria Detalhados

**AC1 - Biblioteca de Música Instrumental**
- 12+ faixas instrumentais não-distratoras (piano, violão, ambient, nature sounds)
- Duração mínima 10 minutos por faixa para evitar loops frequentes
- Volume normalizado entre todas as faixas
- Fade in/out suave entre faixas
- Classificação por tipo: foco intenso, relaxamento, concentração leve

**AC2 - Controles Acessíveis Durante Exercícios**
- Player discreto fixo no canto inferior direito
- Controles: play/pause, próxima faixa, volume (0-100%)
- Ícone "sem som" para mute instantâneo
- Hotkeys: espaço (pause), setas (volume), M (mute)
- Não obstrui área de exercícios em nenhuma resolução

**AC3 - Continuidade Entre Exercícios**
- Música continua suavemente entre transições de exercícios
- Estado preservado durante pausas/retomadas de sessão
- Crossfade suave quando troca faixa
- Sem interrupção durante salvamento de progresso
- Resistente a falhas de rede (continua offline)

**AC4 - Compatibilidade de Hardware**
- Funciona com fones de ouvido básicos (R$ 10-30)
- Compatível com alto-falantes móveis de baixa qualidade
- Funciona em dispositivos Android 7+ e iOS 12+
- Não interfere com notificações do sistema
- Coexiste com outras apps de áudio instaladas

**AC5 - Configuração Personalizada Persistente**
- Preferência de faixa inicial salva por usuário
- Configuração de volume padrão
- Opção "sempre iniciar com música" ou "sempre silencioso"
- Sincronização de preferências entre dispositivos
- Recovery de configuração se dados corrompidos

#### Critérios Técnicos de Implementação

**Web Audio API Implementation:**
```javascript
// Estrutura básica do player
class EduDignityAudioPlayer {
  constructor() {
    this.audioContext = new AudioContext();
    this.gainNode = this.audioContext.createGain();
    this.currentTrack = null;
    this.playlist = [];
  }

  // Métodos: play, pause, setVolume, crossfade, etc.
}
```

**Estrutura de Dados:**
```sql
-- Extensão de configurações de usuário
ALTER TABLE users ADD COLUMN music_enabled BOOLEAN DEFAULT FALSE;
ALTER TABLE users ADD COLUMN music_volume INTEGER DEFAULT 30;
ALTER TABLE users ADD COLUMN preferred_track TEXT DEFAULT 'piano-calm';
```

**Audio File Optimization:**
- Formato: MP3 128kbps (balanço qualidade/tamanho)
- Compressão adicional para dispositivos limitados
- Lazy loading: carrega próxima faixa enquanto atual toca
- Cache inteligente: mantém 3 faixas em memória max

### Story 3.2: Dashboard Web para Professores

**Como** professor em escola pública com recursos tecnológicos limitados,
**Eu quero** visualizar progresso individual e da turma via navegador web simples,
**Para que** eu possa adaptar estratégias pedagógicas com dados reais e exportar relatórios para coordenação.

#### Acceptance Criteria Detalhados

**AC1 - Interface Web Responsiva e Acessível**
- Funciona em computadores escolares antigos (IE11+, Chrome 60+)
- Responsivo: desktop (1024px+), tablet (768px+), mobile fallback
- Tempo de carregamento < 5s mesmo com internet escolar limitada (1Mbps)
- Graceful degradation: funciona sem JavaScript para casos extremos
- Cores com contraste adequado para monitores antigos/mal calibrados

**AC2 - Visualização de Progresso Individual**
- **Dashboard por aluno:** Gráfico de progresso por competência (matemática/português)
- **Timeline de atividade:** Últimas 4 semanas com frequência de uso
- **Análise de dificuldades:** Top 3 tipos de erro com sugestões pedagógicas
- **Padrões de tempo:** Horários preferenciais de estudo, duração média de sessão
- **Indicadores de risco:** Alerta para crianças com > 7 dias sem acesso

**AC3 - Análise da Turma Completa**
- **Mapa de calor:** Competências vs. alunos mostrando padrões coletivos
- **Ranking de dificuldades:** Exercícios com maior taxa de erro na turma
- **Estatísticas de engajamento:** Tempo médio, frequência, taxa de conclusão
- **Comparativo temporal:** Progresso da turma mês a mês
- **Identificação de clusters:** Grupos de alunos com padrões similares

**AC4 - Relatórios Exportáveis para Gestão**
- **PDF detalhado:** Relatório mensal por aluno (2-3 páginas max)
- **CSV de dados:** Export raw para análises personalizadas pela escola
- **Relatório sintético:** 1 página por turma para reuniões pedagógicas
- **Gráficos embeddable:** Imagens PNG para apresentações
- **Conformidade LGPD:** Dados anonizados para compartilhamento externo

**AC5 - Sistema de Login e Gerenciamento**
- **Autenticação simples:** Email + senha, sem 2FA complexo
- **Gestão de múltiplas turmas:** Professor pode acessar várias turmas
- **Permissões granulares:** Coordenador vê todas as turmas, professor só as suas
- **Session management:** Auto-logout após 4h inatividade por segurança
- **Recovery de senha:** Via email, processo simples para professores

#### Interface Dashboard - Wireframe Funcional

**Layout Principal:**
```
+------------------+------------------+------------------+
| [Logo EduDignity]                   | Professor: Maria |
+------------------+------------------+------------------+
| Turma 4º A (28 alunos) ▼           | Período: Set 2025|
+------------------+------------------+------------------+
| RESUMO TURMA     | ALUNOS EM RISCO  | PROGRESSO GERAL  |
| 85% engajamento  | 3 sem acesso 7d+ | ↗ Matemática     |
| 23min sessão avg | 2 com dificuldade| ↘ Português      |
+------------------+------------------+------------------+
|           LISTA DE ALUNOS                             |
| [ ] Ana Silva    | ●●●○○ Matemática | Última: 18/09    |
| [ ] Bruno Costa  | ●●○○○ Português  | Última: 17/09    |
| [ ] ⚠ Carla Dias | ●○○○○ Ambos      | Última: 10/09    |
+------------------+------------------+------------------+
| [Exportar Relatório] [Ver Detalhes] [Configurações]  |
+------------------+------------------+------------------+
```

**Backend API Structure:**
```javascript
// Principais endpoints
GET /api/teachers/{id}/classes        // Lista de turmas
GET /api/classes/{id}/students        // Alunos da turma
GET /api/students/{id}/progress       // Progresso individual
GET /api/classes/{id}/analytics       // Analytics da turma
POST /api/reports/generate            // Gerar relatórios
```

**Database Views para Performance:**
```sql
-- View materializada para analytics rápidos
CREATE MATERIALIZED VIEW class_analytics AS
SELECT
  class_id,
  COUNT(DISTINCT user_id) as total_students,
  AVG(session_duration_minutes) as avg_session_time,
  COUNT(*) FILTER (WHERE last_access > NOW() - INTERVAL '7 days') as active_students,
  -- Mais métricas...
FROM detailed_responses
GROUP BY class_id;
```

---

## Compatibility Requirements

- [x] **Música compatível:** Fones básicos, alto-falantes móveis, dispositivos Android 7+/iOS 12+
- [x] **Dashboard funciona:** Computadores escolares antigos, browsers IE11+/Chrome 60+
- [x] **Audio não interfere:** Leitores de tela, outras apps, notificações sistema
- [x] **Interface web responsiva:** 1024px desktop, 768px tablet, 320px mobile fallback
- [x] **Performance maintained:** < 20MB memoria adicional para music, dashboard < 5s load

## Risk Mitigation

### Primary Risk
**Recursos de mídia causarem problemas de performance ou compatibilidade em hardware escolar limitado**

### Mitigation Strategy
1. **Codecs otimizados:** MP3 comprimido + fallbacks para formatos nativos
2. **Graceful degradation:** Dashboard funciona sem JavaScript, música opcional sempre
3. **Testes em hardware real:** 5+ computadores escolares antigos, 10+ dispositivos móveis básicos
4. **Performance monitoring:** Métricas contínuas de memory/CPU usage
5. **Kill switches:** Feature flags para desabilitar instantaneamente em caso de problemas

### Rollback Plan
1. **Música como módulo separado:** Pode ser desabilitada sem afetar exercícios
2. **Dashboard independente:** Interface web separada da PWA principal
3. **Fallback modes:** Versão texto-only do dashboard para emergências
4. **Data preservation:** Todos os dados educacionais preservados independente das features

## Definition of Done

### Funcional
- [x] Sistema de música funciona offline e não interfere com exercícios
- [x] Dashboard carrega rapidamente em conexões escolares limitadas
- [x] Relatórios exportáveis geram PDFs legíveis e úteis para professores
- [x] Login/logout seguro com gestão de múltiplas turmas

### Técnico
- [x] Performance: música < 20MB memory overhead, dashboard < 5s load time
- [x] Compatibilidade validada em 5+ computadores escolares antigos
- [x] Security review do sistema de autenticação de professores
- [x] LGPD compliance para dados exportados

### Qualidade
- [x] Usabilidade do dashboard validada com 3+ professores reais
- [x] Zero regressão nas funcionalidades dos Epics 1 e 2
- [x] Accessibility compliance mantida para todo o sistema
- [x] Música testada com fones/alto-falantes de diferentes qualidades

---

## Handoff para Story Manager

**Story Manager Handoff:**

"Por favor, desenvolva stories técnicas detalhadas para este épico final. Considerações críticas:

- **Sistema completo existente:** Epics 1-2 implementados (infraestrutura + exercícios + UX + analytics)
- **Pontos de integração:** Sistema de exercícios maduro, dados educacionais coletados, backend API estabelecido
- **Dois sistemas distintos:** PWA enhancement (música) + Web dashboard separado (professores)
- **Requisitos críticos:** Performance não pode degradar sistema educacional principal, compatibilidade com hardware escolar antigo
- **Cada story deve incluir:** Testes de compatibility em hardware limitado, validação de usabilidade com professores, verificação de non-regression

O épico deve finalizar o ecossistema EduDignity com recursos que aumentem concentração estudantil e forneçam insights educacionais acionáveis para professores."

---

## Resumo dos 3 Épicos Completos

### ✅ Epic 1: Fundação Técnica (2-3 sprints)
- Infraestrutura offline-first robusta
- Sistema core de exercícios anti-infantilizados
- Base para todos os outros épicos

### ✅ Epic 2: UX e Analytics (2-3 sprints)
- Personalização respeitosa (fontes, acessibilidade)
- Feedback contextual não-punitivo
- Coleta de dados educacionais LGPD-compliant

### ✅ Epic 3: Features Finais (2-3 sprints)
- Música de fundo para concentração
- Dashboard completo para professores
- Relatórios exportáveis para gestão escolar

**Estimativa Total: 6-9 sprints para sistema EduDignity completo**

---

**🤖 Generated with [Claude Code](https://claude.ai/code)**

**Co-Authored-By: Claude <noreply@anthropic.com>**