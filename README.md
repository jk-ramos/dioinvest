# Documentação FAQ - Sistema Imobiliário

## Visão Geral

Sistema de Perguntas Frequentes (FAQ) integrado ao projeto imobiliário, permitindo gerenciamento e consulta de dúvidas comuns dos usuários.

## API Endpoints

### GET /api/faq

Descrição: Lista todas as perguntas frequentes com filtros opcionais.

Método: GET

URL: `/api/faq`

Parâmetros:
- query `categoria` (opcional): filtrar por categoria (ex: "vendas", "locacao", "financiamento")
- query `busca` (opcional): buscar por palavra-chave na pergunta ou resposta
- query `pagina` (opcional): número da página para paginação
- query `limite` (opcional): número de itens por página (padrão: 10)

Exemplo de Requisição:

```js
GET /api/faq?categoria=vendas&busca=documentos&pagina=1&limite=5
```

Exemplo de Resposta (200):

```json
{
  "total": 25,
  "pagina": 1,
  "limite": 5,
  "itens": [
    {
      "id": 1,
      "pergunta": "Quais documentos são necessários para compra?",
      "resposta": "Para comprar um imóvel você precisa de: RG, CPF, comprovante de renda...",
      "categoria": "vendas",
      "dataCriacao": "2024-01-15T10:30:00Z",
      "ativo": true
    }
  ]
}
```

Códigos de Status: 200, 400, 500

Autenticação: não requer

---

### GET /api/faq/:id

Descrição: Busca uma pergunta específica por ID.

Método: GET

URL: `/api/faq/:id`

Parâmetros:
- path `id` (obrigatório): ID da pergunta

Exemplo de Requisição:

```js
GET /api/faq/1
```

Exemplo de Resposta (200):

```json
{
  "id": 1,
  "pergunta": "Quais documentos são necessários para compra?",
  "resposta": "Para comprar um imóvel você precisa de: RG, CPF, comprovante de renda, comprovante de estado civil...",
  "categoria": "vendas",
  "dataCriacao": "2024-01-15T10:30:00Z",
  "ativo": true
}
```

Códigos de Status: 200, 404, 500

Autenticação: não requer

---

### POST /api/faq

Descrição: Cria uma nova pergunta frequente.

Método: POST

URL: `/api/faq`

Body (JSON):
- `pergunta` (obrigatório): texto da pergunta
- `resposta` (obrigatório): texto da resposta
- `categoria` (obrigatório): categoria da pergunta

Exemplo de Requisição:

```js
POST /api/faq
Content-Type: application/json

{
  "pergunta": "Como funciona o financiamento imobiliário?",
  "resposta": "O financiamento imobiliário é um empréstimo específico...",
  "categoria": "financiamento"
}
```

Exemplo de Resposta (201):

```json
{
  "id": 15,
  "pergunta": "Como funciona o financiamento imobiliário?",
  "resposta": "O financiamento imobiliário é um empréstimo específico...",
  "categoria": "financiamento",
  "dataCriacao": "2024-09-13T14:25:00Z",
  "ativo": true
}
```

Códigos de Status: 201, 400, 500

Autenticação: requer token de administrador

---

### PUT /api/faq/:id

Descrição: Atualiza uma pergunta existente.

Método: PUT

URL: `/api/faq/:id`

Parâmetros:
- path `id` (obrigatório): ID da pergunta

Body (JSON):
- `pergunta` (opcional): nova pergunta
- `resposta` (opcional): nova resposta
- `categoria` (opcional): nova categoria
- `ativo` (opcional): status ativo/inativo

Exemplo de Requisição:

```js
PUT /api/faq/1
Content-Type: application/json

{
  "resposta": "Resposta atualizada com mais detalhes...",
  "ativo": true
}
```

Códigos de Status: 200, 400, 404, 500

Autenticação: requer token de administrador

---

### DELETE /api/faq/:id

Descrição: Remove uma pergunta frequente.

Método: DELETE

URL: `/api/faq/:id`

Parâmetros:
- path `id` (obrigatório): ID da pergunta

Códigos de Status: 204, 404, 500

Autenticação: requer token de administrador

## Middleware de Validação

### Validação de Pergunta

Localização: `back-end/src/middleware/faq-validation.js`

Regras:
- Campo obrigatório para criação/atualização
- Mínimo de 10 caracteres
- Máximo de 500 caracteres
- Não pode conter apenas espaços em branco

### Validação de Resposta

Regras:
- Campo obrigatório para criação/atualização
- Mínimo de 20 caracteres
- Máximo de 2000 caracteres
- Não pode conter apenas espaços em branco

### Validação de Categoria

Categorias permitidas:
- `vendas`
- `locacao` 
- `financiamento`
- `documentacao`
- `visitas`
- `geral`

## Tratamento de Erros

### Erro 400 - Bad Request

Retornado quando:
- Dados de entrada inválidos
- Campos obrigatórios ausentes
- Formato de dados incorreto

Exemplo de Resposta:

```json
{
  "erro": "Dados inválidos",
  "detalhes": [
    "Campo 'pergunta' é obrigatório",
    "Campo 'categoria' deve ser um dos valores permitidos"
  ]
}
```

### Erro 404 - Not Found

Retornado quando:
- Pergunta não encontrada pelo ID
- Rota não existente

Exemplo de Resposta:

```json
{
  "erro": "Pergunta não encontrada",
  "detalhes": "Não foi possível encontrar uma pergunta com o ID fornecido"
}
```

### Erro 500 - Internal Server Error

Retornado quando:
- Erro interno do servidor
- Falha na conexão com banco de dados
- Erro não tratado

Exemplo de Resposta:

```json
{
  "erro": "Erro interno do servidor",
  "detalhes": "Ocorreu um erro inesperado. Tente novamente mais tarde."
}
```

## Guia de Uso - Frontend

### Como Acessar a Aba FAQ

1. **Navegação Principal**
   - Acesse o menu principal do site
   - Clique em "Ajuda" ou "FAQ"
   - Você será redirecionado para `/faq`

2. **Link Direto**
   - Use a URL direta: `https://seusite.com/faq`

### Como Fazer uma Pergunta

#### Para Usuários Comuns:

1. **Buscar Pergunta Existente**
   - Use a barra de busca no topo da página
   - Digite palavras-chave relacionadas à sua dúvida
   - Verifique se sua pergunta já foi respondida

2. **Solicitar Nova Pergunta**
   - Se não encontrar resposta, clique em "Enviar Pergunta"
   - Preencha o formulário de contato
   - Sua pergunta será analisada e pode ser adicionada ao FAQ

#### Para Administradores:

1. **Acesso ao Painel**
   - Faça login como administrador
   - Acesse `/admin/faq`

2. **Criar Pergunta**
   - Clique em "Nova Pergunta"
   - Preencha os campos obrigatórios
   - Selecione a categoria apropriada
   - Salve a pergunta

### Como Procurar por uma Pergunta

#### Método 1: Busca por Texto

1. **Barra de Busca**
   - Localize a caixa de busca no topo da página FAQ
   - Digite palavras-chave relacionadas à sua dúvida
   - Pressione Enter ou clique no ícone de busca

2. **Resultados**
   - As perguntas correspondentes aparecerão destacadas
   - Clique na pergunta para expandir a resposta completa

#### Método 2: Filtro por Categoria

1. **Menu de Categorias**
   - Use os botões de categoria na lateral ou topo
   - Categorias disponíveis:
     - **Vendas**: Processo de compra, documentação
     - **Locação**: Aluguel, contratos, fiador
     - **Financiamento**: Empréstimos, FGTS, consórcios
     - **Documentação**: Papéis necessários, cartório
     - **Visitas**: Agendamento, horários, preparo
     - **Geral**: Outras dúvidas

2. **Visualização**
   - Clique na categoria desejada
   - Apenas perguntas dessa categoria serão exibidas

#### Método 3: Navegação por Lista

1. **Lista Completa**
   - Todas as perguntas são exibidas por padrão
   - Use a paginação no final da página
   - Perguntas são ordenadas por relevância

2. **Expandir/Recolher**
   - Clique na pergunta para ver a resposta
   - Clique novamente para recolher

### Dicas de Uso

#### Para Busca Eficiente:

- Use palavras-chave específicas (ex: "ITBI", "escritura", "FGTS")
- Combine termos relacionados (ex: "financiamento CEF")
- Teste sinônimos se não encontrar resultados

#### Para Melhor Experiência:

- Comece sempre pela busca antes de fazer nova pergunta
- Marque perguntas úteis como favoritas (se disponível)
- Use categorias para navegar por assuntos relacionados

### Funcionalidades Especiais

#### Perguntas Relacionadas

- Após visualizar uma resposta, veja sugestões de perguntas similares
- Links para tópicos relacionados aparecem no final de cada resposta

#### Feedback

- Avalie se a resposta foi útil usando os botões 👍/👎
- Deixe comentários para melhorar o conteúdo (se disponível)

#### Compartilhamento

- Use o botão de compartilhar para enviar links específicos
- URLs diretas para cada pergunta facilitam referências

## Estrutura de Arquivos

```
back-end/src/
├── routes/
│   └── faq.js              # Rotas do FAQ
├── middleware/
│   └── faq-validation.js   # Validações
├── controllers/
│   └── faq-controller.js   # Lógica de negócio
└── models/
    └── faq-model.js        # Modelo de dados

front-end/src/
├── pages/
│   └── FAQ.js              # Página principal do FAQ
├── components/
│   ├── FaqItem.js          # Item individual de FAQ
│   ├── FaqSearch.js        # Componente de busca
│   └── FaqCategories.js    # Filtro de categorias
└── services/
    └── faq-service.js      # Serviços de API
```

## Próximos Passos

1. Implementar sistema de avaliação das respostas
2. Adicionar funcionalidade de sugestão de perguntas
3. Criar dashboard de estatísticas para administradores
4. Implementar notificações para novas perguntas
