# Requisitos funcionais

RF001 – Autenticação de Usuários

O sistema deve permitir que usuários cadastrados realizem login por meio de nome de usuário e senha.


RF002 – Gerenciar Usuários (opcional conforme o escopo da prova)

O sistema deve permitir cadastrar, editar e desativar usuários do sistema, definindo permissões (ex.: almoxarife, administrador).


RF003 – Cadastrar Produtos

O sistema deve permitir o cadastro de produtos, incluindo campos como: nome, descrição, fabricante, modelo, especificações técnicas, capacidade de armazenamento, conectividade, resolução, e outros atributos relevantes.


RF004 – Editar Produtos

O sistema deve permitir a edição dos dados de um produto cadastrado.


RF005 – Excluir/Inativar Produtos

O sistema deve permitir excluir ou inativar um produto do catálogo, impedindo novas movimentações.


RF006 – Consultar Produtos

O sistema deve permitir pesquisar e listar produtos cadastrados, oferecendo filtros por nome, categoria, modelo ou atributos técnicos.


RF007 – Registrar Entrada de Produtos

O sistema deve permitir lançar entradas de produtos no estoque, registrando quantidade, data, usuário responsável e motivo da entrada.


RF008 – Registrar Saída de Produtos

O sistema deve permitir lançar saídas de produtos, registrando quantidade, data, usuário responsável e motivo da saída.


RF009 – Atualizar Quantidade em Estoque

O sistema deve atualizar automaticamente o saldo do estoque a cada movimentação de entrada ou saída.


RF010 – Emitir Alertas de Estoque Mínimo

O sistema deve enviar alertas automáticos sempre que a quantidade de um produto ficar igual ou abaixo do nível mínimo configurado.


RF011 – Configurar Estoque Mínimo

O sistema deve permitir ao usuário definir o nível mínimo de estoque para cada produto.


RF012 – Registrar Histórico de Movimentações

O sistema deve registrar todas as movimentações (entrada e saída), com:

Produto

Quantidade

Tipo de movimentação (entrada/saída)

Usuário responsável

Data e hora

Observações


RF013 – Consultar Histórico de Movimentações

O sistema deve permitir consultar e filtrar o histórico por produto, data, tipo de operação ou usuário responsável.


RF014 – Interface de Login

O sistema deve apresentar uma tela de login para autenticação.


RF015 – Interface Principal (Dashboard)

O sistema deve exibir um painel com informações gerais, como:

Produtos com estoque mínimo

Total de produtos cadastrados

Movimentações recentes


RF016 – Interface de Cadastro de Produto

O sistema deve dispor de uma tela específica para cadastrar e editar produtos.


RF017 – Interface de Gestão de Estoque

O sistema deve possuir uma tela dedicada para registrar entradas e saídas de estoque e visualizar o saldo atual.



# Conectar ao banco criado antes de criar as tabelas:
CREATE DATABASE saep_db;

-- Tabela de Login
```
CREATE TABLE login (
    id_login SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    senha VARCHAR(100) NOT NULL
);
```

-- Tabela de Produto
```
CREATE TABLE produto (
    id_produto SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    preco_unit DECIMAL(10,2) NOT NULL,
    quantidade NUMERIC NOT NULL,
    qtd_minima NUMERIC NOT NULL,
    qtd_maxima NUMERIC NOT NULL,
	tipo_produto VARCHAR(100) not null,
	tensao VARCHAR(50) NOT NULL,
	dimensoes NUMERIC NOT NULL,
	resolucao VARCHAR(100) NOT NULL,
	armazenamento VARCHAR(50),
	conectividade VARCHAR (100)
);
```

-- Tabela de Histórico
```
CREATE TABLE historico (
    id_historico SERIAL PRIMARY KEY,
    fk_produto INT NOT NULL REFERENCES produto(id_produto) ON DELETE CASCADE,
    tipo_movimentacao VARCHAR(100) NOT NULL,
    qtd_movimentada NUMERIC NOT NULL,
    custo_total DECIMAL(10,2) NOT NULL,
    fk_usuario INT NOT NULL REFERENCES login(id_login) ON DELETE CASCADE,
    data_movimentacao DATE NOT NULL
);
```
## Inserindo os dados nas tabelas
```
insert into login(nome, email, senha)
values('usuario teste', 'usuario@teste.com', 'senai103@'),
('usuario admin', administrador@teste.com', 'senai103@'),
('usuario clinete', 'cliente@teste.com', 'senai103@');

INSERT INTO produto (
    nome, preco_unit, quantidade, qtd_minima, qtd_maxima,
    tensao, dimensoes, resolucao, armazenamento, conectividade, tipo_produto
) VALUES
('Smartphone Galaxy S24', 4999.90, 60, 10, 100, 'Bivolt', 6.8, '3200x1440', '256GB', 'Wi-Fi, 5G, Bluetooth', 'smartphone'),
('Smart TV LG OLED 55', 6999.00, 23, 5, 40, '110/220v', 55, '3840x2160 (4K)', NULL, 'Wi-Fi, HDMI, Bluetooth', 'smart TV'),
('poco x7', 300.00, 3, 10, 30, '110/220v', 6.5, '1920x1080', '512GB', 'Wi-Fi, HDMI, Bluetooth', 'smartphone'),
('Notebook Dell XPS 13', 8999.00, 25, 5, 60, 'Bivolt', 13.3, '1920x1080', '512GB SSD', 'Wi-Fi, Bluetooth', 'notebook');

INSERT INTO historico (fk_produto, tipo_movimentacao, qtd_movimentada, custo_total, fk_usuario, data_movimentacao)
VALUES
-- Entradas (compras ou reposições de estoque)
(1, 'entrada', 20, 6998.00, 2, '2025-11-01'),  -- Teclado Mecânico
(2, 'entrada', 50, 12495.00, 2, '2025-11-02'), -- Mouse Logitech
(3, 'entrada', 15, 29985.00, 2, '2025-11-02'), -- Monitor Gamer
(4, 'entrada', 30, 2997.00, 2, '2025-11-03'),  -- Cabo HDMI
(5, 'entrada', 10, 4990.00, 2, '2025-11-03'),  -- Headset

-- Saídas (vendas para clientes)
(1, 'saida', 3, 1049.70, 1, '2025-11-03'),  -- venda de teclados
(2, 'saida', 5, 1249.50, 1, '2025-11-03'),  -- venda de mouses
(3, 'saida', 2, 3998.00, 3, '2025-11-03'),  -- venda de monitores
(4, 'saida', 10, 999.00, 1, '2025-11-03'),  -- venda de cabos
(5, 'saida', 1, 499.00, 3, '2025-11-03');   -- venda de headset
```
