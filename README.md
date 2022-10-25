# Projects
Data Analyst 
**PROJECT- CREATING A DATABASE WITH SQL **

Purpose: The purpose of this project is to provide an environment from an online store who that is both convenient and efficient for users to retrieve and store information.
The design of this Database is made to divide the information into subject-based tables to reduce redundant data. Provides Access with the information it requires to join the information in the tables together as needed. Helps support and ensure the accuracy and integrity of your information. 

CREATE SCHEMA proyect_001;
USE proyect_001;

CREATE TABLE dim_clients(
	id_cliente INT,
    date_creation_client DATE,
    name_client VARCHAR(100),
    surname_client VARCHAR(100),
    email_client VARCHAR(100),
    phonenumber_client VARCHAR(100),
    region_client VARCHAR(100),
    country_client VARCHAR(100),
    postalcode_client VARCHAR(100),
    address_client VARCHAR(255),
    PRIMARY KEY (id_cliente)
);

CREATE TABLE dim_products (
  id_product int  NULL,
  SKU_product int NOT NULL,
  name_product text,
  publish_product int  NULL,
  inventary_product text,
  precio_normal_product int NOT NULL,
  category_product text,
  PRIMARY KEY (SKU_product)
);

CREATE TABLE fac_pedidos (
  id_pedido int NOT NULL,
  SKU_producto int NULL,
  estado_pedido text,
  fecha_pedido date  NULL,
  id_cliente int  NULL,
  tipo_pago_pedido varchar(7) NOT NULL ,
  costo_pedido bigint  NULL,
  importe_de_descuento_pedido decimal(10,0)  NULL,
  importe_total_pedido int  NULL,
  cantidad_pedido int  NULL,
  codigo_cupon_pedido text,
  PRIMARY KEY (id_pedido),
  FOREIGN KEY (id_cliente) REFERENCES dim_clientes (id_cliente),
  FOREIGN KEY (SKU_producto) REFERENCES dim_productos (SKU_producto)
);

CREATE TABLE fac_pagos_stripe (
    fecha_pago DATETIME(6),
    id_pedido INT,
    importe_pago INT,
    moneda_pago TEXT,
    comision_pago DECIMAL(10 , 2 ),
    neto_pago DECIMAL(10 , 2 ),
    tipo_pago TEXT,
    FOREIGN KEY (id_pedido) REFERENCES fac_pedidos (id_pedido)
);

CREATE TABLE campaign_facebook_ads (
  id_campaign bigint NOT NULL,
  source_campaign_id bigint  NULL,
  campaign_created datetime(6)  NULL,
  campaign_status text,
  campaign_name text,
  campaign_objective text,
  campaign_stop_time datetime(6)  NULL,
  PRIMARY KEY (`id_campaign`)
);

CREATE TABLE ad_facebook_ads (
  fecha_creacion_ad date  NULL,
  id_campaign bigint  NULL,
  id_ad_set bigint  NULL,
  id_ad bigint NOT NULL,
  bid_type text,
  estado_ad text,
  nombre_ad text,
  PRIMARY KEY (`id_ad`)
);

CREATE TABLE campaign_facebook_ads_detalle (
  fecha_campaign date  NULL,
  id_ad bigint  NULL,
  id_campaign bigint  NULL,
  clicks int  NULL,
  cpc double  NULL,
  impresiones int  NULL,
  costo double  NULL,
  alcance int  NULL,
  FOREIGN KEY (id_ad) REFERENCES ad_facebook_ads (id_ad),
  FOREIGN KEY (id_campaign) REFERENCES campaign_facebook_ads (id_campaign)
);

CREATE TABLE campaign_google_analytics (
  fecha_google_analytics date,
  bounce_rate double,
  id_campaign bigint,
  nuevos_usuarios int,
  vista_paginas_por_sesion double,
  sesiones int,
  usuarios int,
	FOREIGN KEY (id_campaign) REFERENCES campaign_facebook_ads (id_campaign)
);
