# Epic 1: EduDignity Core Platform - Brownfield Enhancement

**Data de Criação:** 20/09/2025
**Épico:** 1 de 3
**Status:** Planejamento
**Estimativa:** 2-3 sprints

---

## Epic Goal

Estabelecer infraestrutura offline-first robusta e implementar sistema core de blocos educacionais anti-infantilizados, criando base técnica sólida para plataforma educacional em ambiente de vulnerabilidade socioeconômica.

## Epic Description

### Existing System Context

- **Current relevant functionality:** Documentação conceitual completa (brief, brainstorming, PRD) em docs/
- **Technology stack:** Sistema novo - Stack proposto PWA + React/TypeScript + SQLite + PostgreSQL + Service Workers
- **Integration points:** Sincronização cloud, armazenamento local persistente, interface responsiva multi-dispositivo

### Enhancement Details

- **What's being added/changed:** Infraestrutura offline-first completa + sistema de exercícios educacionais com design anti-infantilização
- **How it integrates:** Arquitetura offline-first com Service Workers para caching, SQLite como fonte primária, sincronização automática quando conectividade disponível
- **Success criteria:** Sistema funciona completamente offline por 24h+, exercícios carregam < 3s em 3G, design validado com público-alvo 8-11 anos

---

## Stories Detalhadas

### Story 1.1: Infraestrutura Base e Arquitetura Offline-First

**Como** desenvolvedor,
**Eu quero** estabelecer a infraestrutura base offline-first robusta,
**Para que** o sistema funcione efetivamente em ambientes com conectividade instável típicos do público socioeconômico vulnerável.

#### Acceptance Criteria Detalhados

**AC1 - Service Workers e Cache Inteligente**
- Service Worker registrado e ativo em todos os navegadores suportados
- Cache estratificado: recursos estáticos (CSS/JS), dados dinâmicos (exercícios), configurações de usuário
- Estratégia de cache: Cache First para recursos estáticos, Network First com fallback para dados dinâmicos
- Limpeza automática de cache quando armazenamento < 10% disponível

**AC2 - SQLite Local Robusta**
- Banco SQLite configurado com schema otimizado para dados educacionais
- Tabelas: users, exercises, responses, progress, sync_queue, settings
- Índices otimizados para queries de progresso e relatórios
- Backup automático local a cada 100 operações

**AC3 - Sincronização Bidirecional**
- Queue de sincronização para operações offline → online
- Conflict resolution: server wins para configurações, merge inteligente para progresso
- Retry exponencial com backoff para falhas de rede
- Indicador visual discreto de status de sincronização

**AC4 - Performance em Dispositivos Limitados**
- Tempo de carregamento inicial < 5s em dispositivo 2GB RAM
- Lazy loading de exercícios não utilizados
- Compressão de dados local com gzip
- Memory usage < 150MB durante uso normal

**AC5 - Offline Functionality Completa**
- Todos os exercícios acessíveis offline após primeiro carregamento
- Configurações persistem offline
- Progresso salvo instantaneamente local, sync quando possível
- Feedback visual claro sobre status offline/online

#### Critérios Técnicos de Implementação

**Tecnologias Específicas:**
- Service Worker API para cache management
- IndexedDB wrapper (Dexie.js) para SQLite-like operations
- Background Sync API para sincronização automática
- Web App Manifest para instalação PWA

**Arquitetura de Dados:**
```sql
-- Schema principal
CREATE TABLE users (id, name, school_id, grade_level, created_at);
CREATE TABLE exercises (id, subject, competency, difficulty, content_json);
CREATE TABLE responses (id, user_id, exercise_id, response_data, timestamp, session_id);
CREATE TABLE sync_queue (id, operation, table_name, record_id, data_json, created_at);
```

### Story 1.2: Sistema de Blocos Educacionais Matemática/Português

**Como** criança de 8-11 anos com defasagem educacional,
**Eu quero** acessar exercícios organizados por competência real (não série),
**Para que** eu possa progredir respeitosamente sem constrangimento da minha idade.

#### Acceptance Criteria Detalhados

**AC1 - Blocos de Matemática por Competência**
- **Numeramento:** Contagem, sequências, valor posicional (1-100, 1-1000)
- **Operações Básicas:** Adição/subtração com reagrupamento, multiplicação simples, divisão básica
- **Resolução de Problemas:** Problemas de 1-2 passos contextualizados
- Cada bloco: 10-15 exercícios com dificuldade progressiva
- Adaptação automática baseada em taxa de acerto (>80% = próximo nível)

**AC2 - Blocos de Português por Competência**
- **Decodificação:** Sílabas simples/complexas, palavras, frases curtas
- **Fluência:** Leitura cronometrada discreta (sem pressão visual), compreensão imediata
- **Compreensão:** Textos curtos com perguntas objetivas, inferência básica
- Vocabulário contextualizado sem definições abstratas
- Progressão: letra → sílaba → palavra → frase → texto

**AC3 - Interface Anti-Infantilizada**
- Paleta: tons neutros (azul-acinzentado, verde-oliva, bege)
- Zero personagens cartoon, mascotes ou animações "fofas"
- Tipografia: Inter ou similar, sem fontes decorativas
- Iconografia: símbolos geométricos abstratos, sem rostos sorridentes
- Layout: grid limpo similar a apps produtivos que adolescentes respeitam

**AC4 - Navegação Intuitiva**
- Mapa de competências visual mas não gamificado
- Indicadores de progresso discretos (barras simples, não estrelas/medalhas)
- Breadcrumb claro: Matemática > Operações Básicas > Adição com Reagrupamento
- Botão "Pausar" sempre visível para interrupções domésticas

**AC5 - Sessões Curtas com Pausa/Retomada**
- Sessões limitadas a 5-15 minutos com timer discreto
- Auto-pausa após 20 minutos de inatividade
- Retomada exata no exercício interrompido
- Histórico de sessões para automonitoramento

#### Critérios de Design Específicos

**Validação Anti-Infantilização:**
- Teste A/B com crianças 8-11 anos: design atual vs. versão "infantil"
- Métrica: tempo de engajamento, feedback qualitativo sobre "respeito"
- Benchmark: crianças devem preferir design "sério" por margem > 70%

**Organização de Conteúdo:**
```
/exercises
  /matematica
    /numeramento (10 exercícios adaptivos)
    /operacoes-basicas (15 exercícios progressivos)
    /resolucao-problemas (12 exercícios contextualizados)
  /portugues
    /decodificacao (18 exercícios silábicos)
    /fluencia (12 exercícios cronometrados)
    /compreensao (10 textos + questões)
```

---

## Compatibility Requirements

- [x] **Dispositivos Android básicos:** Funciona fluentemente em 2GB RAM, Android 7+
- [x] **Navegadores antigos:** Chrome 70+, Firefox 65+, Safari 12+ (iOS 12+)
- [x] **Conectividade limitada:** Funciona offline completo, sync com 3G instável
- [x] **Compartilhamento:** Múltiplos usuários no mesmo dispositivo sem conflitos
- [x] **Armazenamento:** < 500MB space total, com limpeza automática

## Risk Mitigation

### Primary Risk
**Falha na sincronização offline/online causando perda crítica de dados educacionais**

### Mitigation Strategy
1. **Backup redundante:** SQLite local + export automático para storage device
2. **Validação de integridade:** Checksums para dados críticos, detecção de corrupção
3. **Recovery automático:** Restauração de backup local se sync falhar
4. **Testes extensivos:** Simulação de falhas de rede, testes em dispositivos reais 2GB RAM
5. **Monitoring:** Logs detalhados de sync failures com alertas automáticos

### Rollback Plan
1. **Dados preservados:** SQLite local mantém todos os dados até sincronização bem-sucedida
2. **Versioning:** Control de versão granular permite rollback para versão anterior estável
3. **Export manual:** Funcionalidade de export de progresso como backup de emergência
4. **Degraded mode:** Sistema continua funcionando offline se sync permanentemente falhar

## Definition of Done

### Funcional
- [x] Sistema carrega e funciona 100% offline após primeira instalação
- [x] Exercícios de matemática e português acessíveis por competência
- [x] Interface validada como não-infantilizada por público-alvo
- [x] Sincronização bidirecional funcionando com conflict resolution

### Técnico
- [x] Performance < 3s carregamento em dispositivo 2GB + 3G
- [x] Testes automatizados cobrindo cenários de falha de rede
- [x] Code review focado em otimização para dispositivos limitados
- [x] Documentação técnica para próximos épicos

### Qualidade
- [x] Zero regressões em funcionalidade (sistema novo)
- [x] Compatibilidade validada em 5+ dispositivos Android básicos
- [x] Accessibility compliance para leitores de tela
- [x] LGPD compliance para dados de menores

---

## Handoff para Story Manager

**Story Manager Handoff:**

"Por favor, desenvolva stories técnicas detalhadas para este épico foundational. Considerações críticas:

- **Sistema completamente novo** com stack PWA + React + SQLite + PostgreSQL
- **Pontos de integração:** Service Workers para offline, IndexedDB para dados locais, sync API para backend
- **Padrões existentes:** Estabelecer padrões anti-infantilização que serão seguidos nos próximos épicos
- **Requisitos de compatibilidade:** Dispositivos 2GB RAM, conectividade 3G instável, navegadores antigos escolares
- **Cada story deve incluir:** Testes de funcionalidade offline, validação de performance, verificação de design anti-infantilização

O épico deve estabelecer base técnica sólida permitindo que Épicos 2 e 3 se concentrem em features educacionais avançadas."

---

**🤖 Generated with [Claude Code](https://claude.ai/code)**

**Co-Authored-By: Claude <noreply@anthropic.com>**