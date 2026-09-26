## Diseño de la Arquitectura en Capas

Organizar los módulos identificados anteriormente dentro de una **primera propuesta de arquitectura**, utilizando una arquitectura de tres capas.

```text
+---------------------------------------------------+
|                  PRESENTACIÓN                     |
|              Web / API / Interfaz                 |
+---------------------------------------------------+
                          ↓
+---------------------------------------------------+
|               LÓGICA DE NEGOCIO                   |
|                                                   |
|  Catálogo                                         |
|  Carrito                                          |
|  Pedidos                                          |
|  Sellers                                          |
|  Usuarios                                         |
+---------------------------------------------------+
                          ↓
+---------------------------------------------------+
|                     DATOS                         |
|                 Base de datos                     |
+---------------------------------------------------+