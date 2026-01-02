# Projeto Conceitual - Sistema de E-commerce

## Descrição do Projeto
Este é um modelo conceitual de banco de dados para um sistema de e-commerce que contempla:
- Gestão de clientes (Pessoa Física e Jurídica)
- Catálogo de produtos
- Processamento de pedidos
- Múltiplas formas de pagamento
- Rastreamento de entregas

## Modelo Entidade-Relacionamento
```mermaid
erDiagram
    CLIENTE ||--o{ PEDIDO : realiza
    CLIENTE ||--o| CLIENTE_PF : "pode ser"
    CLIENTE ||--o| CLIENTE_PJ : "pode ser"
    CLIENTE ||--o{ FORMA_PAGAMENTO : possui
    
    PEDIDO ||--|{ ITEM_PEDIDO : contem
    PEDIDO ||--|| ENTREGA : possui
    PEDIDO ||--o{ PAGAMENTO : "pago por"
    
    PRODUTO ||--o{ ITEM_PEDIDO : "presente em"
    PRODUTO }o--|| CATEGORIA : pertence
    PRODUTO }o--o{ FORNECEDOR : fornecido
    
    FORMA_PAGAMENTO ||--o{ PAGAMENTO : utilizada
    
    CLIENTE {
        int id_cliente
        string email
        string telefone
        date data_cadastro
    }
    
    CLIENTE_PF {
        int id_cliente
        string cpf
        string nome
        string sobrenome
        date data_nascimento
    }
    
    CLIENTE_PJ {
        int id_cliente
        string cnpj
        string razao_social
        string nome_fantasia
        string inscricao_estadual
    }
    
    FORMA_PAGAMENTO {
        int id_forma_pagamento
        int id_cliente
        string tipo
        string descricao
        boolean ativo
    }
    
    PEDIDO {
        int id_pedido
        int id_cliente
        date data_pedido
        decimal valor_total
        string status
    }
    
    ITEM_PEDIDO {
        int id_item
        int id_pedido
        int id_produto
        int quantidade
        decimal preco_unitario
    }
    
    PRODUTO {
        int id_produto
        int id_categoria
        string nome
        string descricao
        decimal preco
        int estoque
    }
    
    CATEGORIA {
        int id_categoria
        string nome
        string descricao
    }
    
    FORNECEDOR {
        int id_fornecedor
        string razao_social
        string cnpj
        string contato
    }
    
    ENTREGA {
        int id_entrega
        int id_pedido
        string codigo_rastreio
        string status
        date data_envio
        date data_entrega_prevista
        date data_entrega_realizada
        string endereco_entrega
    }
    
    PAGAMENTO {
        int id_pagamento
        int id_pedido
        int id_forma_pagamento
        decimal valor
        date data_pagamento
        string status_pagamento
    }
```

## Principais Entidades e Regras de Negócio

### Cliente (PF/PJ)
- Um cliente pode ser **Pessoa Física OU Pessoa Jurídica** (exclusivo)
- Implementado através de especialização/herança

### Formas de Pagamento
- Um cliente pode cadastrar **múltiplas formas de pagamento**
- Exemplos: cartão de crédito, PIX, boleto

### Entrega
- Cada pedido possui **uma entrega**
- Possui **código de rastreio** único
- **Status** da entrega (pendente, em trânsito, entregue, etc.)
