# Taller_Mensajeria_MYSQL

````sql
CREATE DATABASE IF NOT EXISTS mensajeria;
````

````sql
USE mensajeria;
````

````sql
CREATE TABLE usuarios (
    usuario_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    contrasena VARCHAR(100) NOT NULL,
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE conversaciones (
    conversacion_id INT AUTO_INCREMENT PRIMARY KEY,
    nombre_conversacion VARCHAR(100) NULL,
    fecha_creacion TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE participantes (
    participante_id INT AUTO_INCREMENT PRIMARY KEY,
    conversacion_id INT NOT NULL,
    usuario_id INT NOT NULL,
    fecha_union TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (conversacion_id) REFERENCES conversaciones(conversacion_id) ON DELETE CASCADE,
    FOREIGN KEY (usuario_id) REFERENCES usuarios(usuario_id) ON DELETE CASCADE
);

CREATE TABLE mensajes (
    mensaje_id INT AUTO_INCREMENT PRIMARY KEY,
    conversacion_id INT NOT NULL,
    remitente_id INT NOT NULL,
    contenido TEXT NOT NULL,
    fecha_envio TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (conversacion_id) REFERENCES conversaciones(conversacion_id) ON DELETE CASCADE,
    FOREIGN KEY (remitente_id) REFERENCES usuarios(usuario_id) ON DELETE CASCADE
);
````


## Inserciones

````sql
INSERT INTO usuarios (nombre, email, contrasena)
VALUES ('Juan Pérez', 'juan.perez@example.com', 'password123'),
('María López', 'maria.lopez@example.com', 'maria1234'),
('Carlos Martínez', 'carlos.martinez@example.com', 'carlo_pass'),
('Ana Fernández', 'ana.fernandez@example.com', 'ana2023'),
('Luis Gómez', 'luis.gomez@example.com', 'luis_pass'),
('Lucía Rivera', 'lucia.rivera@example.com', 'lucia2023'),
('Pedro Ortiz', 'pedro.ortiz@example.com', 'pedro_pass'),
('Sofía Blanco', 'sofia.blanco@example.com', 'sofiablanco123'),
('Miguel Rojas', 'miguel.rojas@example.com', 'miguerojas2023'),
('Carla Ruiz', 'carla.ruiz@example.com', 'carlitaaruiz');
````

## Consultas

- VER TABLA
````sql
SELECT * FROM usuarios;
````

1). Cree un procedimiento almacenado que permita insertar un nuevo usuario retornando un mensaje que indique si la inserción fue satisfactoria.
````sql
DELIMITER //

CREATE PROCEDURE insertar_usuario (
    IN p_nombre VARCHAR(50),
    IN p_email VARCHAR(100),
    IN p_contrasena VARCHAR(100)
)
BEGIN
    DECLARE exit handler for SQLEXCEPTION 
    BEGIN
        ROLLBACK;
        SELECT 'Error: No se pudo insertar el usuario. El correo ya existe o hay otro problema.' AS mensaje;
    END;
    START TRANSACTION;
    INSERT INTO usuarios (nombre, email, contrasena)
    VALUES (p_nombre, p_email, p_contrasena);
    COMMIT;
    SELECT 'Inserción satisfactoria.' AS mensaje;
END //

DELIMITER ;

CALL insertar_usuario('Karen', 'karenlorenacriscaceres@gmail.com', 'kl123');
````

2). Cree un procedimiento almacenado que permita eliminar un usuario retornando un mensaje que indique si la inserción fue satisfactoria.
````sql
DELIMITER //

CREATE PROCEDURE eliminar_usuario (
    IN p_usuario_id INT
)
BEGIN
    DECLARE exit handler for SQLEXCEPTION 
    BEGIN
        ROLLBACK;
        SELECT 'Error: No se pudo eliminar el usuario. El usuario no existe o hay otro problema.' AS mensaje;
    END;
    START TRANSACTION;
    DELETE FROM usuarios WHERE usuario_id = p_usuario_id;
    COMMIT;
    SELECT 'Eliminación satisfactoria.' AS mensaje;
END //

DELIMITER ;

CALL eliminar_usuario(id);
````

3). Cree un procedimiento almacenado que permita editar un usuario retornando un mensaje que indique si la inserción fue satisfactoria.
````sql
DELIMITER //

CREATE PROCEDURE editar_usuario (
    IN p_usuario_id INT,
    IN p_nombre VARCHAR(50),
    IN p_email VARCHAR(100),
    IN p_contrasena VARCHAR(100)
)
BEGIN
    DECLARE exit handler for SQLEXCEPTION 
    BEGIN
        ROLLBACK;
        SELECT 'Error: No se pudo actualizar el usuario. El correo ya existe o hay otro problema.' AS mensaje;
    END;
    START TRANSACTION;
    UPDATE usuarios 
    SET nombre = p_nombre, email = p_email, contrasena = p_contrasena
    WHERE usuario_id = p_usuario_id;
    COMMIT;
    SELECT 'Actualización satisfactoria.' AS mensaje;
END //

DELIMITER ;

CALL editar_usuario(2, 'Lorena', 'lorena.editado@gmail.com', '323kl');
````

4). Cree un procedimiento almacenado que permita buscar un usuario por su nombre.
````sql
DELIMITER //

CREATE PROCEDURE buscar_usuario_por_nombre (
    IN p_nombre VARCHAR(50)
)
BEGIN
    SELECT * FROM usuarios WHERE nombre LIKE CONCAT('%', p_nombre, '%');
END //

DELIMITER ;

CALL buscar_usuario_por_nombre('Juan');
````

5). Realice una consulta que permita iniciar una conversación.
````sql
INSERT INTO conversaciones (nombre_conversacion)
VALUES ('Holaa');

SELECT LAST_INSERT_ID() AS conversacion_id;
````
- VER TABLA
````sql
SELECT * FROM conversaciones;
````

