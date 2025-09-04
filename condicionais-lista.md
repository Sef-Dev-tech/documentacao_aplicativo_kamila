# 🔄 Página: Lista de Condicionais

## 📍 Localização
- **Arquivo:** `app/condicionais/page.tsx`
- **Rota:** `/condicionais`
- **Componente:** `CondicionaisPage`

## 🎯 Funcionalidade Principal
Página principal para gestão e visualização de condicionais com filtros inteligentes, busca avançada e controles de ordenação.

## 🔧 Funcionalidades Principais

### **Visualização**
- ✅ Lista de condicionais com informações resumidas
- ✅ Exibição de status com cores diferenciadas
- ✅ Informações do cliente e valor total
- ✅ Data de criação e prazo
- ✅ Indicadores visuais de status

### **Filtros Inteligentes**
- ✅ Filtro por status (todas, aberta, finalizada, expirada)
- ✅ Filtro por período (7 dias, 30 dias, 90 dias)
- ✅ Busca por código, cliente ou observações
- ✅ Filtros padrão otimizados (abertas + 30 dias)

### **Ordenação Dinâmica**
- ✅ Ordenação por múltiplos campos
- ✅ Direção ascendente/descendente
- ✅ Indicadores visuais de ordenação ativa
- ✅ Ordenação por prioridade de status

### **Navegação**
- ✅ Links para detalhes da condicional
- ✅ Botão para nova condicional
- ✅ Navegação contextual

## 📊 Estados e Dados

### **Estados Principais**
```typescript
// Dados principais
const [condicionais, setCondicionais] = useState<Condicional[]>([]);
const [loading, setLoading] = useState(true);

// Filtros com valores padrão otimizados
const [filtroStatus, setFiltroStatus] = useState<StatusFiltro>('aberta');
const [filtroPeriodo, setFiltroPeriodo] = useState<PeriodoFiltro>('30dias');
const [termoBusca, setTermoBusca] = useState<string>('');

// Ordenação dinâmica
const [ordenacao, setOrdenacao] = useState<{
  campo: string | null;
  direcao: 'asc' | 'desc';
}>({
  campo: null,
  direcao: 'asc'
});
```

### **Tipos de Dados**
```typescript
type StatusFiltro = 'todas' | 'aberta' | 'finalizada' | 'expirada';
type PeriodoFiltro = '7dias' | '30dias' | '90dias';

interface Condicional {
  id: string;
  codigo: string;
  clienteId: string;
  clienteNome: string;
  dataInicio: Date;
  dataPrazo: Date;
  status: 'aberta' | 'finalizada' | 'cancelada' | 'parcialmente_devolvida';
  valorTotal: number;
  itens: ItemCondicional[];
  observacoes?: string;
}
```

## 🔗 Integrações

### **Serviços**
- `condicional.service.ts` - Busca e gestão de condicionais

### **Componentes Específicos**
- `CondicionaisFiltros` - Controles de filtro
- `CondicionaisResumo` - Resumo estatístico
- `CondicionaisList` - Lista principal de condicionais

### **Bibliotecas**
- `next/link` - Navegação entre páginas
- React hooks padrão para estado

## 🎨 Interface

### **Layout Principal**
```
┌─────────────────────────────────────────┐
│ Cabeçalho + Botão "Nova Condicional"    │
├─────────────────────────────────────────┤
│ Filtros (Status, Período, Busca)        │
├─────────────────────────────────────────┤
│ Resumo Estatístico                       │
├─────────────────────────────────────────┤
│ Lista de Condicionais                    │
│ ┌────────────────────────────────────┐   │
│ │ C001 | Cliente | Status | Valor   │   │
│ │ Data | Prazo   | Ações           │   │
│ └────────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

### **Componentes da Lista**
1. **Código da Condicional** - Identificador único
2. **Cliente** - Nome do cliente
3. **Status** - Badge colorido com status atual
4. **Valor Total** - Valor formatado em moeda
5. **Datas** - Criação e prazo
6. **Ações** - Links para visualizar/editar

## 🔄 Fluxos de Dados

### **Fluxo de Carregamento**
```
1. Componente monta
2. Aplica filtros padrão (abertas + 30 dias)
3. Busca condicionais no serviço
4. Processa e filtra dados
5. Aplica ordenação
6. Renderiza lista
```

### **Fluxo de Filtros**
```
1. Usuário altera filtro
2. Estado local atualizado
3. useEffect detecta mudança
4. Nova busca executada
5. Dados filtrados e ordenados
6. Lista atualizada
```

### **Fluxo de Ordenação**
```
1. Usuário clica em cabeçalho
2. Determina nova direção
3. Atualiza estado de ordenação
4. Aplica ordenação aos dados
5. Re-renderiza lista ordenada
```

## 🎯 Filtros Padrão Otimizados

### **Configuração Padrão**
```typescript
const FILTROS_PADRAO = {
  STATUS: 'aberta',    // Foco nas condicionais ativas
  PERIODO: '30dias',   // Período relevante para gestão
  BUSCA: ''           // Sem busca inicial
} as const;
```

### **Justificativa**
- **Status "aberta":** Usuários geralmente querem ver condicionais que precisam de ação
- **Período "30 dias":** Balanceio entre relevância e completude
- **Sem busca:** Permite visão geral inicial

## 🔧 Funcionalidades Avançadas

### **Ordenação por Prioridade de Status**
```typescript
const prioridade: Record<string, number> = { 
  aberta: 5,           // Maior prioridade
  expirada: 4,         // Segunda prioridade
  finalizada: 3,       // Terceira prioridade
  cancelada: 2,        // Quarta prioridade
  parcialmente_devolvida: 1  // Menor prioridade
};
```

### **Busca Inteligente**
- Busca por código da condicional
- Busca por nome do cliente
- Busca em observações
- Busca case-insensitive

### **Filtros de Período**
- **7 dias:** Condicionais muito recentes
- **30 dias:** Período padrão de gestão
- **90 dias:** Visão histórica ampla

## 🧪 Casos de Teste

### **Funcionalidades Críticas**
1. **Filtro padrão** - Deve carregar condicionais abertas dos últimos 30 dias
2. **Busca por código** - Deve encontrar condicional específica
3. **Ordenação por status** - Deve priorizar condicionais abertas
4. **Filtro por período** - Deve respeitar datas corretamente
5. **Navegação para detalhes** - Links devem funcionar corretamente

### **Cenários de Edge Case**
1. **Nenhuma condicional encontrada** - Deve exibir mensagem apropriada
2. **Condicionais sem cliente** - Deve tratar dados incompletos
3. **Datas inválidas** - Deve validar e tratar erros
4. **Erro de rede** - Deve exibir erro e permitir retry

## 📈 Performance

### **Otimizações**
- ✅ Filtros padrão reduzem carga inicial
- ✅ Busca com debounce (se implementado)
- ✅ Ordenação em memória (não no servidor)
- ✅ Componentes separados para melhor organização

### **Considerações**
- Lista pode crescer com filtros amplos
- Ordenação complexa pode impactar performance
- Busca em múltiplos campos pode ser lenta

## 🎨 Estados Visuais

### **Status com Cores**
- **Aberta:** Verde (ativa, precisa atenção)
- **Finalizada:** Azul (concluída)
- **Expirada:** Vermelho (urgente)
- **Cancelada:** Cinza (inativa)
- **Parcialmente Devolvida:** Laranja (ação parcial)

### **Indicadores de Ordenação**
- Setas visuais nos cabeçalhos
- Destaque do campo ativo
- Direção clara (asc/desc)

## 🔧 Configurações

### **Filtros**
- **Status padrão:** "aberta"
- **Período padrão:** "30dias"
- **Busca:** Sem debounce (implementar se necessário)

### **Ordenação**
- **Campo padrão:** null (ordem natural)
- **Direção padrão:** "asc"
- **Prioridade de status:** Implementada

## 🐛 Problemas Conhecidos

### **Potenciais Melhorias**
- Implementar paginação para listas grandes
- Adicionar filtro por cliente específico
- Implementar busca com debounce
- Adicionar exportação de condicionais
- Melhorar responsividade em mobile