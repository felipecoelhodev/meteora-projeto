# E-commerce | Meteora Store 

O Meteora Store é uma plataforma moderna de e-commerce desenvolvida com Next.js (App Router) e integrada ao Supabase como solução de Backend-as-a-Service (BaaS) para persistência e gestão de dados, contando com uma camada de API interna (BFF) otimizada para a listagem, filtragem e pesquisa de produtos e categorias. A aplicação foi feita para por em prática toda a materialização dos conceitos mais avançados de engenharia de software e desenvolvimento Front-End em React 19 — como estratégias de renderização (SSR, CSR, SSG e ISR), otimização de SEO e criação de endpoints RESTful —, alinhando-se diretamente com as últimas tendências globais de tecnologia e Inteligência Artificial discutidas no mercado para entregar uma experiência de utilizador fluida, performática e de alta qualidade. O projeto está em constante atualização para adicionar novas páginas e funcionalidades novas. 

---

## Funcionalidades Principais

- **Camada de Dados Assíncrona**: Integração direta com o cliente do Supabase para consultas eficientes.
- **Listagem Dinâmica de Categorias**: Endpoint dedicado para obter todas as categorias ordenadas alfabeticamente.
- **Listagem Avançada de Produtos**: Suporte para limitação de quantidade de registos e filtragem por itens em destaque (*featured*).
- **Motor de Pesquisa Interno**: Pesquisa textual flexível que atua sobre o nome e a descrição dos produtos usando correspondência parcial (`ilike`).
- **Script de Inicialização (Seed)**: Script automatizado para limpar tabelas antigas e repovoar a base de dados com categorias e produtos predefinidos.

---

## Tecnologias Utilizadas

- **Framework**: Next.js (App Router / API Routes)
- **Base de Dados**: Supabase (PostgreSQL)
- **Variáveis de Ambiente**: Dotenv (carregamento a partir de `.env.local`)
- **Execução**: Node.js

---

## Estrutura da Base de Dados (PostgreSQL)

A base de dados é composta por duas tabelas principais relacionadas, contendo índices específicos para assegurar a máxima performance nas operações de leitura. Devido à nomenclatura em `camelCase`, os identificadores das colunas devem ser referenciados com aspas duplas nas consultas SQL.

### 1. Tabela `categories`
* `id` (BIGSERIAL, Primary Key)
* `name` (TEXT, Unique, Obrigatório)
* `"imageSrc"` (TEXT)
* `"createdAt"` / `"updatedAt"` (TIMESTAMPTZ, predefinição: `NOW()`)

### 2. Tabela `products`
* `id` (BIGSERIAL, Primary Key)
* `name` (TEXT, Obrigatório)
* `description` (TEXT)
* `price` (TEXT, Obrigatório)
* `"imageSrc"` (TEXT)
* `colors` (JSONB, predefinição: `[]`) — Armazena um array de objetos com o nome e o valor hexadecimal da cor.
* `sizes` (JSONB, predefinição: `[]`) — Armazena um array com os tamanhos disponíveis (ex: P, M, G ou numeração de calçado).
* `"categoryId"` (BIGINT, FK referenciando `categories(id)` com `ON DELETE CASCADE`)
* `"isFeatured"` (BOOLEAN, predefinição: `false`)
* `"isActive"` (BOOLEAN, predefinição: `true`)
* `"createdAt"` / `"updatedAt"` (TIMESTAMPTZ, predefinição: `NOW()`)

### Índices de Performance
* `"idx_products_categoryId"` (Otimização de junções entre tabelas)
* `"idx_products_isFeatured"` (Aceleração de filtros de destaques)
* `"idx_products_isActive"` (Filtragem rápida de produtos ativos)

---

## Configuração do Ambiente

Para que a aplicação comunique corretamente com o Supabase, é estritamente necessário configurar as chaves de acesso. 

1. Crie um ficheiro na raiz do projeto com o nome `.env.local`.
2. Adicione as seguintes variáveis com os dados do seu painel do Supabase:

```env
NEXT_PUBLIC_SUPABASE_URL=o_seu_url_do_supabase
NEXT_PUBLIC_SUPABASE_ANON_KEY=a_sua_chave_anon_do_supabase
