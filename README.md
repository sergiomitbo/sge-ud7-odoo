# SGE · UD7 — Odoo de principio a fin

Tarea UD7 de **Sistemas de Gestión Empresarial** (2.º DAM, MEDAC): instalación de Odoo, usuarios, grupos y privilegios, y un proceso de venta completo del presupuesto a la factura.

<img src="empresa/logo.png" alt="Logo de Zumbido" width="360">

| Dato | Valor |
|---|---|
| **Alumno** | Sergio Mitchell Bocero |
| **Empresa** | Zumbido Miel Urbana, S.L. (empresa ficticia de apicultura urbana en Madrid) |
| **ERP** | Odoo 19.0 Community, instalador oficial de Windows |
| **Acceso** | `http://localhost:8069` · base de datos `zumbido`, sin datos de demostración |
| **Apps** | Ventas, Facturación, Inventario y Contactos |
| **Vídeo** | No incluido: la comprobación de permisos y la venta se documentan con capturas |
| **Entrega** | 05/11/2026 |

## Índice

1. **Empresa** — [ficha de la empresa y catálogo](empresa/ficha-empresa.md) · [logo](empresa/logo.png)
2. **Instalación** — [diapositivas del proceso (PDF)](instalacion/instalacion-odoo.pdf) · [errores encontrados](instalacion/errores-instalacion.md)
3. **Usuarios, grupos y permisos** — [plantilla de usuarios, grupo propio y comprobación de permisos](usuarios/usuarios-grupos-permisos.md)
4. **Proceso de venta** — [paso a paso con usuarios y referencias](venta/proceso-venta.md) · [presupuesto S00002 (PDF)](venta/presupuesto.pdf) · [factura INV/2026/00001 (PDF)](venta/factura.pdf)
5. **Capturas** — [capturas/](capturas/): instalación y empresa, usuarios y grupo propio, comprobación de permisos con cada usuario y cada paso de la venta

## Resumen

- **Instalación:** Odoo 19 Community con el instalador de Windows, que trae Python, PostgreSQL 16 y wkhtmltopdf y deja Odoo como servicio en el puerto 8069. Los avisos que salieron (Odoo arrancando antes que PostgreSQL y apps que se instalan solas) están explicados en el documento de errores.
- **Empresa:** Zumbido vende miel cruda de sus colmenas en azoteas de Madrid, velas de cera, material para apicultores y talleres. Tiene logo, colores y documentos propios, 7 productos con imagen (5 almacenables con stock) y 2 clientes, una empresa y una particular.
- **Usuarios:** cuatro empleados internos con el mínimo privilegio para su puesto (comercial, responsable de ventas, administración y almacén), el administrador y una usuaria portal del cliente Bistró Azotea 21. El grupo propio *Zumbido / Facturación de pedidos* deja a administración facturar los pedidos sin poder vender.
- **Venta:** presupuesto S00002 (Lucía) → pedido (Sergio) → entrega WH/OUT/00001 (Andrés) → factura INV/2026/00001 y pago PBNK1/2026/00001 (Marta). Total 845,06 € y el pedido queda completamente facturado.

## Estructura del repositorio

```
sge-ud7-odoo/
├── README.md
├── empresa/        ficha de la empresa, logo e imágenes de los productos
├── instalacion/    diapositivas de la instalación (PDF y PPTX) y errores
├── usuarios/       usuarios, grupo propio y comprobación de permisos
├── venta/          proceso de venta, presupuesto.pdf y factura.pdf
└── capturas/       capturas de apoyo de cada parte
```

Las contraseñas de los usuarios no están en el repositorio: solo aparecen los correos de acceso.
