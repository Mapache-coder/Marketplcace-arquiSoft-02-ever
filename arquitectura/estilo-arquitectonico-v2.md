# Estilo Arquitectónico: Monolito Modular en Capas

## 1. Descripción del Estilo
Para el sistema de Marketplace, se ha seleccionado el estilo arquitectónico de **Monolito modular combinado con arquitectura en capas**. 
* **Monolito:** Toda la aplicación backend se despliega como una única unidad.
* **Modular:** Las funcionalidades están separadas en módulos independientes (Usuarios, Sellers, Catálogo, Carrito, Pedidos).
* **En Capas:** Cada módulo está organizado lógicamente en capas de presentación, lógica de negocio y datos.

## 2. Diagrama de Arquitectura (Estructura Global)

```mermaid
flowchart TD
    %% Estilos basados en la imagen
    classDef actor fill:#ffffff,stroke:#000000,stroke-width:1px;
    classDef web fill:#ffffff,stroke:#000000,stroke-width:1px;
    classDef mid fill:#e8f0fe,stroke:#a6c8ff,stroke-width:1px;
    classDef pres fill:#ffffff,stroke:#a6c8ff,stroke-width:1px;
    classDef neg fill:#e6f4ea,stroke:#81c995,stroke-width:1px;
    classDef dat fill:#ffffff,stroke:#fbbc04,stroke-width:1px;
    classDef orm fill:#fef7e0,stroke:#fbbc04,stroke-width:1px;
    classDef ext fill:#f1f3f4,stroke:#9aa0a6,stroke-width:1px;

    %% Actores
    C([Cliente]):::actor
    S([Seller]):::actor
    A([Administrador]):::actor

    %% Frontend
    UI["Cliente Web <br/> [Navegador - HTML / CSS / JavaScript]"]:::web
    C --> UI
    S --> UI
    A --> UI

    %% Backend - Monolito
    subgraph Backend ["<<monolito>> Marketplace Backend [Node.js 20 LTS - Express] <br/> Una sola aplicación - un solo proceso - un solo despliegue"]
        
        MW["Middlewares Express (transversales) cors - express.json() - auth (JWT) - validación de entrada - manejo de errores - logger"]:::mid

        %% Organización por Módulos (Columnas)
        subgraph M_Usu ["módulo usuarios <br/> src/modules/usuarios/"]
            direction TB
            U_R["usuarios.routes.js"]:::pres
            U_C["usuarios.controller.js"]:::pres
            U_S["usuarios.service.js <br/> registro, login, roles"]:::neg
            U_Repo["usuarios.repository.js"]:::dat
            U_R --> U_C --> U_S --> U_Repo
        end

        subgraph M_Sel ["módulo sellers <br/> src/modules/sellers/"]
            direction TB
            S_R["sellers.routes.js"]:::pres
            S_C["sellers.controller.js"]:::pres
            S_S["sellers.service.js <br/> alta de tiendas, validación"]:::neg
            S_Repo["sellers.repository.js"]:::dat
            S_R --> S_C --> S_S --> S_Repo
        end

        subgraph M_Cat ["módulo catalogo <br/> src/modules/catalogo/"]
            direction TB
            Cat_R["catalogo.routes.js"]:::pres
            Cat_C["catalogo.controller.js"]:::pres
            Cat_S["catalogo.service.js <br/> productos, categorías, stock"]:::neg
            Cat_Repo["catalogo.repository.js"]:::dat
            Cat_R --> Cat_C --> Cat_S --> Cat_Repo
        end

        subgraph M_Car ["módulo carrito <br/> src/modules/carrito/"]
            direction TB
            Car_R["carrito.routes.js"]:::pres
            Car_C["carrito.controller.js"]:::pres
            Car_S["carrito.service.js <br/> ítems, totales"]:::neg
            Car_Repo["carrito.repository.js"]:::dat
            Car_R --> Car_C --> Car_S --> Car_Repo
        end

        subgraph M_Ped ["módulo pedidos <br/> src/modules/pedidos/"]
            direction TB
            P_R["pedidos.routes.js"]:::pres
            P_C["pedidos.controller.js"]:::pres
            P_S["pedidos.service.js <br/> checkout, estados, pago/envío"]:::neg
            P_Repo["pedidos.repository.js"]:::dat
            P_R --> P_C --> P_S --> P_Repo
        end
        
        ORM["Acceso a datos compartido Sequelize (ORM) - modelos - pool de conexiones (src/shared/db)"]:::orm
        
        %% Conexiones desde Middlewares a las rutas
        MW --> U_R
        MW --> S_R
        MW --> Cat_R
        MW --> Car_R
        MW --> P_R

        %% Conexiones de repositorios al ORM
        U_Repo --> ORM
        S_Repo --> ORM
        Cat_Repo --> ORM
        Car_Repo --> ORM
        P_Repo --> ORM

        %% Flechas punteadas (Uso entre módulos)
        S_S -.-> U_S
        P_S -.-> Car_S
        P_S -.-> Cat_S
        Car_S -.-> Cat_S
    end

    %% Conexión Web a Backend
    UI -- "- HTTPS - JSON - <br/> /api/v1/*" --> MW

    %% Base de Datos
    DB[("PostgreSQL <br/> marketplace_db")]:::actor
    ORM -- "- SQL - TCP 5432 -" --> DB

    %% Sistemas Externos
    Ext_P["<<sistema externo>> <br/> Pasarela de pagos <br/> (p. ej. Culqi / Niubiz)"]:::ext
    Ext_E["<<sistema externo>> <br/> Servicio de envíos <br/> (API del courier)"]:::ext

    P_S -- "HTTPS / REST" --> Ext_P
    P_S -- "HTTPS / REST" --> Ext_E