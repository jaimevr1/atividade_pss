# Epic 4: Sistema Visual e Experiência

**ID:** EPIC-004
**Status:** Planejado
**Prioridade:** Must Have (MVP)
**Estimativa:** 1.5 semanas

## Objetivo

Implementar a identidade visual e princípios de design definidos na PRD, garantindo experiência respeitosa e acessível para faixa etária 9-12 anos.

## Valor de Negócio

Garante diferenciação pedagógica através do "Respeito Cognitivo" - interface madura sem infantilização, promovendo engajamento sustentável e reduzindo barreiras de acessibilidade.

## Princípios de Design (PRD)

- **P2:** Feedback Formativo, Não Punitivo
- **P3:** Respeito Cognitivo (sem infantilização)
- **P4:** Economia de Atenção (propósito pedagógico)
- **P5:** Transparência de Progresso

## User Stories Incluídas

- **US-011:** Sistema de design tokens (cores, tipografia, espaçamento)
- **US-012:** Animações contextuais com Framer Motion
- **US-013:** Acessibilidade (OpenDyslexic, temas claro/escuro)
- **US-014:** Sons contextuais com Tone.js

## Paleta de Cores

### Light Mode
```javascript
{
  background: "#E8EDF2",     // Cinza azulado claro
  surface: "#FFFFFF",        // Cards/módulos
  primary: "#6B46C1",        // Roxo médio
  semantic: {
    correct: "#48BB78",      // Verde
    incorrect: "#F56565",    // Vermelho
    warning: "#ED8936",      // Laranja
    info: "#4299E1"          // Azul
  }
}
```

### Dark Mode
```javascript
{
  background: "#1A202C",     // Azul escuro profundo
  surface: "#2D3748",        // Cards/módulos
  primary: "#9F7AEA",        // Roxo claro
  // ... cores adaptadas
}
```

## Tipografia

```
H1: 32px/700/Atkinson Hyperlegible    # Título atividade
H2: 24px/700/Atkinson Hyperlegible    # Seção
Question: 20px/700/Atkinson           # Enunciados
Body: 18px/400/Atkinson               # Texto corrido
Dialogue: 18px/400/Comic Sans MS      # Falas Jeremias
Feedback: 16px/400/Atkinson           # Feedback inline
Meta: 14px/400/Atkinson               # Info secundária
```

## Animações (Framer Motion)

```javascript
transitions: {
  instant: "100ms",    // Hover
  fast: "200ms",       // Botões, inputs
  normal: "350ms",     // Transições padrão
  slow: "600ms"        // Revelar conteúdo
}

animations: {
  feedbackCorrect: {
    type: "subtle-glow",
    duration: "600ms",
    color: "semantic.correct"
  },
  feedbackIncorrect: {
    type: "shake-horizontal",
    duration: "400ms",
    amplitude: "8px"
  },
  jeremiasMood: {
    type: "bounce",
    duration: "500ms"
  }
}
```

## Sons Contextuais (Tone.js)

### Princípio
Sons = informação, não recompensa pavloviana. Síntese em tempo real, não arquivos MP3.

```javascript
sounds: {
  math: {
    number_placement_correct: {
      type: "pitched_tone",
      mapping: "logarithmic" // 5=nota média, 500=aguda
    },
    balance_achieved: {
      type: "harmony",
      notes: ["C4", "G4"], // Quinta perfeita
      duration: "500ms"
    }
  },
  portuguese: {
    syllable_click: {
      type: "pitched_click"
      // Sílaba tônica = tom mais alto
    }
  },
  jeremias: {
    realizes_mistake: {
      type: "discovery_chord",
      progression: ["Dm", "G", "C"]
    }
  }
}
```

## Critérios de Aceitação

### Design System
- [ ] Tokens implementados em Tailwind CSS
- [ ] Componentes reutilizáveis (Button, Card, Input)
- [ ] Modo claro/escuro funcionais
- [ ] Typography scale consistente

### Animações
- [ ] Feedback visual para acerto/erro
- [ ] Transições suaves entre módulos
- [ ] Animação de mood do Jeremias
- [ ] Performance: 60fps em desktop 1024px+

### Acessibilidade
- [ ] Toggle fontes (Atkinson ↔ OpenDyslexic)
- [ ] Contraste WCAG AA em ambos os temas
- [ ] Navegação por teclado completa
- [ ] Targets clicáveis ≥48px×48px

### Sons
- [ ] Sistema de síntese Tone.js configurado
- [ ] Sons contextuais para feedback matemático
- [ ] Controle de volume/mute
- [ ] Graceful degradation (sem sons = funciona)

## Dependências

- **Pré-requisitos:** Estrutura base React + Vite
- **Bloqueia:** Todos os módulos visuais

## Definição de Pronto

- [ ] Design system completo e documentado
- [ ] Storybook com todos os componentes
- [ ] Testes visuais automatizados
- [ ] Fontes carregadas corretamente (local .woff2)
- [ ] Performance: First Paint <1.5s
- [ ] Testes de acessibilidade passando

## Riscos

| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| Fonte OpenDyslexic não carrega | Baixa | Médio | Fallback para system fonts |
| Animações afetam performance | Média | Médio | Profiling, reduced motion |
| Sons não funcionam em alguns browsers | Alta | Baixo | Feature detection, fallback |

## Assets Necessários

- **Fontes:** Atkinson Hyperlegible (Google), OpenDyslexic (.woff2 local)
- **Ícones:** Lucide React (SVG)
- **Ilustrações:** Emoji 96px (avatar Jeremias)
- **Nenhuma imagem custom** (mantém simplicidade)