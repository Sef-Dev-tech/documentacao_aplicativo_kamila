# 📊 Página: Dashboard Principal

## 📍 Localização
- **Arquivo:** `app/dashboard/page.tsx`
- **Rota:** `/dashboard`
- **Componente:** `DashboardPage`

## 🎯 Funcionalidade Principal
Dashboard principal da aplicação com visão geral de vendas, condicionais, estoque e métricas de negócio em tempo real.

## 🔧 Funcionalidades Principais

### **Métricas de Vendas**
- ✅ Total de vendas do dia/período
- ✅ Valor total vendido
- ✅ Quantidade de produtos vendidos
- ✅ Ticket médio
- ✅ Status das vendas (concluídas/pendentes)

### **Condicionais em Destaque**
- ✅ Total de condicionais abertas
- ✅ Valor total em condicionais
- ✅ Condicionais atrasadas (prazo vencido)
- ✅ Quantidade de produtos em condicional

### **Controle de Estoque**
- ✅ Total de produtos cadastrados
- ✅ Total de itens em estoque
- ✅ Valor total do estoque
- ✅ Produtos com estoque baixo

### **Notificações e Alertas**
- ✅ Aniversários do dia (AniversarioSimples)
- ✅ Condicionais vencidas
- ✅ Estoque baixo
- ✅ Vendas pendentes

## 📊 Estados e Dados

### **Estados Principais**
```typescript
// Dados das views otimizadas
const [vendas, setVendas] = useState<Venda[]>([]);
const [condicionais, setCondicionais] = useState<Condicional[]>([]);
const [produtos, setProdutos] = useState<Produto[]>([]);
const [loading, setLoading] = useState(true);
```

### **Interfaces de Dados**
```typescript
interface Venda {
  venda_id: string;
  data_venda?: string;
  valor_total: number;
  cliente_nome?: string;
  status?: string;
  quantidade_produtos?: number;
  forma_pagamento?: string;
}

interface Condicional {
  condicional_id: string;
  valor_total: number | null;
  status: string;
  cliente_nome?: string;
  codigo?: string;
  data_prazo?: string;
  atrasada?: boolean;
  total_itens?: number;
}

interface EstoqueView {
  total_produtos: number;
  total_itens: number;
  valor_total: string | number;
}
```

## 🔗 Integrações

### **Hooks Utilizados**
- `useSupabaseSWR` - Cache e sincronização de dados em tempo real
- `useEffect` - Carregamento inicial e atualizações
- `useMemo` - Cálculos otimizados de métricas

### **Componentes**
- `AniversarioSimples` - Notificações de aniversário
- Cards de métricas customizados
- Gráficos e indicadores visuais

### **Serviços/Views**
- Views otimizadas do Supabase para performance
- Queries diretas para dados em tempo real
- Cache inteligente com SWR

## 🎨 Interface

### **Layout em Grid**
```
┌─────────────────────────────────────────┐
│ Cabeçalho + Notificações                │
├─────────────────────────────────────────┤
│ ┌─────────┐ ┌─────────┐ ┌─────────┐     │
│ │ Vendas  │ │Condicio.│ │ Estoque │     │
│ │ Hoje    │ │ Abertas │ │ Total   │     │
│ └─────────┘ └─────────┘ └─────────┘     │
├─────────────────────────────────────────┤
│ ┌─────────────────┐ ┌─────────────────┐ │
│ │ Gráfico Vendas  │ │ Alertas/Tarefas │ │
│ │                 │ │                 │ │
│ └─────────────────┘ └─────────────────┘ │
├─────────────────────────────────────────┤
│ ┌─────────────────────────────────────┐ │
│ │ Atividades Recentes                 │ │
│ └─────────────────────────────────────┘ │
└─────────────────────────────────────────┘
```

### **Cards de Métricas**
1. **Vendas do Dia**
   - Valor total
   - Quantidade de vendas
   - Comparação com ontem
   - Status (concluídas/pendentes)

2. **Condicionais**
   - Total abertas
   - Valor em condicionais
   - Condicionais vencidas
   - Ação necessária

3. **Estoque**
   - Total de produtos
   - Valor do estoque
   - Produtos com estoque baixo
   - Alertas de reposição

## 🔄 Fluxos de Dados

### **Carregamento Inicial**
```
1. Componente monta
2. useSupabaseSWR inicia queries paralelas
3. Carrega dados de vendas, condicionais e estoque
4. Processa métricas e cálculos
5. Renderiza dashboard com dados
6. Configura atualizações em tempo real
```

### **Atualizações em Tempo Real**
```
1. SWR detecta mudanças no Supabase
2. Revalida dados automaticamente
3. Atualiza métricas calculadas
4. Re-renderiza componentes afetados
5. Mantém interface sincronizada
```

### **Cálculo de Métricas**
```
1. Dados brutos recebidos
2. useMemo processa cálculos
3. Agrupa por período/categoria
4. Calcula percentuais e comparações
5. Formata para exibição
```

## 📈 Métricas Calculadas

### **Vendas**
- **Total do Dia:** Soma de vendas de hoje
- **Ticket Médio:** Valor total ÷ número de vendas
- **Crescimento:** Comparação com período anterior
- **Meta:** Progresso em relação à meta mensal

### **Condicionais**
- **Valor Total:** Soma de condicionais abertas
- **Quantidade:** Número de condicionais ativas
- **Vencidas:** Condicionais com prazo expirado
- **Conversão:** Taxa de conversão para vendas

### **Estoque**
- **Valor Total:** Soma do valor de todos os produtos
- **Giro:** Velocidade de rotação do estoque
- **Baixo Estoque:** Produtos abaixo do mínimo
- **Categorias:** Distribuição por categoria

## 🚨 Alertas e Notificações

### **Tipos de Alertas**
1. **Críticos (Vermelho)**
   - Condicionais vencidas há mais de 7 dias
   - Produtos com estoque zerado
   - Vendas pendentes há mais de 24h

2. **Atenção (Amarelo)**
   - Condicionais vencendo hoje
   - Estoque baixo (< 5 unidades)
   - Metas não atingidas

3. **Informativos (Azul)**
   - Aniversários do dia
   - Novos produtos cadastrados
   - Relatórios disponíveis

### **Componente AniversarioSimples**
- Exibe aniversariantes do dia
- Notificação discreta no topo
- Link para lista completa de clientes
- Integração com sistema de notificações

## 🔧 Performance e Otimização

### **Estratégias Implementadas**
- ✅ **Views otimizadas** no Supabase para queries rápidas
- ✅ **SWR Cache** para reduzir requisições
- ✅ **useMemo** para cálculos pesados
- ✅ **Componentes lazy** para carregamento sob demanda
- ✅ **Queries paralelas** para carregamento simultâneo

### **Métricas de Performance**
- **Tempo de carregamento inicial:** ~1-2s
- **Atualização em tempo real:** ~500ms
- **Cache hit rate:** ~80%
- **Queries simultâneas:** 3-4 paralelas

## 🧪 Casos de Teste

### **Funcionalidades Críticas**
1. **Carregamento inicial** - Deve carregar todas as métricas
2. **Dados em tempo real** - Deve atualizar automaticamente
3. **Cálculos corretos** - Métricas devem ser precisas
4. **Alertas funcionais** - Notificações devem aparecer
5. **Navegação** - Links devem funcionar corretamente

### **Cenários de Edge Case**
1. **Sem dados** - Deve exibir estado vazio apropriado
2. **Erro de rede** - Deve mostrar erro e retry
3. **Dados inconsistentes** - Deve validar e tratar
4. **Performance lenta** - Deve mostrar loading states

## 🎯 Navegação Rápida

### **Links Principais**
- **Nova Venda:** Botão de ação rápida
- **Nova Condicional:** Acesso direto
- **Produtos:** Link para gestão de estoque
- **Relatórios:** Acesso a relatórios detalhados

### **Ações Contextuais**
- Clique em métrica → página detalhada
- Alertas → ação direta para resolver
- Gráficos → drill-down para detalhes

## 🔧 Configurações

### **Períodos de Análise**
- **Padrão:** Dia atual
- **Comparação:** Dia anterior
- **Tendência:** Últimos 7 dias
- **Meta:** Mês atual

### **Atualizações**
- **Frequência:** A cada 30 segundos (SWR)
- **Manual:** Botão de refresh
- **Automática:** Ao focar na aba

## 🐛 Problemas Conhecidos

### **Limitações Atuais**
- Dashboard pode ficar lento com muitos dados
- Alguns cálculos são feitos no frontend
- Cache pode ficar desatualizado em cenários específicos

### **Melhorias Planejadas**
- Implementar gráficos interativos
- Adicionar filtros de período
- Melhorar responsividade mobile
- Implementar dashboard personalizável
- Adicionar mais métricas de negócio