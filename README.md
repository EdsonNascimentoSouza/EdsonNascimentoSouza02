# Edson Nascimento Souza02
## Projeto de Normalização de dados


![Diagrama](https://github.com/EdsonNascimentoSouza/EdsonNascimentoSouza02/blob/main/Diagram.png)

select *  from clientes;

create table contato as
select 
	codcliente as fk_id_cliente, 
    trim(substring_index(telefones,',',1)) as contato
from clientes
union
select 
	codcliente as fk_id_cliente,  
    trim(substring_index(telefones,',',-1)) as contato
from clientes;

alter table contato add constraint fk_cli foreign key(fk_id_cliente)
references cliente(id);

select * from contato;

-- TABELA ENDEREÇO
-- -------------------------------------------------------
select * from clientes;

create table endereco as
select 
	codcliente as fk_id_cliente,
    substring_index(endereco,',',1) as rua,
    substring_index(endereco,',',-1) as numero,
    cidade,
    estado 
from clientes;

alter table endereco add constraint fk_end foreign key(fk_id_cliente) 
references cliente(id);

-- TABELA PRODUTO
-- -------------------------------------------------------
select * from clientes;

create table produto as
select 
	codcliente fk_id_cliente, 
    trim(substring_index(produtoscomprados,',',1)) as produto 
from clientes
	union
select 
	codcliente fk_id_cliente, 
    trim(substring_index(produtoscomprados,',',-1)) as produto 
from clientes
	union
select 
	codcliente fk_id_cliente, 
    trim(substring_index(substring_index(produtoscomprados,',',2),',',-1)) as produto 
from clientes;

alter table produto add column id int primary key auto_increment first;

select * from produto;

create table item_comprado as
select id as fk_id_produto,fk_id_cliente from produto;

select * from item_comprado;


alter table produto drop column fk_id_cliente;

alter table item_comprado add constraint fk_prod
foreign key(fk_id_produto) references produto(id);

alter table item_comprado add constraint fk_cli_item
foreign key(fk_id_cliente) references cliente(id);


-- UNIFICAR TODAS AS TABELAS
-- -------------------------------------------------------
SELECT 
	c.id,
    c.nome,
    co.contato,
    ed.rua,
    ed.numero,
    ed.cidade,
    ed.estado,
    p.produto
FROM item_comprado as ic
inner join produto as p on ic.fk_id_produto = p.id
inner join cliente as c on ic.fk_id_cliente = c.id
inner join contato as co on c.id = co.fk_id_cliente
inner join endereco as ed on c.id = ed.fk_id_cliente;

SELECT 
  distinct  p.produto,
  c.nome
FROM item_comprado as ic
inner join produto as p on ic.fk_id_produto = p.id
inner join cliente as c on ic.fk_id_cliente = c.id
inner join contato as co on c.id = co.fk_id_cliente
inner join endereco as ed on c.id = ed.fk_id_cliente
where c.id = 1;
;
