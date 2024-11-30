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
    CREATE (:Empleado {nombre: 'Juan', apellidos: 'Pérez', edad: 50, documento: '1001', codigo_empresa: 'EMP001', cargo: 'Gerente'});
    CREATE (:Empleado {nombre: 'María', apellidos: 'López', edad: 45, documento: '1002', codigo_empresa: 'EMP002', cargo: 'Línea Regional'});
    CREATE (:Empleado {nombre: 'Carlos', apellidos: 'Gómez', edad: 42, documento: '1003', codigo_empresa: 'EMP003', cargo: 'Líder Seccional'});
    CREATE (:Empleado {nombre: 'Ana', apellidos: 'Ruiz', edad: 35, documento: '1004', codigo_empresa: 'EMP004', cargo: 'Administrador'});
    CREATE (:Empleado {nombre: 'Luis', apellidos: 'Martínez', edad: 30, documento: '1005', codigo_empresa: 'EMP005', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Elena', apellidos: 'Ramírez', edad: 28, documento: '1006', codigo_empresa: 'EMP006', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Ricardo', apellidos: 'Díaz', edad: 26, documento: '1007', codigo_empresa: 'EMP007', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Carmen', apellidos: 'Torres', edad: 40, documento: '1008', codigo_empresa: 'EMP008', cargo: 'Administrador'});
    CREATE (:Empleado {nombre: 'Diego', apellidos: 'Mejía', edad: 38, documento: '1009', codigo_empresa: 'EMP009', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Lorena', apellidos: 'Castro', edad: 27, documento: '1010', codigo_empresa: 'EMP010', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Pedro', apellidos: 'Mora', edad: 45, documento: '1011', codigo_empresa: 'EMP011', cargo: 'Línea Regional'});
    CREATE (:Empleado {nombre: 'Jorge', apellidos: 'Salinas', edad: 41, documento: '1012', codigo_empresa: 'EMP012', cargo: 'Líder Seccional'});
    CREATE (:Empleado {nombre: 'Sandra', apellidos: 'Velásquez', edad: 32, documento: '1013', codigo_empresa: 'EMP013', cargo: 'Administrador'});
    CREATE (:Empleado {nombre: 'Sofía', apellidos: 'Fernández', edad: 25, documento: '1014', codigo_empresa: 'EMP014', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Camilo', apellidos: 'Suárez', edad: 24, documento: '1015', codigo_empresa: 'EMP015', cargo: 'Empleado'});
    
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
![image](https://github.com/user-attachments/assets/69b11cfc-ba8e-4335-9d7a-02e152bea5c4)


Comandos

    CREATE (:Empleado {nombre: 'Juan', apellidos: 'Pérez', edad: 50, documento: '1001', codigo_empresa: 'EMP001', cargo: 'Gerente'});
    CREATE (:Empleado {nombre: 'María', apellidos: 'López', edad: 45, documento: '1002', codigo_empresa: 'EMP002', cargo: 'Línea Regional'});
    CREATE (:Empleado {nombre: 'Carlos', apellidos: 'Gómez', edad: 42, documento: '1003', codigo_empresa: 'EMP003', cargo: 'Líder Seccional'});
    CREATE (:Empleado {nombre: 'Ana', apellidos: 'Ruiz', edad: 35, documento: '1004', codigo_empresa: 'EMP004', cargo: 'Administrador'});
    CREATE (:Empleado {nombre: 'Luis', apellidos: 'Martínez', edad: 30, documento: '1005', codigo_empresa: 'EMP005', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Elena', apellidos: 'Ramírez', edad: 28, documento: '1006', codigo_empresa: 'EMP006', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Ricardo', apellidos: 'Díaz', edad: 26, documento: '1007', codigo_empresa: 'EMP007', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Carmen', apellidos: 'Torres', edad: 40, documento: '1008', codigo_empresa: 'EMP008', cargo: 'Administrador'});
    CREATE (:Empleado {nombre: 'Diego', apellidos: 'Mejía', edad: 38, documento: '1009', codigo_empresa: 'EMP009', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Lorena', apellidos: 'Castro', edad: 27, documento: '1010', codigo_empresa: 'EMP010', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Pedro', apellidos: 'Mora', edad: 45, documento: '1011', codigo_empresa: 'EMP011', cargo: 'Línea Regional'});
    CREATE (:Empleado {nombre: 'Jorge', apellidos: 'Salinas', edad: 41, documento: '1012', codigo_empresa: 'EMP012', cargo: 'Líder Seccional'});
    CREATE (:Empleado {nombre: 'Sandra', apellidos: 'Velásquez', edad: 32, documento: '1013', codigo_empresa: 'EMP013', cargo: 'Administrador'});
    CREATE (:Empleado {nombre: 'Sofía', apellidos: 'Fernández', edad: 25, documento: '1014', codigo_empresa: 'EMP014', cargo: 'Empleado'});
    CREATE (:Empleado {nombre: 'Camilo', apellidos: 'Suárez', edad: 24, documento: '1015', codigo_empresa: 'EMP015', cargo: 'Empleado'});


### Jerarquía
![image](https://github.com/user-attachments/assets/5971c20c-7a59-482a-af0c-c77d5db1e535)


Comandos 
  
    MATCH (gerente:Empleado {cargo: 'Gerente'}), (linea2:Empleado {documento: '1011'}) CREATE (gerente)-[:ES_JEFE_DE]->(linea2);
    MATCH (linea2:Empleado {documento: '1011'}), (lider2:Empleado {documento: '1012'}) CREATE (linea2)-[:ES_JEFE_DE]->(lider2);
    MATCH (lider2:Empleado {documento: '1012'}), (admin2:Empleado {documento: '1013'}) CREATE (lider2)-[:ES_JEFE_DE]->(admin2);
    MATCH (admin2:Empleado {documento: '1013'}), (empleado2:Empleado {documento: '1014'}) CREATE (admin2)-[:ES_JEFE_DE]->(empleado2);
    MATCH (lider1:Empleado {documento: '1003'}), (admin3:Empleado {documento: '1008'}) CREATE (lider1)-[:ES_JEFE_DE]->(admin3);
    MATCH (admin3:Empleado {documento: '1008'}), (empleado3:Empleado {documento: '1009'}) CREATE (admin3)-[:ES_JEFE_DE]->(empleado3);
    MATCH (lider2:Empleado {documento: '1012'}), (admin4:Empleado {documento: '1004'}) CREATE (lider2)-[:ES_JEFE_DE]->(admin4);
    MATCH (admin4:Empleado {documento: '1004'}), (empleado4:Empleado {documento: '1005'}) CREATE (admin4)-[:ES_JEFE_DE]->(empleado4);
    MATCH (lider1:Empleado {documento: '1003'}), (admin5:Empleado {documento: '1013'}) CREATE (lider1)-[:ES_JEFE_DE]->(admin5);
    MATCH (admin5:Empleado {documento: '1013'}), (empleado5:Empleado {documento: '1010'}) CREATE (admin5)-[:ES_JEFE_DE]->(empleado5);
    MATCH (lider2:Empleado {documento: '1012'}), (admin6:Empleado {documento: '1008'}) CREATE (lider2)-[:ES_JEFE_DE]->(admin6);
    MATCH (admin6:Empleado {documento: '1008'}), (empleado6:Empleado {documento: '1009'}) CREATE (admin6)-[:ES_JEFE_DE]->(empleado6);
    MATCH (gerente:Empleado {cargo: 'Gerente'}), (linea3:Empleado {documento: '1002'}) CREATE (gerente)-[:ES_JEFE_DE]->(linea3);
    MATCH (linea3:Empleado {documento: '1002'}), (lider3:Empleado {documento: '1003'}) CREATE (linea3)-[:ES_JEFE_DE]->(lider3);
    MATCH (lider3:Empleado {documento: '1003'}), (admin7:Empleado {documento: '1004'}) CREATE (lider3)-[:ES_JEFE_DE]->(admin7);
    MATCH (admin7:Empleado {documento: '1004'}), (empleado7:Empleado {documento: '1005'}) CREATE (admin7)-[:ES_JEFE_DE]->(empleado7);

### Post 

![image](https://github.com/user-attachments/assets/c44b2337-e944-466c-9a74-dd5ddd3ad54d)


Comandos
  
    MATCH (empleado:Empleado {documento: '1002'}) CREATE (post3:Post {fecha: date('2024-11-28'), titulo: 'Plan de Marketing', descripcion:                'Estrategia de marketing para el próximo trimestre', likes: 8}) CREATE (empleado)-[:PUBLICA]->(post3);
    MATCH (empleado:Empleado {documento: '1003'}) CREATE (post4:Post {fecha: date('2024-11-27'), titulo: 'Capacitación de Personal', descripcion:         'Nuevo programa de capacitación', likes: 7}) CREATE (empleado)-[:PUBLICA]->(post4); 
    MATCH (empleado:Empleado {documento: '1004'}) CREATE (post5:Post {fecha: date('2024-11-26'), titulo: 'Innovación Tecnológica', descripcion:           'Implementación de nuevas tecnologías', likes: 9}) CREATE (empleado)-[:PUBLICA]->(post5);
    MATCH (empleado:Empleado {documento: '1008'}) CREATE (post6:Post {fecha: date('2024-11-25'), titulo: 'Responsabilidad Social', descripcion:           'Iniciativas de responsabilidad social', likes: 6}) CREATE (empleado)-[:PUBLICA]->(post6);

### Comentario

![image](https://github.com/user-attachments/assets/f2c583a1-9b0a-49e8-925a-8b1bae73ab04)

Comandos

    MATCH (post:Post {titulo: 'Plan de Marketing'}), (empleado:Empleado {documento: '1009'}) CREATE (coment3:Comentario {fecha: date('2024-11-28'),       descripcion: 'Gran idea, ¡cuenta conmigo!'}) CREATE (post)-[:TIENE_COMENTARIO]->(coment3);
    MATCH (post:Post {titulo: 'Capacitación de Personal'}), (empleado:Empleado {documento: '1010'}) CREATE (coment4:Comentario {fecha: date('2024-11-     27'), descripcion: 'Estoy ansiosa por aprender más.'}) CREATE (post)-[:TIENE_COMENTARIO]->(coment4);
    MATCH (post:Post {titulo: 'Innovación Tecnológica'}), (empleado:Empleado {documento: '1011'}) CREATE (coment5:Comentario {fecha: date('2024-11-       26'), descripcion: 'Esto mejorará nuestra eficiencia.'}) CREATE (post)-[:TIENE_COMENTARIO]->(coment5);
    MATCH (post:Post {titulo: 'Responsabilidad Social'}), (empleado:Empleado {documento: '1012'}) CREATE (coment6:Comentario {fecha: date('2024-11-       25'), descripcion: 'Orgulloso de ser parte de esto.'}) CREATE (post)-[:TIENE_COMENTARIO]->(coment6);

### Registro de Likes

![image](https://github.com/user-attachments/assets/96e4fa25-b6bf-4873-8781-d6ce0f5524a7)


Comandos

    MATCH (post:Post {titulo: 'Plan de Marketing'}), (empleado:Empleado {nombre: 'Sandra'}) CREATE (empleado)-[:DA_LIKE_A]->(post); 
    MATCH (post:Post {titulo: 'Plan de Marketing'}), (empleado:Empleado {nombre: 'Lorena'}) CREATE (empleado)-[:DA_LIKE_A]->(post);
    MATCH (post:Post {titulo: 'Plan de Marketing'}), (empleado:Empleado {nombre: 'Diego'}) CREATE (empleado)-[:DA_LIKE_A]->(post); 
    MATCH (post:Post {titulo: 'Capacitación de Personal'}), (empleado:Empleado {nombre: 'Ana'}) CREATE (empleado)-[:DA_LIKE_A]->(post);
    MATCH (post:Post {titulo: 'Capacitación de Personal'}), (empleado:Empleado {nombre: 'Carlos'}) CREATE (empleado)-[:DA_LIKE_A]->(post);
    MATCH (post:Post {titulo: 'Capacitación de Personal'}), (empleado:Empleado {nombre: 'Elena'}) CREATE (empleado)-[:DA_LIKE_A]->(post); 
    MATCH (post:Post {titulo: 'Innovación Tecnológica'}), (empleado:Empleado {nombre: 'Luis'}) CREATE (empleado)-[:DA_LIKE_A]->(post);
    MATCH (post:Post {titulo: 'Innovación Tecnológica'}), (empleado:Empleado {nombre: 'María'}) CREATE (empleado)-[:DA_LIKE_A]->(post);
    MATCH (post:Post {titulo: 'Innovación Tecnológica'}), (empleado:Empleado {nombre: 'Jorge'}) CREATE (empleado)-[:DA_LIKE_A]->(post);
    MATCH (post:Post {titulo: 'Responsabilidad Social'}), (empleado:Empleado {nombre: 'Sandra'}) CREATE (empleado)-[:DA_LIKE_A]->(post);
    MATCH (post:Post {titulo: 'Responsabilidad Social'}), (empleado:Empleado {nombre: 'Lorena'}) CREATE (empleado)-[:DA_LIKE_A]->(post);
    MATCH (post:Post {titulo: 'Responsabilidad Social'}), (empleado:Empleado {nombre: 'Diego'}) CREATE (empleado)-[:DA_LIKE_A]->(post);



### Amistad entre empleados

![image](https://github.com/user-attachments/assets/dbbcc8ad-9a54-4e01-91d0-4e2c534c8e39)



Comandos 


    MATCH (a:Empleado {nombre: 'Juan'}), (b:Empleado {nombre: 'María'})
    CREATE (a)-[:ES_AMIGO_DE]->(b)
    CREATE (b)-[:ES_AMIGO_DE]->(a);

    MATCH (a:Empleado {nombre: 'Carlos'}), (b:Empleado {nombre: 'Ana'})
    CREATE (a)-[:ES_AMIGO_DE]->(b)
    CREATE (b)-[:ES_AMIGO_DE]->(a);

    MATCH (a:Empleado {nombre: 'Luis'}), (b:Empleado {nombre: 'Elena'})
    CREATE (a)-[:ES_AMIGO_DE]->(b)
    CREATE (b)-[:ES_AMIGO_DE]->(a);

    MATCH (a:Empleado {nombre: 'Ricardo'}), (b:Empleado {nombre: 'Carmen'})
    CREATE (a)-[:ES_AMIGO_DE]->(b)
    CREATE (b)-[:ES_AMIGO_DE]->(a);

    MATCH (a:Empleado {nombre: 'Diego'}), (b:Empleado {nombre: 'Lorena'})
    CREATE (a)-[:ES_AMIGO_DE]->(b)
    CREATE (b)-[:ES_AMIGO_DE]->(a);

    MATCH (a:Empleado {nombre: 'Pedro'}), (b:Empleado {nombre: 'Jorge'})
    CREATE (a)-[:ES_AMIGO_DE]->(b)
    CREATE (b)-[:ES_AMIGO_DE]->(a);

    MATCH (a:Empleado {nombre: 'Sandra'}), (b:Empleado {nombre: 'Sofía'})
    CREATE (a)-[:ES_AMIGO_DE]->(b)
    CREATE (b)-[:ES_AMIGO_DE]->(a);

    MATCH (a:Empleado {nombre: 'Camilo'}), (b:Empleado {nombre: 'María'})
    CREATE (a)-[:ES_AMIGO_DE]->(b)
    CREATE (b)-[:ES_AMIGO_DE]->(a);


### Reporte de acoso
![image](https://github.com/user-attachments/assets/4da8c82b-bf36-4da6-91c2-480f6a41f85e)


**Acusado**


![image](https://github.com/user-attachments/assets/23686864-c81e-4542-bff9-16d785458383)


Comandos 

    // Reporte de acoso 1 
    MATCH (reportero:Empleado {nombre: 'Carlos'}), (acusado:Empleado {nombre: 'Juan'}) CREATE (reporte1:ReporteDeAcoso {fecha: date('2024-11-30'),        naturaleza: 'persecución'}) CREATE (reportero)-[:REPORTA_ACOSO]->(reporte1) CREATE (reporte1)-[:ACUSADO_ES]->(acusado); 
    // Reporte de acoso 2
    MATCH (reportero:Empleado {nombre: 'María'}), (acusado:Empleado {nombre: 'Luis'}) CREATE (reporte2:ReporteDeAcoso {fecha: date('2024-11-29'),         naturaleza: 'discriminación'}) CREATE (reportero)-[:REPORTA_ACOSO]->(reporte2) CREATE (reporte2)-[:ACUSADO_ES]->(acusado);
    // Reporte de acoso 3 
    MATCH (reportero:Empleado {nombre: 'Ana'}), (acusado:Empleado {nombre: 'Diego'}) CREATE (reporte3:ReporteDeAcoso {fecha: date('2024-11-28'),          naturaleza: 'acoso verbal'}) CREATE (reportero)-[:REPORTA_ACOSO]->(reporte3) CREATE (reporte3)-[:ACUSADO_ES]->(acusado);
    // Reporte de acoso 4 
    MATCH (reportero:Empleado {nombre: 'Elena'}), (acusado:Empleado {nombre: 'Carlos'}) CREATE (reporte4:ReporteDeAcoso {fecha: date('2024-11-27'),       naturaleza: 'acoso físico'}) CREATE (reportero)-[:REPORTA_ACOSO]->(reporte4) CREATE (reporte4)-[:ACUSADO_ES]->(acusado);
    // Reporte de acoso 5
    MATCH (reportero:Empleado {nombre: 'Jorge'}), (acusado:Empleado {nombre: 'Ricardo'}) CREATE (reporte5:ReporteDeAcoso {fecha: date('2024-11-26'),      naturaleza: 'persecución'}) CREATE (reportero)-[:REPORTA_ACOSO]->(reporte5) CREATE (reporte5)-[:ACUSADO_ES]->(acusado);

## Concultas

### Consultar el listado de POST de un empleado determinado
    MATCH (empleado:Empleado {nombre: 'Carlos'})-[:PUBLICA]->(post:Post)
    RETURN post.titulo AS Título, post.fecha AS Fecha, post.descripcion AS Descripción;

### Consultar los 3 Post más recientes publicados

    MATCH (post:Post)
    RETURN post.titulo AS Título, post.fecha AS Fecha, post.descripcion AS Descripción
    ORDER BY post.fecha DESC
    LIMIT 3;

### Consultar los 3 Post con más me gusta

    MATCH (post:Post)
    RETURN post.titulo AS Título, post.likes AS Likes, post.descripcion AS Descripción
    ORDER BY post.likes DESC
    LIMIT 3;

### Consultar el listado de empleados que tienen como jefe a un empleado determinado

    MATCH (jefe:Empleado {nombre: 'Juan'})-[:ES_JEFE_DE]->(subordinado:Empleado)
    RETURN subordinado.nombre AS Nombre, subordinado.apellidos AS Apellidos, subordinado.cargo AS Cargo;


### Consultar el listado de POST a los cuales un empleado le ha dado me gusta
    MATCH (empleado:Empleado {nombre: 'Sandra'})-[:DA_LIKE_A]->(post:Post)
    RETURN post.titulo AS Título, post.fecha AS Fecha, post.likes AS Likes;


### Consultar los empleados que tengan reportes de acoso laboral y que ocupen un cargo específico

    MATCH (reporte:ReporteDeAcoso)-[:ACUSADO_ES]->(empleado:Empleado {cargo: 'Empleado'})
    RETURN empleado.nombre AS Nombre, empleado.apellidos AS Apellidos, reporte.naturaleza AS Naturaleza, reporte.fecha AS Fecha;


### Consultar la lista de amigos de un empleado determinado y mostrar sus relaciones de amistad en el grafo
    MATCH (empleado:Empleado {nombre: 'Carlos'})-[:ES_AMIGO_DE]->(amigo:Empleado)
    RETURN amigo.nombre AS Nombre, amigo.apellidos AS Apellidos;

    MATCH (empleado:Empleado {nombre: 'Carlos'})-[:ES_AMIGO_DE]->(amigo:Empleado)
    RETURN empleado, amigo;


### Consultar el listado de los 3 empleados con más amigos
    MATCH (empleado:Empleado)-[:ES_AMIGO_DE]->(amigo:Empleado)
    RETURN empleado.nombre AS Nombre, empleado.apellidos AS Apellidos, COUNT(amigo) AS Cantidad_Amigos
    ORDER BY Cantidad_Amigos DESC
    LIMIT 3;

### Consultar los POST con más comentarios
    MATCH (post:Post)-[:TIENE_COMENTARIO]->(comentario:Comentario)
    RETURN post.titulo AS Título, COUNT(comentario) AS Cantidad_Comentarios
    ORDER BY Cantidad_Comentarios DESC;






    






    
    
