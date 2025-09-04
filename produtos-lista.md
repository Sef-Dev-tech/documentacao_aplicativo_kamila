# 📦 Página: Lista de Produtos

## 📍 Localização
- **Arquivo:** `app/produtos/page.tsx`
- **Rota:** `/produtos`
- **Componente:** `ProdutosPaginadaPage`

## 🎯 Funcionalidade Principal
Página principal para gestão e visualização de produtos com funcionalidades avançadas de busca, filtros, paginação e exportação.

## 🔧 Funcionalidades Principais

### **Visualização**
- ✅ Lista paginada de produtos (100 itens por página padrão)
- ✅ Exibição em formato de tabela responsiva
- ✅ Indicadores visuais para produtos em condicional
- ✅ Ícones de categoria personalizados
- ✅ Status de estoque com cores (baixo/médio/alto)

### **Busca e Filtros**
- ✅ Busca inteligente por nome, código de barras ou preço
- ✅ Filtro por categoria (com subcategorias dinâmicas)
- ✅ Filtro por data de criação (hoje, ontem, últimos 7 dias, específica)
- ✅ Filtro por status de condicional (em condicional, sem condicional, prazo vencido)
- ✅ Ordenação por múltiplos campos (nome, preço, estoque, data)
- ✅ Indicadores visuais de filtros ativos
- ✅ Função "Limpar Filtros"

### **Seleção e Exportação**
- ✅ Seleção múltipla de produtos (checkbox)
- ✅ Seleção de todos os produtos da página
- ✅ Exportação para Excel dos produtos selecionados
- ✅ Geração de registros baseada no estoque
- ✅ Validação de produtos com estoque antes da exportação
- ✅ Feedback detalhado do processo de exportação

### **Navegação**
- ✅ Paginação inteligente (resolve limite de 1000 registros)
- ✅ Navegação entre páginas
- ✅ Alteração de itens por página
- ✅ Links para detalhes do produto

## 📊 Estados e Dados

### **Estados Principais (via Hooks)**
```typescript
// Hook useProdutosPaginados
const {
  produtos,           // Array de produtos da página atual
  carregando,         // Estado de carregamento
  erro,              // Mensagens de erro
  paginaAtual,       // Página atual
  totalPaginas,      // Total de páginas
  total,             // Total de produtos
  filtros,           // Filtros ativos
  // ... funções de controle
} = useProdutosPaginados({
  itensPorPaginaInicial: 100,
  autoCarregar: true,
  incluirCondicionais: true
});
```

### **Estados Locais**
```typescript
// Interface local
const [categorias, setCategorias] = useState<string[]>(['Todas']);
const [produtosSelecionados, setProdutosSelecionados] = useState<Set<string>>(new Set());
const [cacheProdutosSelecionados, setCacheProdutosSelecionados] = useState<Map<string, ProdutoComCategoria>>(new Map());
const [exportandoExcel, setExportandoExcel] = useState(false);

// Filtros de interface
const [termoBuscaLocal, setTermoBuscaLocal] = useState('');
const [categoriaSelecionada, setCategoriaSelecionada] = useState('Todas');
const [subcategoriaSelecionada, setSubcategoriaSelecionada] = useState('Todas');
const [filtroData, setFiltroData] = useState('todos');
const [filtroCondicional, setFiltroCondicional] = useState<'todos' | 'em_condicional' | 'sem_condicional' | 'prazo_vencido'>('todos');
```

## 🔗 Integrações

### **Hooks Utilizados**
- `useProdutosPaginados` - Gestão principal de produtos e paginação
- `useCategoriaSubcategoria` - Gestão de categorias e subcategorias

### **Serviços**
- `produto.service.ts` - Busca e gestão de produtos
- `categoria.service.ts` - Gestão de categorias (via hook)

### **Componentes**
- `IndicadorCondicional` - Exibe status de produtos em condicional
- `Paginacao` - Controles de navegação entre páginas
- `HeaderOrdenavel` - Cabeçalhos de tabela com ordenação

### **Bibliotecas Externas**
- `xlsx` - Exportação para Excel
- `next/link` - Navegação entre páginas

## 🎨 Interface

### **Layout Principal**
```
┌─────────────────────────────────────────┐
│ Cabeçalho + Botões de Ação              │
├─────────────────────────────────────────┤
│ Filtros (Busca, Categoria, Data, etc.)  │
├─────────────────────────────────────────┤
│ Indicadores de Filtros Ativos           │
├─────────────────────────────────────────┤
│ Controles de Seleção + Exportação       │
├─────────────────────────────────────────┤
│ Tabela de Produtos                       │
│ ┌─┬──────┬─────────┬───────┬────────┬──┐ │
│ │☐│Código│ Produto │ Preço │Estoque │..│ │
│ └─┴──────┴─────────┴───────┴────────┴──┘ │
├─────────────────────────────────────────┤
│ Paginação                               │
└─────────────────────────────────────────┘
```

### **Colunas da Tabela**
1. **Seleção** - Checkbox para seleção múltipla
2. **Código de Barras** - Código do produto (ordenável)
3. **Produto** - Nome e descrição com ícone
4. **Preço de Venda** - Valor formatado (ordenável)
5. **Estoque** - Quantidade com cores de status
6. **Em Condicional** - Indicador visual de condicionais
7. **Ações** - Link para detalhes

## 🔄 Fluxos de Dados

### **Fluxo de Carregamento**
```
1. Componente monta
2. useProdutosPaginados inicia carregamento automático
3. Busca produtos com informações de condicional
4. Aplica filtros se houver
5. Retorna dados paginados
6. Renderiza tabela
```

### **Fluxo de Filtros**
```
1. Usuário altera filtro
2. Estado local atualizado
3. Hook sincroniza com backend
4. Nova busca executada
5. Resultados atualizados
6. Volta para página 1
```

### **Fluxo de Exportação**
```
1. Usuário seleciona produtos
2. Clica em "Exportar Excel"
3. Valida produtos selecionados
4. Filtra produtos com estoque
5. Gera registros baseados no estoque
6. Cria arquivo Excel
7. Download automático
8. Limpa seleção
```

## 🧪 Casos de Teste Importantes

### **Funcionalidades Críticas**
1. **Busca por código de barras específico** - Deve encontrar produto exato
2. **Filtro "Em Condicional"** - Deve mostrar apenas produtos em condicionais abertas
3. **Exportação com estoque zero** - Deve alertar e excluir produtos sem estoque
4. **Paginação com filtros** - Deve manter filtros ao navegar entre páginas
5. **Seleção múltipla** - Deve manter seleção ao mudar de página

### **Cenários de Edge Case**
1. **Mais de 1000 produtos** - Paginação inteligente deve funcionar
2. **Produtos sem categoria** - Deve exibir "Sem categoria"
3. **Busca sem resultados** - Deve exibir mensagem apropriada
4. **Erro de rede** - Deve exibir erro e botão de retry

## 🔧 Configurações

### **Paginação**
- **Itens por página padrão:** 100
- **Opções disponíveis:** 50, 100, 200, 500
- **Estratégia:** Inteligente (paginação rápida sem filtros, busca completa com filtros)

### **Filtros**
- **Busca:** Debounce de 300ms
- **Categorias:** Carregamento dinâmico
- **Subcategorias:** Dependente da categoria selecionada

### **Exportação**
- **Formato:** XLSX
- **Estrutura:** Nome, Preço, Código de Barras
- **Registros:** Baseado na quantidade em estoque
- **Validação:** Apenas produtos com estoque > 0

## 📈 Performance

### **Otimizações Implementadas**
- ✅ Paginação inteligente (evita carregar todos os produtos)
- ✅ Debounce na busca (reduz requisições)
- ✅ Cache de produtos selecionados
- ✅ Memoização de componentes pesados
- ✅ Carregamento lazy de subcategorias

### **Métricas**
- **Tempo de carregamento inicial:** ~2-3s (100 produtos)
- **Tempo de busca:** ~500ms
- **Tempo de exportação:** ~1-2s (dependendo da quantidade)

## 🐛 Problemas Conhecidos

### **Resolvidos**
- ✅ Limite de 1000 registros do Supabase
- ✅ Produto em condicional não aparecia na coluna
- ✅ Filtros não funcionavam em toda a base
- ✅ Exportação não validava estoque

### **Monitoramento**
- Cache de produtos selecionados pode crescer muito
- Performance com muitos filtros ativos
- Sincronização entre filtros de interface e backend