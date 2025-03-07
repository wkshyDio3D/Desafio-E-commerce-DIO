# Desafio-E-commerce-DIO

-- Entrega de Desafio E-commerce DIO 
-- ===========================================

create database if not exists ecommerce;
use ecommerce;

-- ------------------
CREATE TABLE clients (
  	idClient INT AUTO_INCREMENT primary key,
  	Fname VARCHAR(15),
  	Minit VARCHAR(5),
  	Lname VARCHAR(20),
  	Cpf_Cnpj char(15),
 	 date_birth DATE,
  	Address VARCHAR(45),
  	Referenc VARCHAR(45),
  	Neighborhood VARCHAR(45),
  	City VARCHAR(45),
  	Zip CHAR(10),
  	constraint unique_cpf_cnpj_cliente unique (cpf_cnpj)
  );
  alter table clients auto_increment=1;
  
  -- -----------------
  CREATE TABLE Product (
  	idProduct INT AUTO_INCREMENT primary key,
  	Pname varchar(20) not null,
  	Classification_kids bool default false,
  	Category enum('Eletronicos', 'Vestimentas', 'Brinquedos', 'Alimentos', 'moveis', 'Utilidades') not null,
  	Avaliacion float default 0,
  	Size varchar (10),
  	Descrisao VARCHAR(45) NULL
  );

-- ----------------------
  create table Payments(
    	idPayment int primary key,
    	idClient int, 
    	idSeller int,
    	typePayment enum('Pix', 'Boleto', 'Dinheiro', 'Cartao Debito','Cartao Credito', 'Dois Cartoes'),
    	limitAvailable float
    );

-- --------------------------
create table CardPayments(
	idCard int auto_increment primary key,
   	idclient int,
  	 CardName varchar(20),
        CardNumbers char(16),
        CardDate DATE,
        validKey char(3)
	);
    
-- ------------------------
CREATE TABLE orders(
  	idOrder INT AUTO_INCREMENT primary key,
  	idOrderClient int,
  	orderStatus enum('Em Analize', 'Em Processamento', 'Confirmado', 'Nao Aprovado') DEFAULT 'Em Processamento',
  	orderDescription varchar(255),
  	sendValue float default 10,
  	paymentCash bool default false,
  	constraint fk_order_client foreign key (idOrderClient) references clients(idClient)
		on update cascade
        -- on delete set null
  );
  
-- ------------------------
  CREATE TABLE productStorage(
  	idprodStorage INT AUTO_INCREMENT primary key,
  	storagelocation varchar(255),
  	Quantidade int default 0,
  	orderDescription varchar(255)
  );

-- ------------------------
  create table suppliers(
	 idSupplier INT AUTO_INCREMENT primary key,
	 SocialName varchar(255) not null,
  	 CPF_CNPJ char(15),
   	 Address varchar(45),
   	 zip char(10),
   	 contact char(11) not null,
  	 constraint unique_cpf_cnpj_cliente unique (cpf_cnpj)
  );

-- ------------------------
  create table sellers(
	  idSeller INT AUTO_INCREMENT primary key,
	  SocialName varchar(255) not null,
   	  AbstName varchar(255),
  	  Cpf_Cnpj char(15),
  	  Address varchar (45),
  	  zip char(10),
   	  contact char(11) not null,
  	  constraint unique_cpf_cnpj_cliente unique (cpf_cnpj)
  );

-- ------------------------
  create table productSeller(
	 idPseller int,
   	 idPproduct int,
   	 prodQuantity int default 1,
   	 primary key (idPseller, idPproduct),
   	 constraint fk_product_seller foreign key (idPseller) references sellers(idSeller),
   	 constraint fk_product_product foreign key (idPproduct) references product(idproduct)
    );
    
-- ------------------------
     create table productOrder(
   	 idPOproduct int,
   	 idPOorder int,
   	 poQuantity int default 1,
   	 poStatus enum('Disponivel', 'Sem Estoque') default 'Disponivel',
   	 primary key (idPOproduct, idPOorder),
    	constraint fk_productorder_product foreign key (idPOproduct) references product(idProduct),
    	constraint fk_productorder_order foreign key  (idPOorder) references orders(idOrder)
    );

-- ------------------------
 create table storageLocation(
    	idLproduct int,
    	idLstorage int,
    	location varchar(255) not null,
    	primary key (idLproduct, idLstorage),
    	constraint fk_storage_Location_product foreign key (idLproduct) references product(idproduct),
    	constraint fk_storage_Location_storage foreign key (idLstorage) references productStorage(idprodStorage)
    );

 create table productsupplier(
   	 idPsSupplier int,
   	 idPsProduct int,
    	quantiy int not null,
    	primary key (idPsSupplier, idPsProduct),
    	constraint fk_product_supplier_supplier foreign key (idPsSupplier) references suppliers(idSupplier),
    	constraint fk_product_supplier_product foreign key (idPsProduct) references Product(idProduct)
    );                                                                         
