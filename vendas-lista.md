# 💰 Página: Lista de Vendas

## 📍 Localização
- **Arquivo:** `app/vendas/page.tsx`
- **Rota:** `/vendas`
- **Componente:** `VendasPage`

## 🎯 Funcionalidade Principal
Página principal para visualização e gestão de vendas com filtros avançados, busca inteligente e resumo estatístico.

## 🔧 Funcionalidades Principais

### **Visualização**
- ✅ Lista de vendas com informações resumidas
- ✅ Exibição de status com cores diferenciadas
- ✅ Informações do cliente e valor total
- ✅ Data da venda e forma de pagamento
- ✅ Resumo estatístico das vendas

### **Filtros e Busca**
- ✅ Filtro por status (todas, concluída, pendente, cancelada)
- ✅ Filtro por período (7 dias, 30 dias, 90 dias)
- ✅ Busca por cliente, código ou observações
- ✅ Ordenação dinâmica por múltiplos campos

### **Informações Detalhadas**
- ✅ Valor total formatado em moeda
- ✅ Forma de pagamento compacta
- ✅ Status visual com badges
- ✅ Data formatada em português
- ✅ Links para detalhes da venda

## 📊 Estados e Dados

### **Estados Principais**
```typescript
// Dados principais
const [vendas, setVendas] = useState<Venda[]>([]);
const [loading, setLoading] = useState(true);

// Filtros
const [filtroStatus, setFiltroStatus] = useState<string>('todas');
const [filtroPeriodo, setFiltroPeriodo] = useState<string>('7dias');
const [termoBusca, setTermoBusca] = useState('');

// Ordenação dinâmica
const [ordenacao, setOrdenacao] = useState<{
  campo: string | null;
  direcao: 'asc' | 'desc';
}>({
  campo: null,
  direcao: 'asc'
});
```

### **Interfaces de Dados**
```typescript
interface Venda {
  id: string;
  clienteId: string;
  clienteNome: string;
  valorTotal: number;
  status: 'concluida' | 'pendente' | 'cancelada';
  formaPagamento: string;
  parcelas?: number;
  dataVenda: Date;
  observacao?: string;
  itens: ItemVenda[];
}

interface ItemVenda {
  id: string;
  produtoId: string;
  produtoNome: string;
  quantidade: number;
  precoUnitario: number;
  subtotal: number;
}
```

## 🔗 Integrações

### **Serviços**
- `venda.service.ts` - Busca e gestão de vendas

### **Componentes Específicos**
- `VendasResumo` - Resumo estatístico das vendas
- `FormasPagamentoCompacto` - Exibição compacta das formas de pagamento

### **Bibliotecas**
- `date-fns` - Formatação de datas em português
- `next/link` - Navegação entre páginas

## 🎨 Interface

### **Layout Principal**
```
┌─────────────────────────────────────────┐
│ Cabeçalho + Botão "Nova Venda"          │
├─────────────────────────────────────────┤
│ Filtros (Status, Período, Busca)        │
├─────────────────────────────────────────┤
│ Resumo Estatístico (VendasResumo)       │
├─────────────────────────────────────────┤
│ Lista de Vendas                          │
│ ┌────────────────────────────────────┐   │
│ │ #001 | Cliente | Status | Valor   │   │
│ │ Data | Pagamento | Ações          │   │
│ └────────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

### **Informações por Venda**
1. **Número/ID** - Identificador da venda
2. **Cliente** - Nome do cliente
3. **Status** - Badge colorido (concluída/pendente/cancelada)
4. **Valor Total** - Formatado em R$
5. **Data** - Formatada em português (dd/MM/yyyy)
6. **Forma de Pagamento** - Compacta com ícones
7. **Ações** - Ver detalhes, editar, etc.

## 🔄 Fluxos de Dados

### **Fluxo de Carregamento**
```
1. Componente monta
2. Aplica filtros padrão (todas + 7 dias)
3. Busca vendas no serviço
4. Processa e filtra dados
5. Aplica ordenação se definida
6. Renderiza lista e resumo
```

### **Fluxo de Filtros**
```
1. Usuário altera filtro
2. Estado local atualizado
3. useEffect detecta mudança
4. Nova busca executada
5. Dados filtrados e processados
6. Lista e resumo atualizados
```

### **Fluxo de Busca**
```
1. Usuário digita termo
2. Estado termoBusca atualizado
3. Filtro aplicado aos dados
4. Lista filtrada em tempo real
5. Resumo recalculado
```

## 🎯 Funcionalidades de Filtro

### **Filtro por Status**
- **Todas:** Exibe todas as vendas
- **Concluída:** Vendas finalizadas
- **Pendente:** Vendas em andamento
- **Cancelada:** Vendas canceladas

### **Filtro por Período**
- **7 dias:** Vendas da última semana
- **30 dias:** Vendas do último mês
- **90 dias:** Vendas do último trimestre

### **Busca Inteligente**
- Busca por nome do cliente
- Busca por ID/código da venda
- Busca em observações
- Case-insensitive

## 🔧 Ordenação Dinâmica

### **Campos Ordenáveis**
- **Data:** Mais recente/antiga primeiro
- **Valor:** Maior/menor valor
- **Cliente:** Ordem alfabética
- **Status:** Por prioridade

### **Implementação**
```typescript
const handleOrdenar = (campo: string) => {
  const novaDirecao = ordenacao.campo === campo && ordenacao.direcao === 'asc' 
    ? 'desc' 
    : 'asc';
  
  setOrdenacao({ campo, direcao: novaDirecao });
};
```

## 🎨 Estados Visuais

### **Status com Cores**
- **Concluída:** Verde (✅ finalizada)
- **Pendente:** Amarelo (⏳ em andamento)
- **Cancelada:** Vermelho (❌ cancelada)

### **Formatação de Valores**
- **Moeda:** R$ 1.234,56 (padrão brasileiro)
- **Data:** 15/03/2024 (dd/MM/yyyy)
- **Percentuais:** 10,5% (quando aplicável)

## 🧪 Casos de Teste

### **Funcionalidades Críticas**
1. **Carregamento inicial** - Deve mostrar vendas dos últimos 7 dias
2. **Filtro por status** - Deve filtrar corretamente por cada status
3. **Busca por cliente** - Deve encontrar vendas do cliente específico
4. **Ordenação por valor** - Deve ordenar do maior para menor e vice-versa
5. **Navegação para detalhes** - Links devem abrir página correta

### **Cenários de Edge Case**
1. **Nenhuma venda encontrada** - Deve exibir mensagem apropriada
2. **Vendas sem cliente** - Deve tratar dados incompletos
3. **Valores zerados** - Deve exibir corretamente
4. **Datas inválidas** - Deve validar e tratar erros

## 📈 Performance

### **Otimizações Implementadas**
- ✅ Filtro padrão de 7 dias reduz carga inicial
- ✅ Busca em memória (não servidor)
- ✅ Componentes separados para melhor organização
- ✅ Formatação de data otimizada

### **Considerações**
- Lista pode crescer com filtros amplos
- Busca em múltiplos campos pode impactar performance
- Resumo estatístico recalculado a cada filtro

## 🔧 Integrações Específicas

### **Resumo Estatístico (VendasResumo)**
- Total de vendas no período
- Valor total vendido
- Ticket médio
- Comparação com período anterior

### **Formas de Pagamento Compacto**
- Ícones para cada forma de pagamento
- Informação de parcelas quando aplicável
- Layout compacto para tabela

## 🎯 Navegação e Ações

### **Ações Disponíveis**
- **Ver Detalhes:** Link para `/vendas/[id]`
- **Nova Venda:** Botão para `/vendas/nova`
- **Editar:** (se implementado)
- **Cancelar:** (se implementado)

### **Navegação Contextual**
- Breadcrumb para localização
- Links relacionados (cliente, produtos)
- Navegação entre páginas relacionadas

## 🐛 Problemas Conhecidos

### **Melhorias Potenciais**
- Implementar paginação para listas grandes
- Adicionar filtro por forma de pagamento
- Implementar busca com debounce
- Adicionar exportação de vendas
- Melhorar responsividade em dispositivos móveis
- Implementar filtro por faixa de valor
- Adicionar gráficos de vendas por período