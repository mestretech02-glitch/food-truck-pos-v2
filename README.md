# 🍔 Food Truck POS - Sistema de Ponto de Venda

Um sistema moderno e responsivo de Ponto de Venda (POS) para Food Trucks, desenvolvido com HTML5, CSS3 e JavaScript puro, integrado com Supabase como banco de dados.

## ✨ Funcionalidades

- **💳 Ponto de Venda**: Adicione produtos ao carrinho, escolha método de pagamento e finalize vendas
- **👨‍🍳 Cozinha**: Acompanhe pedidos em tempo real com status (Pendente → Pronto → Entregue)
- **📦 Estoque**: Visualize ingredientes, estoque atual e alertas de estoque baixo
- **🔄 Sincronização em Tempo Real**: Atualização automática a cada 5 segundos
- **📱 Design Responsivo**: Funciona perfeitamente em desktop, tablet e celular

## 🚀 Como Usar

### 1. Configurar o Supabase

1. Vá para [https://supabase.com](https://supabase.com)
2. Crie uma nova conta e um novo projeto
3. Vá para **SQL Editor** e execute o script abaixo para criar as tabelas:

```sql
-- Tabela de Ingredientes
CREATE TABLE IF NOT EXISTS public.ingredients (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  stock NUMERIC NOT NULL DEFAULT 0,
  unit TEXT NOT NULL,
  pricePerUnit NUMERIC NOT NULL DEFAULT 0,
  lowStockThreshold NUMERIC NOT NULL DEFAULT 0,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Tabela de Produtos
CREATE TABLE IF NOT EXISTS public.products (
  id TEXT PRIMARY KEY,
  name TEXT NOT NULL,
  sellingPrice NUMERIC NOT NULL DEFAULT 0,
  cost NUMERIC NOT NULL DEFAULT 0,
  category TEXT NOT NULL,
  recipe JSONB NOT NULL DEFAULT '[]',
  imageUrl TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Tabela de Vendas
CREATE TABLE IF NOT EXISTS public.sales (
  id TEXT PRIMARY KEY,
  items JSONB NOT NULL DEFAULT '[]',
  totalAmount NUMERIC NOT NULL DEFAULT 0,
  paymentMethod TEXT NOT NULL,
  status TEXT NOT NULL DEFAULT 'pending',
  createdAt TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updatedAt TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Desativar RLS (permite acesso público)
ALTER TABLE public.ingredients DISABLE ROW LEVEL SECURITY;
ALTER TABLE public.products DISABLE ROW LEVEL SECURITY;
ALTER TABLE public.sales DISABLE ROW LEVEL SECURITY;

-- Dados de Exemplo
INSERT INTO public.ingredients (id, name, stock, unit, pricePerUnit, lowStockThreshold) VALUES
('ing-pao', 'Pão de Hambúrguer', 50, 'un', 0.50, 10),
('ing-carne', 'Carne Bovina (Kg)', 10, 'kg', 25.00, 2),
('ing-queijo', 'Queijo Cheddar (un)', 100, 'un', 0.80, 20),
('ing-alface', 'Alface', 30, 'un', 1.00, 5),
('ing-tomate', 'Tomate', 40, 'un', 0.80, 10)
ON CONFLICT DO NOTHING;

INSERT INTO public.products (id, name, sellingPrice, cost, category, recipe) VALUES
('prod-burger-classico', 'Hambúrguer Clássico', 15.00, 5.00, 'Lanches', '[{"id": "ing-pao", "quantity": 1}, {"id": "ing-carne", "quantity": 0.15}, {"id": "ing-queijo", "quantity": 1}]'),
('prod-burger-especial', 'Hambúrguer Especial', 18.00, 6.50, 'Lanches', '[{"id": "ing-pao", "quantity": 1}, {"id": "ing-carne", "quantity": 0.20}, {"id": "ing-queijo", "quantity": 2}, {"id": "ing-alface", "quantity": 1}, {"id": "ing-tomate", "quantity": 1}]'),
('prod-refrigerante', 'Refrigerante 2L', 8.00, 3.00, 'Bebidas', '[]'),
('prod-agua', 'Água 500ml', 2.00, 0.50, 'Bebidas', '[]')
ON CONFLICT DO NOTHING;
```

4. Vá para **Settings** → **API** e copie:
   - **Project URL** (SUPABASE_URL)
   - **anon public** (SUPABASE_KEY)

### 2. Atualizar as Credenciais

Abra o arquivo `index.html` e procure pelas linhas (aproximadamente linha 520):

```javascript
const SUPABASE_URL = 'https://lyvovusanhuzppoyvtfu.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

Substitua pelos seus valores do Supabase.

### 3. Implantar no Netlify

#### Opção A: Usando GitHub (Recomendado)

1. Crie um novo repositório no GitHub
2. Faça push do código
3. Vá para [https://app.netlify.com](https://app.netlify.com)
4. Clique em **"New site from Git"**
5. Selecione seu repositório
6. Clique em **"Deploy"**

#### Opção B: Deploy Manual

1. Vá para [https://app.netlify.com](https://app.netlify.com)
2. Arraste e solte a pasta do projeto na área de upload
3. Pronto! Seu site estará online em segundos

**Seu aplicativo está online! 🎉**

## 📁 Estrutura

```
food-truck-pos/
├── index.html          # Aplicativo completo (HTML + CSS + JS)
├── _redirects          # Configuração de redirecionamento (Netlify)
├── .gitignore          # Arquivos a ignorar no Git
└── README.md           # Este arquivo
```

## 🔧 Tecnologias

- **HTML5**: Estrutura
- **CSS3**: Estilo e responsividade
- **JavaScript**: Lógica da aplicação
- **Supabase**: Banco de dados em nuvem
- **Netlify**: Hospedagem

## 📱 Recursos

- ✅ Adicionar/remover produtos do carrinho
- ✅ Múltiplos métodos de pagamento (Dinheiro, Débito, Crédito, PIX)
- ✅ Atualização automática de estoque
- ✅ Acompanhamento de pedidos em tempo real
- ✅ Alertas de estoque baixo
- ✅ Interface intuitiva e responsiva

## 🐛 Solução de Problemas

### "Erro ao carregar produtos"
- Verifique se o Supabase está acessível
- Verifique se as credenciais estão corretas no `index.html`
- Verifique se as tabelas foram criadas corretamente

### "Erro ao finalizar venda"
- Verifique se o estoque é suficiente
- Verifique se a receita do produto está correta (JSON válido)

### "Pedidos não aparecem na cozinha"
- Verifique se o status da venda é `pending` ou `ready`
- Aguarde alguns segundos para a atualização automática

## 📞 Suporte

Se tiver dúvidas ou problemas, verifique:
- A documentação do [Supabase](https://supabase.com/docs)
- A documentação do [Netlify](https://docs.netlify.com)

## 📄 Licença

Este projeto é de código aberto e está disponível sob a licença MIT.

---

**Desenvolvido com ❤️ para Food Trucks**

