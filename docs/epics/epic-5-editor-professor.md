# Epic 5: Editor de Atividades (Professor)

**ID:** EPIC-005
**Status:** Planejado
**Prioridade:** Should Have (MVP+)
**Estimativa:** 2 semanas

## Objetivo

Criar ferramenta intuitiva para professores criarem e gerenciarem atividades combinando módulos educacionais, garantindo autonomia pedagógica sem dependência técnica.

## Valor de Negócio

Permite personalização do conteúdo pelo professor, adaptando-se às necessidades específicas da turma. Reduz dependência de desenvolvedores para criação de novas atividades, escalando o impacto pedagógico.

## User Stories Incluídas

- **US-015:** Interface de criação de atividades
- **US-016:** Configuração de módulos via formulário
- **US-017:** Preview da atividade criada
- **US-018:** Gestão de atividades existentes
- **US-019:** Dashboard do professor (visão turma)

## Funcionalidades Principais

### 1. Criador de Atividades
```
FLUXO:
1. Selecionar tipo: [Fluência] [Integração]
2. Escolher módulos da biblioteca
3. Configurar cada módulo (JSON visual)
4. Preview da atividade
5. Publicar para turma
```

### 2. Dashboard Professor
```
LAYOUT:
┌─────────────────────────────────────────┐
│ Dimidui | Professor                     │
├─────────────────────────────────────────┤
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
│ │ Ana (5º)   | ⭐⭐⭐⭐ | 9/10    │
│ │ Bruno (4º) | ⭐⭐⭐   | 6/10    │
│                                         │
│ ➕ CRIAR NOVA ATIVIDADE                 │
│ [+ Nova Fluência] [+ Nova Integração]   │
│                                         │
│ 📊 EXPORTAR DADOS (CSV)                 │
└─────────────────────────────────────────┘
```

### 3. Configurador de Módulos

#### Exemplo: M1 - BateriaRápida
```
┌─────────────────────────────────────┐
│ CONFIGURAR: Bateria Rápida         │
├─────────────────────────────────────┤
│ Tipo de Operação:                  │
│ ◉ Divisão  ○ Multiplicação         │
│ ○ Adição   ○ Subtração             │
│                                     │
│ Alcance Numérico:                  │
│ Min: [1 ] Max: [100]               │
│                                     │
│ Quantidade de Problemas: [30]      │
│                                     │
│ Tempo Limite (opcional): [  ] min  │
│                                     │
│ [Preview] [Salvar] [Cancelar]       │
└─────────────────────────────────────┘
```

## Critérios de Aceitação

### Editor de Atividades
- [ ] Interface drag-and-drop para ordenar módulos
- [ ] Formulários de configuração por tipo de módulo
- [ ] Validação de configurações obrigatórias
- [ ] Preview funcional antes da publicação
- [ ] Templates predefinidos para início rápido

### Dashboard Professor
- [ ] Visão consolidada do progresso da turma
- [ ] Identificação de conceitos com mais dificuldade
- [ ] Perfil individual de cada aluno
- [ ] Exportação de dados para CSV
- [ ] Histórico de atividades criadas

### Gestão de Atividades
- [ ] Lista de atividades criadas
- [ ] Duplicar atividade existente
- [ ] Editar atividade não iniciada pelos alunos
- [ ] Arquivar atividades antigas
- [ ] Atribuir atividade para semana específica

## Templates Predefinidos

### Template: Fluência Matemática
```yaml
name: "Prática de Divisão - Semana X"
type: "fluencia"
duration: 20
modules:
  - type: "M20" # EstadoJeremias
    config: {context: "start", duration: 2}
  - type: "M1"  # BateriaRápida
    config: {operation: "division", min: 10, max: 100, count: 30}
  - type: "M14" # AutoAvaliação
    config: {concepts: ["divisão"]}
```

### Template: Integração Narrativa
```yaml
name: "Jeremias Aprende Frações"
type: "integracao"
duration: 20
modules:
  - type: "M9"  # CompreensãoLeitora
    config: {text: "problema_feira", duration: 3}
  - type: "M10" # JeremiasResolver
    config: {concept: "frações", error: "compara_numerador"}
  - type: "M15" # ReflexãoPós
    config: {questions: ["dificuldade", "aprendizado"]}
```

## Dependências

- **Pré-requisitos:** Epic 1 (módulos) e Epic 3 (plataforma) completos
- **Bloqueia:** Adoção em larga escala

## Definição de Pronto

- [ ] Professor cria atividade nova em <10min
- [ ] Zero erros na configuração de módulos
- [ ] Dashboard carrega dados de 30 alunos <5s
- [ ] Exportação CSV com dados pedagógicos úteis
- [ ] Documentação de uso para professores
- [ ] Testes com 2-3 professores reais

## Riscos

| Risco | Probabilidade | Impacto | Mitigação |
|-------|---------------|---------|-----------|
| Interface muito complexa | Alta | Alto | Testes com usuários, simplificação |
| Professor não usa editor | Média | Alto | Templates robustos, treinamento |
| Performance com muitos dados | Baixa | Médio | Paginação, otimização queries |

## Métricas de Sucesso

- **Adoção:** >80% professores criam ≥1 atividade/semana
- **Usabilidade:** Criação de atividade <10min
- **Eficácia:** Atividades criadas têm >70% completion rate
- **Suporte:** <5min/semana de suporte técnico por professor

## Funcionalidades Futuras (V2+)

- [ ] Editor visual drag-and-drop
- [ ] Biblioteca de atividades compartilhadas
- [ ] Analytics avançados de aprendizado
- [ ] Alertas automáticos (aluno com dificuldade)
- [ ] Integração com sistemas escolares (Google Classroom)