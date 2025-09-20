# Epic 2: EduDignity User Experience & Analytics - Brownfield Enhancement

**Data de Criação:** 20/09/2025
**Épico:** 2 de 3
**Status:** Planejamento
**Dependência:** Epic 1 (Infraestrutura Base)
**Estimativa:** 2-3 sprints

---

## Epic Goal

Implementar experiência de usuário personalizada e acessível com sistema robusto de coleta de dados educacionais, garantindo feedback respeitoso e insights acionáveis para educadores através de analytics granulares.

## Epic Description

### Existing System Context

- **Current relevant functionality:** Infraestrutura offline-first estabelecida + sistema de exercícios core funcionando (Epic 1)
- **Technology stack:** PWA funcionando + React/TypeScript + SQLite operacional + sincronização cloud ativa
- **Integration points:** Sistema de exercícios existente, banco de dados local estruturado, backend de sincronização, configurações de usuário

### Enhancement Details

- **What's being added/changed:** Sistema de personalização de fontes + feedback contextual progressivo não-punitivo + coleta granular de dados educacionais
- **How it integrates:** Extensão do sistema de exercícios com camadas de UX personalizada e analytics educacionais
- **Success criteria:** Configurações persistem offline/online, feedback validado como não-constrangedor, dados coletados sem impacto na performance

---

## Stories Detalhadas

### Story 2.1: Sistema de Configuração de Fontes e Acessibilidade

**Como** criança com dificuldades visuais específicas ou preferências de leitura,
**Eu quero** ajustar tamanho e tipo de fonte conforme minha necessidade individual,
**Para que** eu possa ler confortavelmente sem barreiras adicionais ao aprendizado.

#### Acceptance Criteria Detalhados

**AC1 - Opções de Tamanho de Fonte**
- 4 tamanhos disponíveis: Pequeno (14px), Médio (16px), Grande (20px), Extra-Grande (24px)
- Aplicação instantânea em todos os exercícios sem recarregamento de página
- Preview em tempo real durante seleção
- Otimização de layout responsivo para tamanhos maiores
- Teste em dispositivos pequenos (5" screen) para garantir usabilidade

**AC2 - Variedades de Fonte por Necessidade**
- **Sans-serif padrão:** Inter ou Roboto para legibilidade geral
- **Dyslexia-friendly:** OpenDyslexic ou similar com maior espaçamento entre caracteres
- **Alta legibilidade:** Atkinson Hyperlegible para baixa visão
- **Alto contraste:** Opção de weight bold automático para todas as fontes
- Fallbacks robustos para dispositivos que não suportam fontes customizadas

**AC3 - Persistência e Sincronização**
- Configurações salvas instantaneamente no SQLite local
- Sincronização entre dispositivos quando criança usar múltiplos equipamentos
- Backup de configurações incluído no export de dados
- Recovery automático se configurações corrompidas
- Isolamento por usuário em dispositivos compartilhados

**AC4 - Interface de Configuração Intuitiva**
- Acesso via ícone de configurações no canto superior direito
- Preview das mudanças com texto de exemplo dos exercícios
- Botão "Aplicar" e "Cancelar" claramente visíveis
- Configuração acessível mesmo para crianças com baixa literacia digital
- Help text discreto explicando benefícios de cada opção

**AC5 - Acessibilidade Avançada**
- Compatibilidade com leitores de tela (NVDA, JAWS)
- Navegação completa via teclado
- Contraste adequado (WCAG AA) para todas as combinações
- Zoom do navegador não quebra layout até 200%
- Text-to-speech nativo funciona corretamente

#### Critérios Técnicos de Implementação

**CSS Custom Properties:**
```css
:root {
  --font-size-base: var(--user-font-size, 16px);
  --font-family-primary: var(--user-font-family, 'Inter');
  --font-weight: var(--user-font-weight, 400);
}
```

**Estrutura de Dados:**
```sql
-- Extensão da tabela users
ALTER TABLE users ADD COLUMN font_size TEXT DEFAULT 'medium';
ALTER TABLE users ADD COLUMN font_family TEXT DEFAULT 'inter';
ALTER TABLE users ADD COLUMN accessibility_needs TEXT; -- JSON array
```

### Story 2.2: Coleta Detalhada de Dados por Questão

**Como** professor em escola pública,
**Eu quero** dados granulares sobre desempenho de cada criança por questão,
**Para que** eu possa identificar padrões específicos de dificuldade e adaptar estratégias pedagógicas.

#### Acceptance Criteria Detalhados

**AC1 - Métricas por Resposta Individual**
- **Tempo de resposta:** Início da questão até submissão (precisão centésimos)
- **Tentativas:** Número de modificações na resposta antes de submeter
- **Tipo de erro:** Classificação automática (conceitual, procedimental, distração)
- **Sequência de ações:** Ordem de interação com elementos da questão
- **Hesitação:** Detecção de pausas > 5s durante resposta

**AC2 - Contexto de Sessão**
- **Metadata temporal:** Horário início/fim, dia da semana, duração total
- **Ambiente técnico:** Dispositivo, resolução, tipo de conexão, interrupções detectadas
- **Padrões de pausa:** Frequência e duração de pausas durante exercícios
- **Sequência de exercícios:** Ordem de acesso, tempo entre exercícios
- **Estado de bateria:** Dispositivo móvel - correlação com performance

**AC3 - Anonização e Proteção de Dados (LGPD)**
- Hash irreversível de identificadores pessoais antes de storage
- Dados geolocalização removidos automaticamente
- Retenção limitada: dados comportamentais por 2 anos máximo
- Opt-out completo disponível para responsáveis
- Audit trail de acessos aos dados para compliance

**AC4 - Coleta Não-Invasiva**
- Zero impacto perceptível na responsividade da interface
- Coleta assíncrona com batch processing
- Fallback graceful se coleta falhar (exercícios continuam normais)
- Indicador visual discreto sobre coleta ativa (dot verde pequeno)
- Opção de desabilitar coleta mantendo funcionalidade completa

**AC5 - Otimização de Performance**
- Coleta local com sync em background
- Compressão de dados antes de storage
- Cleanup automático de dados antigos
- Memory usage da coleta < 20MB
- Não bloquear UI thread durante processamento

#### Estrutura de Dados Analytics

**Tabela de Responses Expandida:**
```sql
CREATE TABLE detailed_responses (
  id INTEGER PRIMARY KEY,
  user_hash TEXT, -- anonizado
  exercise_id TEXT,
  session_id TEXT,
  question_index INTEGER,
  response_data JSON,
  response_time_ms INTEGER,
  attempt_count INTEGER,
  error_type TEXT,
  action_sequence JSON,
  device_context JSON,
  timestamp INTEGER,
  sync_status TEXT DEFAULT 'pending'
);
```

**Índices de Performance:**
```sql
CREATE INDEX idx_responses_user_exercise ON detailed_responses(user_hash, exercise_id);
CREATE INDEX idx_responses_session ON detailed_responses(session_id);
CREATE INDEX idx_responses_sync ON detailed_responses(sync_status);
```

### Story 2.3: Sistema de Feedback Contextual Progressivo

**Como** criança em processo de aprendizagem com histórico de frustração educacional,
**Eu quero** feedback que me ajude a entender sem me envergonhar ou infantilizar,
**Para que** eu possa aprender com erros sem desenvolver aversão ao estudo.

#### Acceptance Criteria Detalhados

**AC1 - Feedback Imediato (0-1s)**
- Animação visual sutil confirmando resposta registrada
- Sem indicação de "certo/errado" imediata - apenas confirmação de esforço
- Micro-animação respeitosa (leve fade ou check discreto)
- Consistente independentemente da correção da resposta
- Zero som ou vibração para não perturbar ambiente doméstico

**AC2 - Feedback Explicativo (2-5s)**
- Dica contextual sobre processo de resolução, não sobre resultado
- Linguagem construtiva focada no "como pensar" sobre o problema
- Variação nas mensagens para evitar repetição mecânica
- Adaptação baseada no tipo de erro detectado (conceitual vs. procedimental)
- Opção de "mostrar processo" ao invés de "mostrar resposta"

**AC3 - Feedback de Competência (fim de bloco)**
- Celebração respeitosa do progresso demonstrado
- Foco em habilidades desenvolvidas, não em quantidade de acertos
- Mensagens personalizadas baseadas no padrão de evolução individual
- Reconhecimento de esforço e persistência, especialmente após erros
- Linguagem que reforça capacidade de crescimento ("Você está desenvolvendo...")

**AC4 - Zero Linguagem Infantilizada ou Punitiva**
- Proibição total de: "Muito bem!", "Ops!", "Tente novamente!", emojis
- Tom conversacional respeitoso como entre iguais
- Evitar superlativos e diminutivos
- Linguagem direta e informativa
- Teste com público-alvo para validar percepção de respeito

**AC5 - Personalização do Nível de Feedback**
- Opção "Feedback Mínimo" para crianças que preferem autonomia
- Opção "Feedback Detalhado" para crianças que querem mais orientação
- Configuração "Feedback Apenas de Progresso" eliminando feedback por questão
- Adaptação automática baseada no comportamento (se criança pula feedback constantemente)
- Preferências persistem e sincronizam entre dispositivos

#### Biblioteca de Mensagens Anti-Infantilizadas

**Feedback Explicativo - Matemática:**
```
"Dividir 24 por 6 significa descobrir quantos grupos de 6 cabem em 24"
"Reagrupamento: quando não há unidades suficientes, pegue uma dezena"
"Este problema tem dois passos: primeiro calcule [...], depois [...]"
```

**Feedback Explicativo - Português:**
```
"Esta palavra tem o som /ão/ no final, que pode ser escrito -ão, -am ou -an"
"Releia a primeira frase para encontrar a informação que responde à pergunta"
"O contexto da frase ajuda a descobrir o significado desta palavra"
```

**Feedback de Competência:**
```
"Você demonstrou compreensão consistente de adição com reagrupamento"
"Sua velocidade de leitura melhorou 15% mantendo a compreensão"
"Você resolveu problemas de dois passos com estratégia organizada"
```

---

## Compatibility Requirements

- [x] **Configurações sincronizam:** Entre dispositivos compartilhados sem conflitos de usuário
- [x] **Fontes renderizam:** Adequadamente em dispositivos Android básicos, iOS antigos
- [x] **Coleta não impacta:** Performance de exercícios < 5ms overhead por resposta
- [x] **Feedback instantâneo:** Carrega em < 1s visual, < 3s explicativo
- [x] **Armazenamento:** Dados analytics comprimidos < 1MB per user per month

## Risk Mitigation

### Primary Risk
**Coleta excessiva de dados impactar performance ou violar privacidade de menores**

### Mitigation Strategy
1. **Anonização rigorosa:** Hash irreversível + removal de PII antes de qualquer storage
2. **Coleta otimizada:** Batch processing, async operations, selective data points
3. **LGPD compliance:** Legal review + opt-out fácil + retenção limitada
4. **Performance monitoring:** Métricas contínuas de impact na UX
5. **Audit trail:** Log de todos os acessos para compliance e security

### Rollback Plan
1. **Feature flags:** Desabilitar coleta instantaneamente via configuração
2. **Data purge:** Script automático para remover dados se necessário
3. **Graceful degradation:** Sistema funciona 100% sem coleta ativa
4. **Export preservation:** Progresso educacional preservado independente de analytics

## Definition of Done

### Funcional
- [x] Configurações de fonte persistem offline e sincronizam online
- [x] Feedback validado como não-constrangedor por público-alvo 8-11 anos
- [x] Coleta de dados funciona sem impacto na responsividade
- [x] Sistema de personalização acessível via teclado e leitor de tela

### Técnico
- [x] Performance: overhead de coleta < 5ms per response
- [x] LGPD compliance validado por legal review
- [x] Testes automatizados para cenários de falha de sync
- [x] Monitoring de performance impact implementado

### Qualidade
- [x] Acessibilidade WCAG AA compliant
- [x] Teste de usabilidade com crianças confirma design respeitoso
- [x] Analytics schema validado por educadores
- [x] Zero regressão na funcionalidade de exercícios do Epic 1

---

## Handoff para Story Manager

**Story Manager Handoff:**

"Por favor, desenvolva stories técnicas detalhadas para este épico de UX. Considerações críticas:

- **Building on Epic 1:** Infraestrutura offline + exercícios core já estabelecidos
- **Pontos de integração:** Sistema de exercícios existente, banco SQLite, sync backend, configurações de usuário
- **Padrões a seguir:** Design anti-infantilização estabelecido no Epic 1
- **Requisitos críticos:** Performance não pode degradar, LGPD compliance obrigatório, acessibilidade total
- **Cada story deve incluir:** Testes de performance impact, validação de privacidade, verificação de tone respeitoso

O épico deve criar experiência personalizada respeitosa que gera insights educacionais valiosos sem constrangimento ou invasão de privacidade."

---

**🤖 Generated with [Claude Code](https://claude.ai/code)**

**Co-Authored-By: Claude <noreply@anthropic.com>**