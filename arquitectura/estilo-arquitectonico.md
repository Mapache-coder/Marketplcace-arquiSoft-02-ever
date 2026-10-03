# Estilo Arquitectónico: Monolito Modular en Capas

## 1. Descripción del Estilo
Para el sistema de Marketplace, se ha seleccionado el estilo arquitectónico de **Monolito modular combinado con arquitectura en capas**. 
* **Monolito:** Toda la aplicación backend se despliega como una única unidad.
* **Modular:** Las funcionalidades están separadas en módulos independientes (Usuarios, Sellers, Catálogo, Carrito, Pedidos).
* **En Capas:** Cada módulo está organizado lógicamente en Capa de Presentación, Capa de Lógica de Negocio y Capa de Datos.

## 2. Diagrama de Arquitectura (Estructura Global)

```mermaid
flowchart TD
    %% Definición de estilos
    classDef actor fill:#eceff1,stroke:#78909c,stroke-width:1px;
    classDef web fill:#ffffff,stroke:#b0bec5,stroke-width:2px;
    classDef mid fill:#f5f5f5,stroke:#9e9e9e,stroke-width:1px,stroke-dasharray: 5 5;
    classDef pres fill:#e3f2fd,stroke:#64b5f6,stroke-width:2px;
    classDef neg fill:#e8f5e9,stroke:#81c784,stroke-width:2px;
    classDef dat fill:#fbe9e7,stroke:#ff8a65,stroke-width:2px;
    classDef orm fill:#fff3e0,stroke:#ffb74d,stroke-width:1px;
    classDef ext fill:#eceff1,stroke:#607d8b,stroke-width:2px;

    %% Actores
    C([Cliente]):::actor
    S([Seller]):::actor
    A([Administrador]):::actor

    %% Frontend
    UI["Cliente Web <br/> Navegador / HTML / CSS / JS"]:::web
    C --> UI
    S --> UI
    A --> UI

    %% Backend
    subgraph Backend ["Monolito Marketplace Backend - Node.js 20 LTS - Express"]
        direction TB
        MW["Middlewares Express transversales <br/> cors, express.json(), auth JWT, validación, manejo de errores, logger"]:::mid

        subgraph Presentacion ["1. CAPA DE PRESENTACIÓN - Recibe peticiones HTTP"]
            direction LR
            C1[usuarios.controller]:::pres
            C2[sellers.controller]:::pres
            C3[catalogo.controller]:::pres
            C4[carrito.controller]:::pres
            C5[pedidos.controller]:::pres
        end

        subgraph Negocio ["2. CAPA DE LÓGICA DE NEGOCIO - Reglas de negocio"]
            direction LR
            S1[usuarios.service]:::neg
            S2[sellers.service]:::neg
            S3[catalogo.service]:::neg
            S4[carrito.service]:::neg
            S5[pedidos.service]:::neg
        end

        subgraph Datos ["3. CAPA DE DATOS - Persistencia"]
            direction LR
            R1[usuarios.repository]:::dat
            R2[sellers.repository]:::dat
            R3[catalogo.repository]:::dat
            R4[carrito.repository]:::dat
            R5[pedidos.repository]:::dat
        end
        
        ORM["Acceso a datos compartidos <br/> Sequelize ORM / Modelos / Pool de conexiones"]:::orm

        %% Flujo Vertical Principal (Llamadas síncronas)
        MW --> Presentacion
        C1 --> S1
        C2 --> S2
        C3 --> S3
        C4 --> S4
        C5 --> S5

        S1 --> R1
        S2 --> R2
        S3 --> R3
        S4 --> R4
        S5 --> R5
        
        R1 & R2 & R3 & R4 & R5 --> ORM

        %% Flujo Horizontal (Comunicación entre módulos)
        S4 -.->|"Usa módulo"| S3
        S5 -.->|"Usa módulo"| S4
    end

    UI -- "HTTPS / JSON / REST" --> MW

    %% Base de Datos y Externos
    DB[(PostgreSQL)]:::ext
    ORM -- "SQL / TCP 5432" --> DB

    Pasarela[["Pasarela de Pagos"]]:::ext
    Envios[["Servicio de Envíos"]]:::ext

    S5 -- "HTTPS / REST" --> Pasarela
    S5 -- "HTTPS / REST" --> Envios