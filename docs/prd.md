# Dimidui - Especificação Técnica Completa

**Versão:** 1.0  
**Data:** Outubro 2025  
**Status:** Aprovado para Desenvolvimento

---

## 1. VISÃO GERAL DO PROJETO

### 1.1 Identidade

**Nome:** Dimidui  
**Tagline:** Aprenda Fazendo, Ensine Crescendo  
**Propósito:** Plataforma modular de atividades educacionais para prática supervisionada em matemática e português, focada em fluência conceitual e resolução de problemas integrados.

### 1.2 Contexto

**Público-Alvo:**
- Primário: Alunos 4º-6º ano Fundamental I (9-12 anos)
- Secundário: Professor como facilitador/monitor

**Ambiente de Uso:**
- Local: Laboratório escolar (5 computadores desktop)
- Frequência: 20min/semana (fluência) + 20min/semana opcional (integração)
- Modelo: Aula supervisionada, trabalho individual
- Conectividade: Online (Supabase backend)

### 1.3 Objetivos Pedagógicos

**Primários:**
1. Desenvolver fluência em operações matemáticas básicas
2. Construir compreensão conceitual através de problemas integrados
3. Treinar metacognição via ensino do "Jeremias" (técnica Feynman)
4. Fortalecer interpretação de texto em contextos matemáticos

**Não-Objetivos:**
- Substituir ensino tradicional
- Funcionar sem supervisão adulta
- Cobrir todo currículo BNCC
- Gamificação competitiva (ranks, leaderboards)

---

## 2. PRINCÍPIOS DE DESIGN

**P1. Modularidade Radical**  
Todo componente é configurável via JSON. Professor cria atividades combinando módulos.

**P2. Feedback Formativo, Não Punitivo**  
Erro = oportunidade de diagnóstico. Sem "game over" ou penalidades dramáticas.

**P3. Respeito Cognitivo**  
Interface clara sem infantilização. Paleta madura, tipografia generosa.

**P4. Economia de Atenção**  
Cada interação tem propósito pedagógico. Sem animações/sons decorativos.

**P5. Transparência de Progresso**  
Aluno e professor veem claramente domínio vs necessidade de prática.

**P6. Progressão Desbloqueável**  
Conteúdo avançado só acessível após demonstrar pré-requisitos.

---

## 3. IDENTIDADE VISUAL

### 3.1 Tipografia

```
HIERARQUIA:
H1: 32px / 700 / Atkinson Hyperlegible     # Título de atividade
H2: 24px / 700 / Atkinson Hyperlegible     # Seção
Question: 20px / 700 / Atkinson Hyperlegible  # Enunciados
Body: 18px / 400 / Atkinson Hyperlegible   # Texto corrido
Dialogue: 18px / 400 / Comic Sans MS       # Falas do Jeremias
Feedback: 16px / 400 / Atkinson Hyperlegible  # Feedback inline
Meta: 14px / 400 / Atkinson Hyperlegible   # Informações secundárias

ACESSIBILIDADE:
- Line Height: 1.6 (mínimo)
- Letter Spacing: 0.01em
- Max Line Length: 65ch
```

**Fontes:**
- **Primary:** Atkinson Hyperlegible (Google Fonts)
- **Alternative:** OpenDyslexic (asset local .woff2) - toggle no settings
- **Character:** Comic Sans MS (system font) - apenas diálogos do Jeremias

### 3.2 Paleta de Cores

#### LIGHT MODE

```javascript
{
  background: "#E8EDF2",        // Cinza azulado claro
  surface: "#FFFFFF",           // Cards/módulos
  
  primary: {
    base: "#6B46C1",            // Roxo médio
    light: "#9F7AEA",           // Roxo claro (gradiente)
    dark: "#553C9A"             // Roxo escuro (gradiente)
  },
  
  text: {
    onLight: "#2D3748",         // Preto suave em fundos claros
    onDark: "#FFFFFF",          // Branco em fundos escuros
    secondary: "#4A5568"        // Texto secundário
  },
  
  semantic: {
    correct: "#48BB78",         // Verde
    incorrect: "#F56565",       // Vermelho
    warning: "#ED8936",         // Laranja
    info: "#4299E1"             // Azul
  },
  
  actions: {
    primary: "#F6AD55",         // Laranja quente (ações principais)
    secondary: "#A0AEC0",       // Cinza (ações secundárias)
    danger: "#E53E3E"           // Vermelho (encerramento/cancelar)
  }
}
```

#### DARK MODE

```javascript
{
  background: "#1A202C",        // Azul escuro profundo
  surface: "#2D3748",           // Cards/módulos
  
  primary: {
    base: "#9F7AEA",            // Roxo claro
    light: "#B794F4",           // Roxo mais claro (gradiente)
    dark: "#805AD5"             // Roxo médio (gradiente)
  },
  
  text: {
    onLight: "#E2E8F0",         // Branco suave
    onDark: "#1A202C",          // Preto em elementos claros
    secondary: "#A0AEC0"        // Texto secundário
  },
  
  semantic: {
    correct: "#68D391",         // Verde claro
    incorrect: "#FC8181",       // Vermelho claro
    warning: "#F6AD55",         // Laranja claro
    info: "#63B3ED"             // Azul claro
  },
  
  actions: {
    primary: "#FBD38D",         // Laranja claro
    secondary: "#718096",       // Cinza médio
    danger: "#FC8181"           // Vermelho claro
  }
}
```

### 3.3 Espaçamento & Layout

```javascript
spacing: {
  xs: "4px",
  sm: "8px",
  md: "16px",
  lg: "24px",
  xl: "32px",
  xxl: "48px",
  xxxl: "64px"
}

layout: {
  containerMaxWidth: "1200px",
  contentMaxWidth: "800px",
  
  moduleCard: {
    padding: "24px",
    borderRadius: "12px",
    shadow: "0 2px 8px rgba(0,0,0,0.1)"
  },
  
  clickTarget: "min 48px × 48px",  // Acessibilidade
  betweenOptions: "16px"           // Evita clique acidental
}
```

### 3.4 Animações

```javascript
transitions: {
  instant: "100ms",    // Hover
  fast: "200ms",       // Botões, inputs
  normal: "350ms",     // Transições padrão
  slow: "600ms",       // Revelar conteúdo
  
  easing: {
    standard: "cubic-bezier(0.4, 0.0, 0.2, 1)",
    enter: "cubic-bezier(0.0, 0.0, 0.2, 1)",
    exit: "cubic-bezier(0.4, 0.0, 1, 1)"
  }
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
    duration: "500ms",
    easing: "cubic-bezier(0.68, -0.55, 0.265, 1.55)"
  }
}
```

### 3.5 Uso de Emojis

**REGRA:**
- ✅ Decoração/estado emocional: Emojis OK (Jeremias 😕 vs 😊)
- ❌ Conteúdo pedagógico: SVGs customizados (garantia de consistência)

**Exemplo:**
```
❌ "Complete o padrão: 🔴🔵🔴🔵__"  (inconsistente entre OSs)
✅ "Complete o padrão: ●●●●__" (círculos CSS/SVG controlados)
```

---

## 4. SISTEMA DE PERSONAGENS

### 4.1 Jeremias (MVP - Único Personagem)

**Função Pedagógica:**  
Aluno virtual que comete erros conceituais típicos. Criança identifica erro, explica correção, resolve problema similar (técnica Feynman).

**Personalidade:**
- Curioso e colaborativo
- Confiante mas humilde quando corrigido
- Linguagem similar à criança (gírias sutis: "tipo", "cara")

#### Estados Emocionais

```javascript
moods: {
  neutral: {
    emoji: "🙂",
    description: "Pronto para aprender"
  },
  thinking: {
    emoji: "🤔",
    description: "Tentando resolver"
  },
  confused: {
    emoji: "😕",
    description: "Não tenho certeza..."
  },
  eureka: {
    emoji: "😮",
    description: "Ah! Entendi!"
  },
  confident: {
    emoji: "😊",
    description: "Acho que sei fazer isso"
  },
  grateful: {
    emoji: "🙏",
    description: "Obrigado por me ensinar!"
  }
}
```

#### Níveis de Maturidade

```javascript
levels: {
  1: {
    label: "Iniciante",
    errorFrequency: "alta",
    errorTypes: ["operação_básica", "leitura_superficial"],
    unlockAt: 0, // início
    dialogue: "Estou começando a aprender isso..."
  },
  
  2: {
    label: "Praticando",
    errorFrequency: "média-alta",
    errorTypes: ["reagrupamento", "interpretação_literal"],
    unlockAt: 5, // após 5 correções
    dialogue: "Já melhorei, mas ainda confundo algumas coisas."
  },
  
  3: {
    label: "Em Progresso",
    errorFrequency: "média",
    errorTypes: ["ordem_operações", "contexto_problema"],
    unlockAt: 10,
    dialogue: "Estou ficando bom nisso!"
  },
  
  4: {
    label: "Avançado",
    errorFrequency: "baixa",
    errorTypes: ["erros_sutis", "casos_especiais"],
    unlockAt: 20,
    dialogue: "Raramente erro agora. Valeu pela ajuda!"
  },
  
  5: {
    label: "Mestre",
    errorFrequency: "nenhuma",
    errorTypes: [],
    unlockAt: 35,
    dialogue: "Agora eu ensino você! Crie um problema para EU resolver?"
  }
}
```

#### Avatar Visual

**Decisão:** Emoji grande (96x96px) + nome "Jeremias"
- Custo zero de produção
- Consistente com mood system
- Rápido de implementar

### 4.2 Personagens Futuros (Versão 2+)

**Tia Jandira:** Sistema de hints quando aluno trava  
**Pavão:** Antagonista (se houver demanda pedagógica validada)

---

## 5. CATÁLOGO DE ERROS DO JEREMIAS

### 5.1 Matemática

#### Adição/Subtração

**Nível 1: Inverte Operação**
```
Problema: 5 + 3 = ?
Erro Jeremias: "É 5 - 3 = 2!"
Tipo: confunde_operacao
Correção: Identificar símbolo correto
```

**Nível 2: Esquece Reagrupamento**
```
Problema: 27 + 15 = ?
Erro Jeremias: "É 312 porque 2+1=3, 7+5=12"
Tipo: esquece_vai_um
Correção: Revisar reagrupamento
```

**Nível 3: Ordem em Problema Verbal**
```
Problema: "João tinha 15 maçãs, deu 8 para Maria"
Erro Jeremias: "É 8 - 15 = -7"
Tipo: ordem_subtração
Correção: Identificar ordem correta
```

#### Multiplicação

**Nível 1: Confunde com Adição**
```
Problema: 3 × 4 = ?
Erro Jeremias: "É 3 + 4 = 7"
Tipo: confunde_multiplicacao_adicao
Correção: Diferença entre operações
```

**Nível 2: Esquece Dezenas**
```
Problema: 23 × 4 = ?
Erro Jeremias: "20×4=80, 3×4=12, então 8012"
Tipo: esquece_reagrupamento_mult
Correção: Reagrupamento em multiplicação
```

**Nível 3: Confunde Unidades**
```
Problema: "3 caixas com 5 bolas cada"
Erro Jeremias: "Preciso de 3+5=8 caixas"
Tipo: confunde_unidade_total
Correção: Identificar o que está sendo multiplicado
```

#### Divisão

**Nível 1: Inverte Dividendo/Divisor**
```
Problema: 12 ÷ 3 = ?
Erro Jeremias: "É 3 ÷ 12 = 0,25"
Tipo: inverte_ordem_divisao
Correção: Ordem na divisão importa
```

**Nível 2: Ignora Resto**
```
Problema: 17 ÷ 5 = ?
Erro Jeremias: "É 3"
Tipo: ignora_resto
Correção: Identificar e representar resto
```

**Nível 3: Confunde Distribuição**
```
Problema: "23 figurinhas para 4 amigos"
Erro Jeremias: "Cada um recebe 4"
Tipo: confunde_dividendo_divisor
Correção: Identificar quem recebe o quê
```

#### Frações

**Nível 1: Compara Apenas Numerador**
```
Problema: 1/4 vs 1/3, qual maior?
Erro Jeremias: "1/4 é maior porque 4>3"
Tipo: compara_so_numerador
Correção: Denominador maior = partes menores
```

**Nível 2: Soma Direto**
```
Problema: 1/4 + 1/4 = ?
Erro Jeremias: "É 2/8"
Tipo: soma_numerador_denominador
Correção: Soma com mesmo denominador
```

**Nível 3: Equivalência Literal**
```
Problema: 1/2 = 2/4?
Erro Jeremias: "Não, porque números diferentes"
Tipo: nao_entende_equivalencia
Correção: Conceito de frações equivalentes
```

#### Geometria

**Nível 1: Confunde Perímetro/Área**
```
Problema: "Área do retângulo 4×5"
Erro Jeremias: "É 4+5+4+5=18"
Tipo: confunde_perimetro_area
Correção: Diferença entre medidas
```

**Nível 2: Conta Cantos como Lados**
```
Problema: "Quantos lados tem o quadrado?"
Erro Jeremias: "Tem 8, 4 lados e 4 cantos"
Tipo: confunde_lado_vertice
Correção: Lado vs vértice
```

### 5.2 Português

#### Ortografia

**Nível 1: ss/s/ç**
```
Palavra: passar
Erro Jeremias: "pasar"
Tipo: uso_ss_entre_vogais
Correção: Regra do ss
```

**Nível 2: mais/mas**
```
Frase: "Eu quero ____ sorvete"
Erro Jeremias: "mas" (ao invés de mais)
Tipo: confunde_mais_mas
Correção: mais=quantidade, mas=oposição
```

**Nível 3: Acentuação**
```
Palavra: música
Erro Jeremias: "musica"
Tipo: falta_acento_tonico
Correção: Identificar sílaba tônica e regra
```

#### Concordância

**Nível 1: Singular/Plural Verbo**
```
Frase: "Os menino corre"
Erro Jeremias: mantém verbo no singular
Tipo: concordancia_verbal
Correção: Verbo concorda com sujeito
```

**Nível 2: Singular/Plural Adjetivo**
```
Frase: "As casa bonita"
Erro Jeremias: não pluraliza adjetivo
Tipo: concordancia_nominal
Correção: Adjetivo concorda com substantivo
```

#### Interpretação

**Nível 1: Literal Demais**
```
Texto: "Maria está azul hoje"
Erro Jeremias: "Maria pintou a pele de azul"
Tipo: interpretacao_literal
Correção: Identificar linguagem figurada
```

**Nível 2: Informação Implícita**
```
Texto: "Pedro saiu de casaco"
Pergunta: Estava frio?
Erro Jeremias: "Não diz no texto"
Tipo: nao_faz_inferencia
Correção: Fazer inferências razoáveis
```

**Nível 3: Causa/Consequência**
```
Erro Jeremias: Inverte ordem de eventos
Tipo: confunde_cronologia
Correção: Identificar relação temporal
```

#### Pontuação

**Nível 1: Esquece Ponto Final**
```
Erro Jeremias: Múltiplas frases sem pontuação
Tipo: falta_ponto_final
Correção: Finalizar frases
```

**Nível 2: Vírgula Muda Sentido**
```
Exemplos: "Não, pode entrar" vs "Não pode entrar"
Erro Jeremias: Não percebe diferença
Tipo: impacto_virgula
Correção: Vírgula altera significado
```

---

## 6. HABILIDADES BNCC (10 Prioritárias MVP)

### 6.1 Matemática (6 habilidades)

**EF04MA02 - Ordem de Grandeza**  
Ler, escrever e ordenar números até dezena de milhar  
*Módulos:* M4 (OrdemCrescente), M2 (LinhaNumérica)

**EF04MA07 - Problemas de Multiplicação**  
Resolver problemas envolvendo diferentes significados da multiplicação  
*Módulos:* M1 (BateriaRápida), M10 (AlexTentaResolver), M5 (IdentificaOperação)

**EF05MA03 - Divisão com Resto**  
Identificar e representar divisão com resto  
*Módulos:* M1 (BateriaRápida), M10 (AlexTentaResolver)

**EF05MA08 - Frações**  
Representar e comparar frações  
*Módulos:* M3 (FraçãoVisual)

**EF05MA20 - Figuras Planas**  
Reconhecer propriedades de triângulos e quadriláteros  
*Módulos:* M16 (ConstruçãoGeométrica)

**EF06MA24 - Gráficos**  
Resolver problemas com dados em gráficos  
*Módulos:* M17 (GráficoInterativo)

### 6.2 Português (4 habilidades)

**EF04LP04 - Ortografia e Concordância**  
Usar regras básicas de concordância nominal e verbal  
*Módulos:* M8 (CorrigeOrtografia)

**EF04LP15 - Localização de Informação**  
Localizar informações explícitas em texto  
*Módulos:* M9 (CompreensãoLeitora), M6 (DestacaPalavraChave)

**EF05LP05 - Ortografia: Acentuação**  
Grafar palavras com regras de acentuação  
*Módulos:* M8 (CorrigeOrtografia)

**EF05LP15 - Interpretação Inferencial**  
Reconhecer relações lógicas entre partes de texto  
*Módulos:* M9 (CompreensãoLeitora), M11 (CenárioSequencial)

---

## 7. BIBLIOTECA DE MÓDULOS (20 Componentes)

### CATEGORIA 1: FLUÊNCIA - Prática Repetitiva

#### M1. BateriaRápida
**Função:** Gera N problemas matemáticos do mesmo tipo. Feedback imediato.  
**Config:** tipo operação, alcance numérico, quantidade, tempo limite  
**Use Case:** 30 contas de divisão, 40 multiplicações

#### M2. LinhaNuméricaInterativa
**Função:** Arrastar marcador para posição correta na reta.  
**Config:** alcance, alvos, tolerância, saltos disponíveis  
**Use Case:** Localizar 347 na reta 0-1000

#### M3. FraçãoVisual
**Função:** Manipular representações visuais de frações.  
**Config:** tipo representação (barra/pizza/grid), frações alvo, modo  
**Use Case:** Construir 3/8, comparar 1/4 vs 1/3

#### M4. OrdemCrescente
**Função:** Ordenar números por arrastar ou clicar em sequência.  
**Config:** lista de números, modo interação  
**Use Case:** Ordenar 347, 89, 1205, 456

#### M5. IdentificaOperação
**Função:** Ler problema verbal, identificar operação.  
**Config:** lista de problemas, opções de operação  
**Use Case:** "João tinha 5 e ganhou 3" → adição

### CATEGORIA 2: INTERPRETAÇÃO - Português

#### M6. DestacaPalavraChave
**Função:** Clicar em palavras específicas em texto.  
**Config:** texto, categorias alvo (substantivos/verbos), feedback  
**Use Case:** Identificar todos os substantivos

#### M7. OrdenaSentença
**Função:** Arrastar palavras para formar frase correta.  
**Config:** palavras embaralhadas, solução  
**Use Case:** Formar frase gramaticalmente correta

#### M8. CorrigeOrtografia
**Função:** Clicar em palavras erradas e corrigir.  
**Config:** texto com erros, tipo interação (selecionar/digitar)  
**Use Case:** Encontrar "pesso" → "pesou"

#### M9. CompreensãoLeitora
**Função:** Ler texto, responder perguntas múltipla escolha.  
**Config:** texto, lista de perguntas, tipo (literal/inferencial)  
**Use Case:** "Qual era o problema do personagem?"

### CATEGORIA 3: INTEGRAÇÃO - Jeremias & Galáticos

#### M10. JeremiasResolver
**Função:** Jeremias mostra raciocínio com erro. Aluno identifica + corrige.  
**Config:** problema, raciocínio, passo do erro, tipo de erro  
**Use Case:** Jeremias confunde adição com multiplicação

#### M11. CenárioSequencial
**Função:** Sequência de módulos encadeados formando problema complexo.  
**Config:** lista de fases (cada fase é um módulo)  
**Use Case:** Ler carta → identificar operação → resolver → ensinar Jeremias

#### M12. ProblemaAberto
**Função:** Problema com múltiplas soluções válidas.  
**Config:** contexto, constraints, critérios validação  
**Use Case:** "Comprar material com R$50, ≥3 itens"

### CATEGORIA 4: METACOGNIÇÃO

#### M13. PrevisãoDesempenho
**Função:** Aluno estima quantas questões acertará.  
**Config:** tipo atividade, escala previsão  
**Use Case:** "Acha que vai acertar quantas de 30?"

#### M14. AutoAvaliação
**Função:** Aluno classifica própria compreensão.  
**Config:** lista conceitos, escala (3-5 níveis)  
**Use Case:** "Divisão com resto: Não entendo / Entendo um pouco / Domino / Sei ensinar"

#### M15. ReflexãoPós
**Função:** Perguntas sobre processo (seleção de opções).  
**Config:** lista perguntas, opções pré-definidas  
**Use Case:** "O que foi mais difícil? (cálculo/interpretação/outro)"

### CATEGORIA 5: VISUAL-ESPACIAL

#### M16. ConstruçãoGeométrica
**Função:** Clicar em grid para formar figura com constraints.  
**Config:** tamanho grid, objetivo (área/perímetro), modo  
**Use Case:** "Forme retângulo de área 24"

#### M17. GráficoInterativo
**Função:** Construir ou completar gráfico.  
**Config:** dados iniciais, tarefa  
**Use Case:** "Adicione barra de quarta-feira com valor 7"

#### M18. PadrãoVisual
**Função:** Identificar regra e completar sequência.  
**Config:** sequência inicial, tipo padrão  
**Use Case:** "●●●●__?" ou "2,4,6,8,__?"

### CATEGORIA 6: INTERFACE/FEEDBACK

#### M19. BarraProgresso
**Função:** Barra visual mostrando avanço + marcos.  
**Config:** total módulos, marcos (mensagens em pontos)  
**Use Case:** "3/5 módulos. Jeremias está entendendo!"

#### M20. EstadoJeremias
**Função:** Exibe avatar do Jeremias com mood + diálogo.  
**Config:** estado emocional, texto  
**Use Case:** Transições entre módulos, feedback narrativo

---

## 8. ARQUITETURA DO SISTEMA

### 8.1 Estrutura de Rotação Semanal

```
SEMANA TIPO A: FLUÊNCIA (20min)
├─ Introdução (M20 - EstadoJeremias) - 2min
├─ Prática em Volume (M1 - BateriaRápida) - 15min
└─ Autoavaliação (M14) - 3min

SEMANA TIPO B: INTEGRAÇÃO (20min)
├─ Contexto Narrativo (M9 ou M11) - 3min
├─ Jeremias Tenta Resolver (M10) - 8min
├─ Aluno Resolve Similar - 7min
└─ Reflexão (M15) - 2min

ROTAÇÃO:
Turma Manhã: Segunda (Fluência) + Sexta (Integração)
Turma Tarde: Terça (Fluência apenas)
```

### 8.2 Sistema de Progressão Conceitual

```javascript
grafo_conceitos: {
  "multiplicação_básica": {
    prerequisitos: ["adição_básica"],
    desbloqueia: ["divisão_simples", "área_retângulo"],
    threshold: 0.80  // 80% acerto em 15+ tentativas
  },
  
  "divisão_com_resto": {
    prerequisitos: ["divisão_simples"],
    desbloqueia: ["problema_integrado_feira"],
    threshold: 0.75
  }
}

regra_desbloqueio: {
  atividade_integrada: {
    requer: ["conceito_A", "conceito_B", "conceito_C"],
    logica: "todos acima do threshold"
  }
}
```

---

## 9. SCHEMA DO BANCO DE DADOS

### 9.1 Estrutura (Supabase PostgreSQL)

```sql
-- TABELA: students
CREATE TABLE students (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  grade_level INT CHECK (grade_level IN (4,5,6)),
  created_at TIMESTAMP DEFAULT NOW(),
  
  -- Preferências
  font_preference TEXT DEFAULT 'atkinson' 
    CHECK (font_preference IN ('atkinson', 'opendyslexic')),
  theme_preference TEXT DEFAULT 'light' 
    CHECK (theme_preference IN ('light', 'dark'))
);

-- TABELA: activities
CREATE TABLE activities (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  week_number INT,
  type TEXT CHECK (type IN ('fluencia', 'integracao')),
  bncc_codes TEXT[],
  estimated_duration INT, -- minutos
  
  -- Configuração em JSON
  modules JSONB NOT NULL,
  
  created_at TIMESTAMP DEFAULT NOW(),
  published BOOLEAN DEFAULT FALSE
);

-- TABELA: student_progress
CREATE TABLE student_progress (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  student_id UUID REFERENCES students(id) ON DELETE CASCADE,
  activity_id UUID REFERENCES activities(id) ON DELETE CASCADE,
  
  -- Status
  status TEXT CHECK (status IN ('not_started', 'in_progress', 'completed')),
  started_at TIMESTAMP,
  completed_at TIMESTAMP,
  time_spent INT, -- segundos
  
  -- Dados de desempenho
  interactions JSONB, -- Array de cada interação
  final_score FLOAT,
  
  UNIQUE(student_id, activity_id)
);

-- TABELA: concept_mastery
CREATE TABLE concept_mastery (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  student_id UUID REFERENCES students(id) ON DELETE CASCADE,
  bncc_code TEXT NOT NULL,
  
  -- Métricas
  attempts INT DEFAULT 0,
  correct INT DEFAULT 0,
  incorrect INT DEFAULT 0,
  mastery_level FLOAT DEFAULT 0.0, -- 0.0 a 1.0
  
  last_practiced TIMESTAMP,
  
  UNIQUE(student_id, bncc_code)
);

-- TABELA: jeremias_state
CREATE TABLE jeremias_state (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  student_id UUID REFERENCES students(id) ON DELETE CASCADE,
  
  -- Estado
  maturity_level INT DEFAULT 1 
    CHECK (maturity_level BETWEEN 1 AND 5),
  concepts_taught TEXT[],
  
  -- Contadores
  times_taught INT DEFAULT 0,
  corrections_made INT DEFAULT 0,
  
  -- Narrativa
  last_dialogue TEXT,
  last_mood TEXT,
  last_interaction TIMESTAMP,
  
  UNIQUE(student_id)
);

-- VIEW: Dashboard do Professor
CREATE VIEW teacher_dashboard AS
SELECT 
  s.name AS student_name,
  s.grade_level,
  COUNT(DISTINCT sp.activity_id) FILTER 
    (WHERE sp.status = 'completed') AS activities_completed,
  AVG(sp.final_score) AS avg_score,
  j.maturity_level AS jeremias_level,
  COUNT(DISTINCT cm.bncc_code) FILTER 
    (WHERE cm.mastery_level >= 0.75) AS concepts_mastered
FROM students s
LEFT JOIN student_progress sp ON s.id = sp.student_id
LEFT JOIN jeremias_state j ON s.id = j.student_id
LEFT JOIN concept_mastery cm ON s.id = cm.student_id
GROUP BY s.id, s.name, s.grade_level, j.maturity_level;
```

### 9.2 Lógica de Evolução do Jeremias

```javascript
// Quando aluno ensina Jeremias corretamente
function updateJeremiasProgress(studentId, conceptTaught, wasCorrect) {
  if (wasCorrect) {
    await supabase
      .from('jeremias_state')
      .update({
        concepts_taught: sql`array_append(concepts_taught, '${conceptTaught}')`,
        times_taught: sql`times_taught + 1`,
        corrections_made: sql`corrections_made + 1`,
        
        // A cada 5 correções, Jeremias "amadurece"
        maturity_level: sql`
          CASE 
            WHEN (corrections_made + 1) % 5 = 0 
            THEN LEAST(maturity_level + 1, 5)
            ELSE maturity_level
          END
        `
      })
      .eq('student_id', studentId);
  }
}

// Quando gera erro do Jeremias
function generateJeremiasError(concept, studentMaturityLevel) {
  const errorCatalog = {
    "multiplicação": {
      level1: "confunde_com_adicao",
      level2: "esquece_reagrupamento",
      level3: "ordem_operacoes",
      level4: "erro_sutil_unidade",
      level5: null // não erra mais
    }
  };
  
  // Jeremias comete erros do nível ABAIXO do aluno
  const errorLevel = Math.max(1, studentMaturityLevel - 1);
  
  return errorCatalog[concept][`level${errorLevel}`];
}
```

---

## 10. STACK TECNOLÓGICA

```javascript
stack: {
  frontend: {
    framework: "React 18",
    language: "JavaScript",
    styling: "Tailwind CSS 3.x",
    stateManagement: "React Context API",
    routing: "React Router 6",
    animations: "Framer Motion",
    audio: "Tone.js (síntese contextual)"
  },
  
  backend: {
    platform: "Supabase",
    database: "PostgreSQL 14+",
    auth: "Supabase Auth (simplificado)",
    storage: "Supabase Storage",
    realtime: "Supabase Realtime"
  },
  
  assets: {
    fonts: [
      "Atkinson Hyperlegible (Google Fonts)",
      "OpenDyslexic (local .woff2)",
      "Comic Sans MS (system)"
    ],
    icons: "Lucide React (SVG)",
    illustrations: "Emoji 96px (Jeremias)"
  },
  
  development: {
    bundler: "Vite",
    linter: "ESLint",
    formatter: "Prettier",
    versionControl: "Git + GitHub"
  },
  
  deployment: {
    hosting: "Vercel (free tier)",
    ci_cd: "GitHub Actions",
    analytics: "Plausible (LGPD-compliant)"
  }
}
```

---

## 11. SISTEMA DE FEEDBACK AUDITIVO

### 11.1 Princípio

Sons = informação, não recompensa pavloviana. Síntese em tempo real via Tone.js (não arquivos MP3).

### 11.2 Mapeamento de Sons

```javascript
sounds: {
  math: {
    number_placement_correct: {
      type: "pitched_tone",
      mapping: "logarithmic",
      // Tom varia com magnitude: 5=nota média, 500=nota aguda
    },
    
    balance_achieved: {
      type: "harmony",
      notes: ["C4", "G4"], // Quinta perfeita
      duration: "500ms"
    },
    
    pattern_recognized: {
      type: "melodic_sequence",
      // Ex: 2,4,6,8 → do-re-mi-fa
    }
  },
  
  portuguese: {
    syllable_click: {
      type: "pitched_click",
      // Sílaba tônica = tom mais alto
    },
    
    punctuation: {
      period: "descending_tone",      // Finalização
      comma: "pause_tone",            // Suspensão
      question: "rising_tone",        // Interrogação
      exclamation: "emphatic_tone"    // Ênfase
    }
  },
  
  jeremias: {
    realizes_mistake: {
      type: "discovery_chord",
      progression: ["Dm", "G", "C"], // Resolução harmônica
      duration: "800ms"
    },
    
    mood_change: {
      confused: "descending_minor",
      eureka: "ascending_major",
      grateful: "warm_chord"
    }
  }
}
```

---

## 12. FLUXOS CRÍTICOS (USER STORIES)

### 12.1 Login Aluno

```
1. Tela inicial exibe logo Dimidui + lista de nomes
2. Aluno clica em seu nome
3. Sistema carrega:
   - Preferências (fonte/tema)
   - Estado do Jeremias
   - Progresso conceitual
4. Redireciona para Dashboard Aluno
```

### 12.2 Dashboard Aluno

```
LAYOUT:
┌─────────────────────────────────────────┐
│ Header: "Olá, [Nome]" | ⚙️ Settings     │
├─────────────────────────────────────────┤
│                                         │
│ 📊 MEU PROGRESSO                        │
│ ├─ Conceitos dominados: 7/10           │
│ └─ Atividades completadas: 12          │
│                                         │
│ 🎯 ATIVIDADE DESTA SEMANA               │
│ ┌───────────────────────────────────┐   │
│ │ Divisão com Resto - Prática       │   │
│ │ Tipo: Fluência | 20min            │   │
│ │                                   │   │
│ │ [Começar] ──────────────────►     │   │
│ └───────────────────────────────────┘   │
│                                         │
│ 🤝 JEREMIAS                             │
│ ├─ Nível: 3 (Em Progresso)             │
│ ├─ Conceitos aprendidos: 5              │
│ └─ "Estou ficando bom nisso!"          │
│                                         │
│ 📚 ATIVIDADES ANTERIORES                │
│ └─ [Lista com histórico]                │
│                                         │
└─────────────────────────────────────────┘
```

### 12.3 Atividade de Fluência (Exemplo)

```
FLUXO:
1. M20 - EstadoJeremias (2min)
   ├─ Avatar 😊 + "Vamos praticar divisão!"
   └─ [Começar]

2. M1 - BateriaRápida (15min)
   ├─ Problema 1/30: "17 ÷ 5 = ?"
   ├─ Input numérico
   ├─ Feedback imediato: ✓ ou ✗ com correção
   ├─ Barra progresso: 15/30
   └─ Repete até 30 ou timeout

3. M14 - AutoAvaliação (3min)
   ├─ "Como você se sente com divisão agora?"
   ├─ [Preciso praticar mais]
   ├─ [Estou melhorando]
   └─ [Já domino]

4. CONCLUSÃO
   ├─ "Você acertou 24/30 (80%)"
   ├─ Jeremias: "Você está indo bem!"
   ├─ Atualiza concept_mastery
   ├─ Verifica desbloqueio de atividade integrada
   └─ [Voltar ao Dashboard]
```

### 12.4 Atividade de Integração (Exemplo)

```
FLUXO:
1. M9 - CompreensãoLeitora (3min)
   ├─ Texto: Problema da feira de ciências
   └─ Perguntas: Extrair informações chave

2. M5 - IdentificaOperação (2min)
   └─ "Qual operação resolve?" [Mult][Div][Adic][Sub]

3. M10 - JeremiasResolver (8min)
   ├─ Jeremias mostra raciocínio passo-a-passo
   ├─ Comete erro no passo 3
   ├─ "Clique onde Jeremias errou"
   ├─ Aluno clica passo 3
   ├─ "Qual foi o erro?"
   │   └─ [Confundiu unidade com total]
   │   └─ [Errou conta]
   │   └─ [Não leu direito]
   ├─ Aluno seleciona "Confundiu unidade com total"
   ├─ Jeremias: "Ah! Então eu preciso multiplicar DEPOIS dividir!"
   └─ Atualiza jeremias_state

4. ALUNO RESOLVE SIMILAR (7min)
   ├─ Problema análogo com números diferentes
   ├─ Sem ajuda do Jeremias
   └─ Feedback final

5. M15 - Reflexão (2min)
   ├─ "Foi fácil encontrar erro do Jeremias?"
   └─ "O que você aprendeu?"

6. CONCLUSÃO
   ├─ Jeremias evolui para nível 4
   └─ [Voltar ao Dashboard]
```

### 12.5 Dashboard Professor

```
LAYOUT:
┌─────────────────────────────────────────┐
│ Dimidui | Professor                     │
├─────────────────────────────────────────┤
│                                         │
│ 📊 VISÃO GERAL DA TURMA                 │
│ ├─ Alunos ativos: 12                   │
│ ├─ Atividade desta semana: 8/12 (67%)  │
│ └─ Tempo médio: 18min                   │
│                                         │
│ 🎯 CONCEITOS COM MAIS ERROS             │
│ ├─ Divisão com resto: 45% erro         │
│ ├─ Frações equivalentes: 38%           │
│ └─ Interpretação inferencial: 32%      │
│                                         │
│ 👥 DESEMPENHO POR ALUNO                 │
│ ┌─────────────────────────────────┐     │
│ │ Ana (5º)   | ⭐⭐⭐⭐ | 9/10    │     │
│ │ Bruno (4º) | ⭐⭐⭐   | 6/10    │     │
│ │ Carlos (6º)| ⭐⭐⭐⭐⭐| 10/10  │     │
│ └─────────────────────────────────┘     │
│                                         │
│ ➕ CRIAR NOVA ATIVIDADE                 │
│ [+ Nova Fluência] [+ Nova Integração]   │
│                                         │
│ 📁 ATIVIDADES ANTERIORES                │
│ 📊 EXPORTAR DADOS (CSV)                 │
│                                         │
└─────────────────────────────────────────┘
```

---

## 13. PRIORIZAÇÃO (MoSCoW)

### MUST HAVE (MVP - Semanas 1-3)

**Módulos:**
- [x] M1 - BateriaRápida
- [x] M5 - IdentificaOperação
- [x] M10 - JeremiasResolver
- [x] M14 - AutoAvaliação
- [x] M20 - EstadoJeremias

**Sistema:**
- [x] Login simples (selecionar nome)
- [x] Dashboard aluno básico
- [x] Runtime de atividades (JSON → UI)
- [x] Salvar progresso (Supabase)
- [x] Sistema de progressão conceitual
- [x] Estado do Jeremias (5 níveis)

**Interface:**
- [x] Editor de atividade (formulário simples)
- [x] Toggle de fonte (Atkinson ↔ OpenDyslexic)
- [x] Toggle de tema (Light ↔ Dark)

### SHOULD HAVE (Versão 2 - Semanas 4-6)

**Módulos:**
- [ ] M2 - LinhaNumérica
- [ ] M3 - FraçãoVisual
- [ ] M9 - CompreensãoLeitora
- [ ] M11 - CenárioSequencial
- [ ] M19 - BarraProgresso

**Sistema:**
- [ ] Dashboard professor (visão turma)
- [ ] Exportar dados (CSV)
- [ ] Sistema de hints progressivos
- [ ] Sons contextuais (Tone.js)

### COULD HAVE (Versão 3 - Semanas 7-10)

**Módulos:**
- [ ] M6, M7, M8 (módulos português)
- [ ] M16, M17, M18 (módulos visuais-espaciais)
- [ ] M12 - ProblemaAberto
- [ ] M13 - PrevisãoDesempenho
- [ ] M15 - ReflexãoPós

**Sistema:**
- [ ] Editor visual (drag-and-drop)
- [ ] Analytics dashboard
- [ ] Alertas professor (aluno travado)

### WON'T HAVE (Fora escopo V1)

- [ ] Modo mobile/tablet
- [ ] Multiplayer/colaboração
- [ ] IA generativa
- [ ] Reconhecimento de voz
- [ ] Gamificação social
- [ ] Modo offline completo

---

## 14. MÉTRICAS DE SUCESSO

### Adoção
- ✓ 80%+ alunos completam atividade da semana
- ✓ Professor cria 1 atividade nova/semana sem ajuda
- ✓ Taxa de abandono <15%

### Efetividade Pedagógica
- ✓ Alunos fluência+integração têm >15% mais acerto em avaliação externa
- ✓ 70%+ demonstram domínio (threshold) após 2 semanas
- ✓ Correlação autoavaliação vs real >0.6 após 1 mês

### Eficiência
- ✓ Tempo médio fluência: 18-22min
- ✓ Tempo médio integração: 18-25min
- ✓ Professor gasta <5min/semana gerenciando

### Engagement
- ✓ Alunos pedem atividade "extra" espontaneamente
- ✓ <10% reclamações sobre "atividade chata"
- ✓ Retenção semanal >90%

### Técnico
- ✓ Zero perda de dados
- ✓ <2% sessões com bugs críticos
- ✓ Carregamento <3seg

---

## 15. RISCOS & MITIGAÇÕES

| Risco | Impacto | Prob | Mitigação |
|-------|---------|------|-----------|
| Atividades repetitivas/tediosas | Alto | Média | Variar mecânicas; rotação fluência/integração |
| Professor não cria atividades | Alto | Média | Testar editor com usuário real; formulários simples |
| Bugs críticos (perda dados) | Crítico | Baixa | Backup auto 2min; teste extensivo |
| Alunos não conseguem usar | Alto | Baixa | Testar com 2-3 alunos antes de lançar |
| Downtime Supabase | Médio | Baixa | Atividade backup em papel |
| Desenvolvimento demora | Médio | Alta | MVP radical (5 módulos); versão 1 limitada |
| Jeremias é irritante | Médio | Média | Testar diálogos; permitir "modo sem narrativa" |

---

## 16. PRÓXIMOS PASSOS

### Fase 1: Setup (Semana 1)
- [ ] Criar repositório Git
- [ ] Setup Vite + React + Tailwind
- [ ] Configurar Supabase (projeto + tabelas)
- [ ] Instalar dependências (Framer Motion, Tone.js, etc)
- [ ] Setup design tokens (cores, tipografia)

### Fase 2: Core Components (Semana 2)
- [ ] Implementar M20 (EstadoJeremias)
- [ ] Implementar M1 (BateriaRápida)
- [ ] Implementar M14 (AutoAvaliação)
- [ ] Sistema de login básico
- [ ] Dashboard aluno (versão mínima)

### Fase 3: Integração & Estado (Semana 3)
- [ ] Implementar M10 (JeremiasResolver)
- [ ] Implementar M5 (IdentificaOperação)
- [ ] Sistema de progresso (concept_mastery)
- [ ] Sistema de evolução Jeremias
- [ ] Salvar/carregar estado (Supabase)

### Fase 4: Testing & Polish (Semana 4)
- [ ] Testar com 2-3 alunos reais
- [ ] Ajustes baseados em feedback
- [ ] Editor de atividade (formulário)
- [ ] Criar 2-3 atividades de teste
- [ ] Deploy Vercel (produção)

---

## APÊNDICES

### A. Glossário

**BNCC:** Base Nacional Comum Curricular  
**Fluência:** Velocidade + precisão em habilidade específica  
**Galáticos:** Modo de atividade integrada (múltiplos conceitos)  
**Jeremias:** Personagem NPC que comete erros conceituais  
**Módulo:** Componente reutilizável de atividade  
**MVP:** Minimum Viable Product  
**Threshold:** Limite mínimo de desempenho para domínio

### B. Referências Pedagógicas

- Técnica Feynman (ensinar para aprender)
- Prática Deliberada (Ericsson)
- Espaçamento de Revisão (Ebbinghaus)
- Feedback Formativo (Black & Wiliam)
- Scaffolding (Vygotsky - ZPD)

### C. Contatos & Recursos

**Repositório:** [A definir]  
**Figma:** [A definir]  
**Supabase Project:** [A definir]  
**Deploy URL:** [A definir]

---

**FIM DA ESPECIFICAÇÃO**

*Documento vivo - atualizar conforme desenvolvimento evolui*