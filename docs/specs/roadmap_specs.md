# Dimidui - Roadmap e Evolução do Produto

**Visão:** Plataforma modular de prática educacional escalável e adaptativa  
**Horizonte:** 12 meses

---

## VISÃO GERAL DAS FASES

```
MVP (Semanas 1-4)
  ↓
Consolidação (Semanas 5-8)
  ↓
Expansão (Semanas 9-16)
  ↓
Otimização (Semanas 17-24)
  ↓
Escala (Semanas 25-52)
```

---

## FASE 1: MVP (Semanas 1-4) ✅

**Status:** EM DESENVOLVIMENTO

### Objetivos
- Provar conceito com grupo pequeno (5-12 alunos)
- Validar mecânicas principais (fluência + integração + Jeremias)
- Iterar baseado em feedback direto

### Features Entregues
- [x] Sistema de login (seleção de nome)
- [x] Dashboard aluno básico
- [x] 5 módulos essenciais:
  - M01 - BateriaRápida
  - M05 - IdentificaOperação
  - M10 - JeremiasResolver
  - M14 - AutoAvaliação
  - M20 - EstadoJeremias
- [x] ActivityRuntime (engine de módulos)
- [x] Salvar/carregar progresso
- [x] Sistema de evolução do Jeremias (5 níveis)
- [x] Rastreamento de concept_mastery
- [x] Editor de atividades (formulário básico)

### Métricas de Sucesso
- ✅ 80%+ alunos completam atividade semanal
- ✅ Tempo médio < 25 min
- ✅ Zero bugs críticos (perda de dados)
- ✅ Professor cria 1 atividade/semana sem ajuda

### Decisões Tomadas
- ✓ Apenas Jeremias (não Tia Jandira nem Pavão)
- ✓ Desktop only (não mobile)
- ✓ Sem multi-tenancy (professor único)
- ✓ Apenas 10 habilidades BNCC prioritárias

---

## FASE 2: CONSOLIDAÇÃO (Semanas 5-8)

**Status:** PLANEJADO

### Objetivos
- Estabilizar features existentes
- Adicionar módulos de português
- Melhorar feedback visual/sonoro
- Preparar para expansão

### Features Planejadas

#### 2.1 Módulos de Português (3 novos)
- [ ] **M06 - DestacaPalavraChave**
  - Clicar em substantivos/verbos/adjetivos em texto
  - Feedback visual imediato
  - 3 níveis de dificuldade
  
- [ ] **M08 - CorrigeOrtografia**
  - Identificar palavras com erro
  - Selecionar correção de lista
  - Erros típicos (ss/s/ç, mais/mas, acentuação)
  
- [ ] **M09 - CompreensãoLeitora**
  - Texto curto (2-3 parágrafos)
  - 3-5 perguntas múltipla escolha
  - Tipos: literal, inferencial, crítico

#### 2.2 Sistema de Sons Contextuais
- [ ] Integração Tone.js
- [ ] 5 sons básicos:
  - Acerto (harmonia)
  - Erro (dissonância leve)
  - Jeremias entende (acorde de resolução)
  - Progresso (tom crescente)
  - Conclusão (melodia completa)
- [ ] Toggle on/off de som

#### 2.3 Dashboard Professor Completo
- [ ] Visão de turma (teacher_dashboard view)
- [ ] Heatmap de conceitos (quais mais difíceis)
- [ ] Filtros por aluno/semana
- [ ] Exportar CSV
- [ ] Alertas (aluno não completou, travado >10min)

#### 2.4 Melhorias de UX
- [ ] Animações mais suaves (Framer Motion)
- [ ] Loading states melhores (skeletons)
- [ ] Toasts para feedback (success/error)
- [ ] Tutorial inicial (primeira vez do aluno)
- [ ] Settings acessíveis (ícone de engrenagem)

### Critérios de Conclusão
- [ ] 15 módulos funcionando (5 MVP + 10 novos)
- [ ] Professor usa dashboard semanalmente
- [ ] Sons contextuais agradáveis (não irritantes)
- [ ] Tutorial reduz confusão em 50%+

### Tempo Estimado
- **Desenvolvimento:** 3 semanas
- **Testes e ajustes:** 1 semana

---

## FASE 3: EXPANSÃO (Semanas 9-16)

**Status:** FUTURO

### Objetivos
- Cobrir mais habilidades BNCC (20 total)
- Adicionar mecânicas visuais-espaciais
- Implementar problemas integrados complexos
- Testar com 2-3 turmas (~30 alunos)

### Features Planejadas

#### 3.1 Módulos Visuais-Espaciais (3 novos)
- [ ] **M16 - ConstruçãoGeométrica**
  - Grid clicável
  - Formar figuras com área/perímetro específico
  - Validação geométrica
  
- [ ] **M17 - GráficoInterativo**
  - Construir gráficos de barras/linhas
  - Adicionar dados por clique
  - Interpretar gráficos prontos
  
- [ ] **M18 - PadrãoVisual**
  - Sequências numéricas e geométricas
  - Identificar regra
  - Completar padrão

#### 3.2 Módulo de Cenários Integrados
- [ ] **M11 - CenárioSequencial**
  - Encadear 3-5 módulos em narrativa
  - Exemplo: Ler carta → Identificar operação → Jeremias tenta → Aluno resolve
  - Barra de progresso por fase
  
- [ ] **Biblioteca de Cenários**
  - 5 cenários pré-criados:
    1. Feira de Ciências
    2. Festa de Aniversário
    3. Viagem de Campo
    4. Loja de Brinquedos
    5. Horta Escolar

#### 3.3 Sistema de Revisão Espaçada
- [ ] Algoritmo de repetição espaçada
- [ ] Questões reaparecem em 1 dia, 3 dias, 7 dias, 14 dias
- [ ] Priorizar conceitos com mastery_level < 0.75
- [ ] Módulo especial "Revisão Personalizada"

#### 3.4 Personagens Adicionais (Se Validado)
- [ ] **Tia Jandira** (Sistema de Hints)
  - Aparece quando aluno trava >2min
  - Oferece 3 níveis de dica (progressivos)
  - Não dá resposta direta
  
- [ ] **Pavão** (Desafios)
  - Antagonista amigável
  - Propõe desafios extras
  - Recompensas simbólicas (badges?)

#### 3.5 Analytics Básico
- [ ] Tempo médio por módulo
- [ ] Taxa de conclusão por tipo de atividade
- [ ] Conceitos com mais erro (agregado)
- [ ] Evolução temporal (gráfico de linha)

### Critérios de Conclusão
- [ ] 20 habilidades BNCC cobertas
- [ ] 20 módulos implementados
- [ ] Sistema de revisão funcional
- [ ] Testado com 30+ alunos em 2+ turmas

### Tempo Estimado
- **Desenvolvimento:** 6 semanas
- **Testes e iteração:** 2 semanas

---

## FASE 4: OTIMIZAÇÃO (Semanas 17-24)

**Status:** FUTURO

### Objetivos
- Melhorar performance e escalabilidade
- Adicionar features de engajamento (sem gamificação tóxica)
- Refinar algoritmo de adaptividade
- Preparar para multi-professor

### Features Planejadas

#### 4.1 Performance
- [ ] Code splitting (lazy loading de módulos)
- [ ] Otimização de imagens (WebP)
- [ ] Service Worker (PWA básico, cache offline)
- [ ] Índices adicionais no banco
- [ ] Query optimization (Supabase)

#### 4.2 Adaptividade Inteligente
- [ ] Algoritmo de dificuldade dinâmica
  - Acerta 3 seguidas → aumenta dificuldade
  - Erra 2 seguidas → diminui dificuldade
- [ ] Sugestão de próxima atividade baseada em gaps
- [ ] Predição de mastery_level futuro

#### 4.3 Engajamento Saudável
- [ ] Sistema de marcos (milestones) não-competitivo
  - "Você praticou 10 dias consecutivos!"
  - "Você ensinou Jeremias 20 vezes!"
  - "Você dominou frações!"
- [ ] Feedback narrativo do Jeremias evolui
- [ ] Mini-celebrações visuais (confete sutil)

#### 4.4 Multi-Professor (Preparação)
- [ ] Adicionar campo `teacher_id` nas tabelas
- [ ] Policies RLS por professor
- [ ] Dashboard de admin (gerenciar professores)
- [ ] Modo "escola" vs "professor individual"

#### 4.5 Acessibilidade Avançada
- [ ] Leitor de tela completo (ARIA labels)
- [ ] Navegação 100% por teclado
- [ ] Modo alto contraste
- [ ] Ajuste de velocidade de animações

### Critérios de Conclusão
- [ ] Carregamento <2s (mesmo em 3G)
- [ ] Algoritmo de adaptividade funciona bem (80% precisão)
- [ ] Multi-professor testado com 2 professores
- [ ] Acessibilidade WCAG 2.1 AAA

### Tempo Estimado
- **Desenvolvimento:** 6 semanas
- **Testes e refinamento:** 2 semanas

---

## FASE 5: ESCALA (Semanas 25-52)

**Status:** VISÃO

### Objetivos
- Escalar para múltiplas escolas
- Adicionar features colaborativas
- Criar marketplace de atividades (professores criam e compartilham)
- Monetização sustentável (se necessário)

### Features Visionárias

#### 5.1 Multi-Tenancy Completo
- [ ] Escolas como organizações
- [ ] Professores dentro de escolas
- [ ] Turmas com roster de alunos
- [ ] Permissões granulares

#### 5.2 Features Colaborativas
- [ ] Alunos podem criar problemas para outros
- [ ] Peer teaching (aluno ajuda colega)
- [ ] Discussões assíncronas em cenários
- [ ] Projetos em dupla (shared activities)

#### 5.3 Marketplace de Atividades
- [ ] Professores publicam atividades
- [ ] Outros professores podem "clonar" e adaptar
- [ ] Rating e reviews
- [ ] Filtros por série/BNCC/dificuldade
- [ ] Templates pré-prontos

#### 5.4 IA Generativa (Opcional)
- [ ] Geração automática de problemas (GPT-4)
- [ ] Feedback personalizado (análise de erro)
- [ ] Sugestões de caminho de aprendizado
- [ ] Criação de cenários narrativos

#### 5.5 Mobile App (React Native)
- [ ] Versão mobile nativa
- [ ] Sincronização offline
- [ ] Notificações push (lembretes gentis)

#### 5.6 Relatórios Avançados
- [ ] PDF exportável (relatório por aluno)
- [ ] Comparação com benchmarks nacionais
- [ ] Predição de desempenho futuro
- [ ] Recomendações automáticas de intervenção

### Critérios de Conclusão
- [ ] 100+ professores ativos
- [ ] 1000+ alunos usando
- [ ] Marketplace com 50+ atividades
- [ ] Sustentabilidade financeira (se necessário)

### Tempo Estimado
- **Desenvolvimento incremental:** 6 meses
- **Iteração contínua:** Ongoing

---

## MELHORIAS CONTÍNUAS (TODO MOMENTO)

### Conteúdo
- Adicionar 1-2 atividades novas por semana
- Refinar atividades existentes baseado em dados
- Atualizar catálogo de erros do Jeremias

### UX/UI
- A/B testing de interfaces
- Melhorias baseadas em feedback
- Ajustes de copy/linguagem

### Performance
- Monitorar e otimizar queries lentas
- Reduzir tamanho de bundle
- Melhorar First Contentful Paint

### Pedagógico
- Validar efetividade com métricas reais
- Ajustar thresholds de mastery
- Refinar algoritmo de adaptividade

---

## BACKLOG DE IDEIAS (NÃO PRIORIZADAS)

### Mecânicas Criativas
- [ ] Drag-and-drop para montar equações
- [ ] Linha do tempo interativa (eventos históricos)
- [ ] Mapa conceitual clicável
- [ ] Quebra-cabeças matemáticos (tangram digital)
- [ ] Animações explicativas (manim-style)

### Features Sociais
- [ ] Leaderboards opcionais (opt-in)
- [ ] Desafios semanais da comunidade
- [ ] Showcasing de trabalhos dos alunos
- [ ] Perfis públicos (com privacidade)

### Integrações
- [ ] Google Classroom
- [ ] Microsoft Teams
- [ ] Canvas LMS
- [ ] Export para Google Sheets
- [ ] Webhook para sistemas externos

### Acessibilidade Avançada
- [ ] Tradução para Libras (vídeos)
- [ ] Modo daltônico (paletas alternativas)
- [ ] Leitura de texto via TTS
- [ ] Controle por voz (comandos simples)

---

## CRITÉRIOS DE PRIORIZAÇÃO

Ao decidir o que implementar, considerar:

### Framework RICE
**Reach:** Quantos usuários impacta?  
**Impact:** Qual o impacto pedagógico?  
**Confidence:** Qual certeza de que funciona?  
**Effort:** Quanto tempo/esforço requer?

**Score RICE = (R × I × C) / E**

### Exemplo de Aplicação

**Feature:** Sistema de Hints (Tia Jandira)
- Reach: 100% dos alunos (todos travam às vezes)
- Impact: Alto (reduz frustração, melhora autonomia)
- Confidence: Médio (precisa testar implementação)
- Effort: 2 semanas

**Score RICE = (100 × 3 × 2) / 2 = 300**

**Feature:** Mobile App
- Reach: 30% dos alunos (contexto escolar = desktop)
- Impact: Médio (conveniência, mas não essencial)
- Confidence: Alto (tecnologia madura)
- Effort: 8 semanas

**Score RICE = (30 × 2 × 3) / 8 = 22.5**

**Conclusão:** Priorizar Hints sobre Mobile.

---

## DECISÕES ARQUITETURAIS FUTURAS

### Quando Escalar Backend?

**Sinais:**
- >50k requests/mês (limite Supabase Free)
- >500 MB database (limite Free)
- Queries ficando lentas (>1s)

**Opções:**
1. Upgrade Supabase Pro ($25/mês)
2. Migrar para infraestrutura própria (AWS/GCP)
3. Híbrido (Supabase + CDN + Cache)

### Quando Refatorar para TypeScript?

**Sinais:**
- >10k linhas de código
- >3 desenvolvedores no projeto
- Bugs de tipo frequentes

**Benefícios:**
- Type safety
- Autocompletion melhor
- Refactoring mais seguro

**Custo:**
- 2-4 semanas de migração
- Curva de aprendizado

### Quando Adicionar Testes Automatizados?

**Agora:**
- Testes unitários para lógica crítica (scoring, mastery)

**Fase 3:**
- Testes E2E para fluxos principais (Playwright)

**Fase 4:**
- Visual regression testing (Percy/Chromatic)

---

## MÉTRICAS DE SUCESSO POR FASE

### MVP (Fase 1)
- 80%+ taxa de conclusão
- <3 bugs críticos/mês
- Feedback qualitativo positivo

### Consolidação (Fase 2)
- 90%+ taxa de conclusão
- Professor usa dashboard 2x/semana
- Tempo médio por atividade estável (~20min)

### Expansão (Fase 3)
- 2+ turmas usando
- 15+ conceitos BNCC com mastery_level > 0.75
- Retenção semanal >85%

### Otimização (Fase 4)
- Carregamento <2s
- 95%+ uptime
- Multi-professor funcionando

### Escala (Fase 5)
- 100+ professores
- 1000+ alunos
- Sustentabilidade financeira

---

## RISCOS E MITIGAÇÕES

### Risco 1: Alunos Acham Repetitivo
**Mitigação:**
- Variar mecânicas entre semanas
- Narrativa do Jeremias evolui
- Desafios opcionais (Pavão)
- Revisão espaçada (não repetir igual)

### Risco 2: Professor Não Consegue Criar Atividades
**Mitigação:**
- Editor cada vez mais simples
- Templates pré-prontos
- Marketplace de atividades
- Vídeos tutoriais curtos

### Risco 3: Performance Degrada com Escala
**Mitigação:**
- Monitoramento proativo (Sentry, Plausible)
- Otimização contínua
- Upgrade de infraestrutura planejado
- Cache agressivo

### Risco 4: Bugs Críticos em Produção
**Mitigação:**
- Testes abrangentes pré-deploy
- Staging environment
- Rollback rápido (Vercel)
- Backups diários (Supabase)

### Risco 5: Desengajamento Após Novidade
**Mitigação:**
- Conteúdo sempre novo (1-2 atividades/semana)
- Feedback constante dos alunos
- Mecânicas surpresa ocasionais
- Evolução do Jeremias mantém interesse

---

## CHECKLIST DE EVOLUÇÃO SUSTENTÁVEL

Para garantir crescimento saudável:

- [ ] **Documentação atualizada** a cada nova feature
- [ ] **Testes** para todo código crítico novo
- [ ] **Feedback dos usuários** coletado sistematicamente
- [ ] **Métricas monitoradas** semanalmente
- [ ] **Backlog priorizado** por impacto pedagógico
- [ ] **Tech debt** endereçado regularmente (20% do tempo)
- [ ] **Performance** não degrada (benchmark mensal)
- [ ] **Acessibilidade** mantida em todas novas features

---

## VISÃO DE LONGO PRAZO (3-5 Anos)

**Dimidui como plataforma de prática adaptativa**
- Referência em educação gamificada sem gamificação tóxica
- Usado por 10k+ alunos em 100+ escolas
- Marketplace vibrante com 500+ atividades
- Pesquisa acadêmica validando efetividade
- Open-source (comunidade contribuindo)
- Sustentável financeiramente (freemium ou B2B)

**Impacto Desejado:**
- Reduzir lacunas de aprendizado em 30%+
- Aumentar autonomia e metacognição dos alunos
- Liberar tempo do professor para atendimento individualizado
- Tornar prática deliberada mais eficiente e agradável

---

**Roadmap vivo - revisar trimestralmente! 🗺️**