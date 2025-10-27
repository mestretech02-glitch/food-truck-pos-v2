# 📋 Guia Completo de Configuração - Food Truck POS

Este guia vai te levar passo a passo para colocar o Food Truck POS online em menos de 30 minutos!

## ✅ Pré-requisitos

- Uma conta no [Supabase](https://supabase.com) (grátis)
- Uma conta no [GitHub](https://github.com) (grátis)
- Uma conta no [Netlify](https://netlify.com) (grátis)
- Este projeto descompactado em uma pasta

## 🔧 Passo 1: Configurar o Supabase

### 1.1 Criar Projeto

1. Vá para [https://supabase.com](https://supabase.com)
2. Clique em **"Start your project"**
3. Faça login com sua conta (ou crie uma nova)
4. Clique em **"New project"**
5. Preencha:
   - **Project name**: `food-truck-pos`
   - **Database password**: Crie uma senha forte
   - **Region**: Selecione a região mais próxima de você
6. Clique em **"Create new project"**

### 1.2 Criar Tabelas

1. Aguarde o projeto ser criado (pode levar alguns minutos)
2. Clique em **"SQL Editor"** (no menu esquerdo)
3. Clique em **"New Query"**
4. Copie e cole todo o código abaixo:

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

-- Desativar RLS
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

5. Clique em **"RUN"** (ou pressione Ctrl + Enter)
6. Aguarde a execução (deve aparecer uma mensagem de sucesso)

### 1.3 Obter as Credenciais

1. Clique em **"Settings"** (no menu esquerdo)
2. Clique em **"API"**
3. Você vai ver:
   - **Project URL** (copie este)
   - **anon public** (copie este também)

4. **Guarde essas credenciais!** Você vai precisar delas no próximo passo.

## 📝 Passo 2: Atualizar o Arquivo index.html

1. Abra o arquivo `index.html` com o Bloco de Notas
2. Procure pelas linhas (aproximadamente na linha 520):

```javascript
const SUPABASE_URL = 'https://lyvovusanhuzppoyvtfu.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...';
```

3. Substitua:
   - `SUPABASE_URL` pelo seu **Project URL** do Supabase
   - `SUPABASE_KEY` pela sua **anon public key** do Supabase

4. Salve o arquivo (Ctrl + S)

**Exemplo:**
```javascript
const SUPABASE_URL = 'https://seu-projeto.supabase.co';
const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSI...';
```

## 🐙 Passo 3: Criar Repositório no GitHub

### 3.1 Criar Repositório

1. Vá para [https://github.com/new](https://github.com/new)
2. Preencha:
   - **Repository name**: `food-truck-pos`
   - **Description**: `Food Truck POS System`
   - **Public**: Deixe marcado
   - **Add a README file**: Deixe desmarcado
   - **Add .gitignore**: Deixe desmarcado

3. Clique em **"Create repository"**

### 3.2 Fazer Upload dos Arquivos

Você pode fazer isso de duas formas:

#### Opção A: Usando GitHub Desktop (Mais Fácil)

1. Baixe e instale [GitHub Desktop](https://desktop.github.com)
2. Abra o GitHub Desktop
3. Clique em **"File"** → **"Add Local Repository"**
4. Selecione a pasta do projeto
5. Clique em **"Publish repository"**
6. Deixe o nome como `food-truck-pos`
7. Clique em **"Publish Repository"**

#### Opção B: Usando a Web

1. Vá para seu repositório no GitHub
2. Clique em **"Add file"** → **"Upload files"**
3. Arraste e solte os arquivos:
   - `index.html`
   - `_redirects`
   - `.gitignore`
   - `README.md`
   - `SETUP_GUIDE.md`

4. Clique em **"Commit changes"**

## 🚀 Passo 4: Implantar no Netlify

### 4.1 Conectar GitHub ao Netlify

1. Vá para [https://app.netlify.com](https://app.netlify.com)
2. Clique em **"Sign up"** (ou faça login se já tem conta)
3. Selecione **"GitHub"** para conectar

### 4.2 Criar Site

1. Clique em **"New site from Git"**
2. Selecione **"GitHub"**
3. Procure por **`food-truck-pos`**
4. Clique em **"Deploy site"**

### 4.3 Configurar Netlify (Importante!)

Se o site mostrar "404", faça isso:

1. Vá para seu site no Netlify
2. Clique em **"Site settings"**
3. Clique em **"Build & deploy"**
4. Procure por **"Publish directory"**
5. Deixe vazio (ou coloque `.`)
6. Salve

**Pronto! Seu site está online! 🎉**

## ✅ Verificar se Tudo Está Funcionando

1. Acesse a URL do seu site no Netlify
2. Você deve ver a página do Food Truck POS
3. Teste:
   - Clique em um produto para adicionar ao carrinho
   - Selecione um método de pagamento
   - Clique em "Finalizar Venda"
   - Vá para a aba "Cozinha" para ver o pedido

Se tudo funcionar, parabéns! 🎊

## 🔧 Adicionar Mais Produtos

Para adicionar mais produtos e ingredientes:

1. Vá para seu projeto no Supabase
2. Clique em **"Table Editor"**
3. Selecione **"products"** ou **"ingredients"**
4. Clique em **"Insert"** para adicionar novos itens

## 📱 Acessar de Qualquer Lugar

Seu site está online e pode ser acessado de qualquer dispositivo com internet!

- **Desktop**: Abra a URL no navegador
- **Mobile**: Abra a URL no navegador do celular
- **Tablet**: Funciona perfeitamente

## 🆘 Problemas Comuns

### "Erro ao carregar produtos"
- Verifique se o Supabase está online
- Verifique se as credenciais estão corretas
- Verifique se as tabelas foram criadas

### "Página em branco"
- Recarregue a página (F5)
- Verifique se o JavaScript está habilitado
- Abra o console (F12) para ver erros

### "404 Not Found"
- Verifique se o arquivo `_redirects` existe
- Verifique se o arquivo `index.html` está na raiz
- Faça um novo deploy no Netlify

## 📞 Próximos Passos

Agora que seu site está online, você pode:

1. **Customizar**: Adicione seu logo, cores, etc.
2. **Adicionar Produtos**: Insira seus produtos no Supabase
3. **Treinar Equipe**: Mostre como usar o sistema
4. **Monitorar Vendas**: Acompanhe as vendas em tempo real

---

**Parabéns! Seu Food Truck POS está pronto para usar! 🍔**

