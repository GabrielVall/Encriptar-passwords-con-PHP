# Encriptar-passwords-con-PHP
#### Para encriptar la contraseña utilizaremos la función `password_hash()` de php, aquí puedes ver la documentación sobre la encriptación [PHP: password_hash](https://www.php.net/manual/en/function.password-hash.php).
#### Para hacer la encriptación solo necesitaremos poner nuestra contraseña adentro de la funcion, por ejemplo: `password_hash($_POST['password'], PASSWORD_DEFAULT)`
#### Nota: Tenga en cuenta que esta encriptación está diseñada para cambiar con el tiempo a medida que se agregan algoritmos nuevos y más fuertes a PHP. Por ese motivo, la longitud del resultado del uso de este identificador puede cambiar con el tiempo. Por lo tanto, se recomienda almacenar el resultado en una columna de la base de datos que pueda expandirse más allá de los 60 caracteres (255 caracteres sería una buena opción).

## Parameters

### Password
La contraseña del usuario

Precaución
Usar `PASSWORD_BCRYPT` como algoritmo, dará como resultado que el parámetro de contraseña se trunque a una longitud máxima de 72 bytes.

### Algo
Nota: Algo se refiere a algoritmo.
Una constante de algoritmo de contraseña que denota el algoritmo que se usará al codificar la contraseña.

### Options
Una matriz asociativa que contiene opciones. Consulte las constantes del algoritmo de contraseña para obtener documentación sobre las opciones admitidas para cada algoritmo.
Si se omite, se creará una sal aleatoria y se utilizará el costo predeterminado.

## Ejemplo
```PHP
<?php
session_start(); // Inicializamos la sesión
include_once("../m/SQLConexion.php"); // incluimos nuestra conexión a la base de datos
$sql = new SQLConexion();
$hashed_password = password_hash($_POST['password'], PASSWORD_DEFAULT); // Encriptamos la contraseña
$rpta = $sql->obtenerResultadoID("CALL sp_crear_usuario('".$_POST['usuario'] ."','".$hashed_password ."',@_ID)");

if ($rpta[0][0] > 0) {
    // Código cuando la consulta se ejecute con exito
    $response_array['status'] = 'completado';
} elseif($rpta[0][0] == 0){
    // Código cuando de un error
    $response_array['status'] = 'error, correo registrado';
}else{
  // Código para cualquier otro error
  $response_array['msg'] = 'Ha ocurrido un error interno.';
}
header('Content-type: application/json');
echo json_encode($response_array);
```
