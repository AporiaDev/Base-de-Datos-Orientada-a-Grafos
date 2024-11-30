# Base-de-Datos-Orientada-a-Grafos

En un proyecto de desarrollo de software en Colombia se esta construyendo una plataforma de comunicación para una
empresa con un gran número de empleados distribuidos a lo largo del país, se quiere crear una red social corporativa
donde los usuarios tengan sus páginas personales, en las que pueden incluir información básica y de contacto,
adicionalmente los usuarios pueden hacer publicaciones en una especie de muro personal, la plataforma también tiene
la opción de agregar a otros usuarios como amigos, también se busca que se pueda establecerá la cadena de mando de
las personas por lo tanto es necesario poder incluir información sobre quien es el jefe de quien y que cargo ocupa en la
empresa
Finalmente y buscando hacer seguimiento a los casos de acoso laboral el comité de ética de la organización quiere
mapear los reportes de acoso reportado para algunos empleados, para determinar si existen relaciones de acoso por
parte de empleados que tengan cargos importantes en la empresa y que aprovechen esta posición para ello.
A continuación se presenta la descripción base de entidades y relaciones del problema

## Construcción de entidades

### Empleado : nombre, apellidos, edad, documento, codigo_empresa, cargo
    CREATE (:Empleado {nombre: 'Juan', apellidos: 'Pérez', edad: 45, documento: '123456', codigo_empresa: 'EMP001', cargo: 'Gerente'});
    CREATE (:Empleado {nombre: 'María', apellidos: 'Gómez', edad: 40, documento: '654321', codigo_empresa: 'EMP002', cargo: 'Línea Regional'});
    CREATE (:Empleado {nombre: 'Carlos', apellidos: 'Díaz', edad: 35, documento: '111222', codigo_empresa: 'EMP003', cargo: 'Líder Seccional'});
    CREATE (:Empleado {nombre: 'Ana', apellidos: 'Ruiz', edad: 30, documento: '987654', codigo_empresa: 'EMP004', cargo: 'Administrador'});
    CREATE (:Empleado {nombre: 'Luis', apellidos: 'Hernández', edad: 25, documento: '456789', codigo_empresa: 'EMP005', cargo: 'Empleado'});
### Post : fecha, titulo, descripcion, likes
    MATCH (empleado:Empleado {nombre: 'Juan'})
    CREATE (post:Post {fecha: date('2024-11-30'), titulo: 'Estrategia Corporativa', descripcion: 'Nueva estrategia para el próximo año', likes: 0})
    CREATE (empleado)-[:PUBLICA]->(post);
### Comentario : fecha, descripcion
    MATCH (post:Post {titulo: 'Estrategia Corporativa'}), (empleado:Empleado {nombre: 'Carlos'})
    CREATE (comentario:Comentario {fecha: date('2024-11-30'), descripcion: 'Excelente propuesta, muy interesante'})
    CREATE (post)-[:TIENE_COMENTARIO]->(comentario);
### Reporte de Acoso: fecha, naturaleza
    MATCH (reportero:Empleado {nombre: 'Carlos'}), (acusado:Empleado {nombre: 'Juan'})
    CREATE (reporte:ReporteDeAcoso {fecha: date('2024-11-30'), naturaleza: 'persecución'})
    CREATE (reportero)-[:REPORTA_ACOSO]->(reporte)
    CREATE (reporte)-[:ACUSADO_ES]->(acusado);

## Construcción de relaciones

### Jerarquía entre empleados (respetando los roles)
    (Empleado)-[:ES_JEFE_DE]->(Empleado)
               Gerente → Línea regional → Líder seccional → Administrador → Empleado.
               
### Publicación de posts
    (Empleado)-[:PUBLICA]->(Post)
              Indica qué empleado publica un post.

### Me gusta en posts
    (Empleado)-[:DA_LIKE_A]->(Post)
              Relación para registrar que un empleado da me gusta a un post.

### Comentarios en posts
    (Post)-[:TIENE_COMENTARIO]->(Comentario)
              Conecta comentarios al post principal.

### Amistad entre empleados:
    (Empleado)-[:ES_AMIGO_DE]->(Empleado)
              Relación bidireccional para la amistad entre empleados.

### Reportes de acoso
    (Empleado)-[:REPORTA_ACOSO]->(ReporteDeAcoso)
              Relación de un empleado que reporta un caso de acoso.

    (ReporteDeAcoso)-[:ACUSADO_ES]->(Empleado)
              Relación para identificar al empleado acusado en un reporte de acoso.

## Creación de esquemas

### Empleados
![image](https://github.com/user-attachments/assets/db7f117b-a04a-47ae-a835-6fc2ca68e76d)

Comandos

    CREATE (:Empleado {nombre: 'Juan', apellidos: 'Pérez', edad: 45, documento: '123456', codigo_empresa: 'EMP001', cargo: 'Gerente'});
    CREATE (:Empleado {nombre: 'María', apellidos: 'Gómez', edad: 40, documento: '654321', codigo_empresa: 'EMP002', cargo: 'Línea Regional'});
    CREATE (:Empleado {nombre: 'Carlos', apellidos: 'Díaz', edad: 35, documento: '111222', codigo_empresa: 'EMP003', cargo: 'Líder Seccional'});
    CREATE (:Empleado {nombre: 'Ana', apellidos: 'Ruiz', edad: 30, documento: '987654', codigo_empresa: 'EMP004', cargo: 'Administrador'});
    CREATE (:Empleado {nombre: 'Luis', apellidos: 'Hernández', edad: 25, documento: '456789', codigo_empresa: 'EMP005', cargo: 'Empleado'});
    


### Jerarquía
![image](https://github.com/user-attachments/assets/dd4623ce-0a03-4f9c-86d2-7f2bec7a264a)

Comandos 
  
    MATCH (gerente:Empleado {cargo: 'Gerente'}), (linea:Empleado {cargo: 'Línea Regional'})
    CREATE (gerente)-[:ES_JEFE_DE]->(linea);

    MATCH (linea:Empleado {cargo: 'Línea Regional'}), (lider:Empleado {cargo: 'Líder Seccional'})
    CREATE (linea)-[:ES_JEFE_DE]->(lider);

    MATCH (lider:Empleado {cargo: 'Líder Seccional'}), (admin:Empleado {cargo: 'Administrador'})
    CREATE (lider)-[:ES_JEFE_DE]->(admin);

    MATCH (admin:Empleado {cargo: 'Administrador'}), (empleado:Empleado {cargo: 'Empleado'})
    CREATE (admin)-[:ES_JEFE_DE]->(empleado);

### Post 

![image](https://github.com/user-attachments/assets/8afcdca2-46ff-4b62-883f-0fe00e9f7e12)

Comandos
  
    MATCH (empleado:Empleado {nombre: 'Juan'})
    CREATE (post:Post {fecha: date('2024-11-30'), titulo: 'Estrategia Corporativa', descripcion: 'Nueva estrategia para el próximo año', likes: 0})
    CREATE (empleado)-[:PUBLICA]->(post);

### Comentario

![image](https://github.com/user-attachments/assets/05009fa7-fa7c-4bd4-bf09-42ab71a638c3)

Comandos

    MATCH (post:Post {titulo: 'Estrategia Corporativa'}), (empleado:Empleado {nombre: 'Carlos'})
    CREATE (comentario:Comentario {fecha: date('2024-11-30'), descripcion: 'Excelente propuesta, muy interesante'})
    CREATE (post)-[:TIENE_COMENTARIO]->(comentario);



    






    
    
