flowchart TD
%% =========================
%% DEFINICIÓN DE CLASES (Estilos globales)
%% =========================
classDef actor fill:#4A90E2,stroke:#2170B5,stroke-width:2px,color:#fff,rx:15,ry:15,font-weight:bold
classDef presentacion fill:#9B59B6,stroke:#7D3C98,stroke-width:2px,color:#fff,rx:5,ry:5
classDef negocio fill:#2ECC71,stroke:#229954,stroke-width:2px,color:#fff,rx:5,ry:5
classDef datos fill:#F39C12,stroke:#D68910,stroke-width:2px,color:#fff,rx:5,ry:5
classDef externo fill:#34495E,stroke:#2C3E50,stroke-width:2px,color:#fff,rx:5,ry:5,stroke-dasharray: 4 4

%% =========================
%% ACTORES
%% =========================
subgraph ACTORES ["👤 ACTORES"]
    Cliente["Cliente"]:::actor
    Seller["Seller"]:::actor
    Admin["Administrador"]:::actor
end

%% =========================
%% PRESENTACIÓN
%% =========================
subgraph PRESENTACION ["🖥️ PRESENTACIÓN"]
    Web["Aplicación Web / API REST"]:::presentacion
end

%% =========================
%% LÓGICA DE NEGOCIO
%% =========================
subgraph NEGOCIO ["⚙️ LÓGICA DE NEGOCIO"]
    Usuarios["Usuarios"]:::negocio
    Sellers["Sellers"]:::negocio
    Catalogo["Catálogo"]:::negocio
    Carrito["Carrito"]:::negocio
    Pedidos["Pedidos"]:::negocio
end

%% =========================
%% DATOS
%% =========================
subgraph DATOS ["💾 DATOS"]
    %% Se usa [(" ")] para darle forma de cilindro de base de datos
    BD[("Base de datos")]:::datos
end

%% =========================
%% SISTEMAS EXTERNOS
%% =========================
subgraph EXTERNOS ["🔌 SISTEMAS EXTERNOS"]
    Pago["Pasarela de pago"]:::externo
    ERP["ERP"]:::externo
    Envio["Servicio de envío"]:::externo
end

%% =========================
%% FLUJO PRINCIPAL
%% =========================
ACTORES --> PRESENTACION
PRESENTACION --> NEGOCIO
NEGOCIO --> DATOS

%% Integraciones (Línea punteada para denotar comunicación externa)
DATOS -.->|"Integraciones"| EXTERNOS

%% =========================
%% DISTRIBUCIÓN HORIZONTAL (Alineación)
%% =========================
Cliente ~~~ Seller
Seller ~~~ Admin
Usuarios ~~~ Sellers
Sellers ~~~ Catalogo
Catalogo ~~~ Carrito
Carrito ~~~ Pedidos
Pago ~~~ ERP
ERP ~~~ Envio

%% =========================
%% ESTILOS DE SUBGRÁFICOS (Contenedores)
%% =========================
style ACTORES fill:#EAF2F8,stroke:#4A90E2,stroke-width:2px,color:#154360,stroke-dasharray: 5 5
style PRESENTACION fill:#F4ECF7,stroke:#9B59B6,stroke-width:2px,color:#512E5F,stroke-dasharray: 5 5
style NEGOCIO fill:#EAFAF1,stroke:#2ECC71,stroke-width:2px,color:#186A3B,stroke-dasharray: 5 5
style DATOS fill:#FEF5E7,stroke:#F39C12,stroke-width:2px,color:#7E5109,stroke-dasharray: 5 5
style EXTERNOS fill:#EAECEE,stroke:#34495E,stroke-width:2px,color:#1C2833,stroke-dasharray: 5 5