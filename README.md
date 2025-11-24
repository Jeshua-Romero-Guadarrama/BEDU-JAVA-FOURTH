# 🩺 Sistema de Encuestas HIMFG (POO)

Proyecto en Java 11 para el Hospital Infantil de Mexico "Federico Gomez", enfocado en el Departamento de Cuidados Paliativos y Calidad de Vida. Gestiona pacientes y aplica encuestas de control de sintomas y bienestar.

## ✅ Requisitos
- Java 11 o superior en la linea de comandos.

## ⚙️ Compilacion
Desde la raiz:
```
$files = Get-ChildItem -Recurse -Filter *.java -Path src | ForEach-Object { $_.FullName }
javac -d out $files
```
En bash:
```
javac -d out $(find src -name "*.java")
```

## ▶️ Ejecucion
```
java -cp out com.himfg.hospitalinfantil.aplicacion.AplicacionConsola
```
La auditoria se escribe en `auditoria-himfg.log`. La aplicacion precarga dos pacientes demo para agilizar las pruebas.

## 📋 Flujo principal
- Menu para registrar, listar, buscar y desactivar pacientes.
- Catalogo de encuestas para Cuidados Paliativos Pediatricos.
- Aplicacion de encuestas con preguntas de sintomas (escala 1-5) y bienestar psicosocial/espiritual.
- Al finalizar una encuesta, el sistema permite exportar a CSV por paciente.

## 🧭 Opciones del menu
1. Registrar paciente  
2. Listar pacientes activos  
3. Buscar paciente por id  
4. Desactivar paciente  
5. Ver encuestas de cuidados paliativos  
6. Aplicar encuesta de cuidados paliativos  
0. Salir  

## 📝 Ejemplo rapido
```
=== Sistema de Encuestas HIMFG (POO) ===

Seleccione una opcion:
1. Registrar paciente
...
6. Aplicar encuesta de cuidados paliativos
1
Nombre completo del paciente: Juan Perez
Fecha de nacimiento (AAAA-MM-DD): 2010-05-01
Sexo (FEMENINO/MASCULINO/INTERSEXUAL/NO_ESPECIFICADO): masculino
Numero de expediente: EXP-00123
Paciente registrado con id: 1

Seleccione una opcion:
6
Encuestas de Cuidados Paliativos y Calidad de Vida:
1. Control de sintomas pediatrico (Paliativos)
   Descripcion: Monitorea la intensidad de los sintomas fisicos prioritarios
2. Bienestar psicosocial y espiritual
   Descripcion: Explora necesidades emocionales, sociales y espirituales del paciente y la familia
Elija el numero de encuesta a aplicar: 1
Ingrese el id del paciente: 1

Aplicando: Control de sintomas pediatrico (Paliativos)
Dolor en las ultimas 24 horas (1=nada, 5=severo)
Respuesta: 3
Dificultad para respirar (1=nada, 5=muy intensa)
Respuesta: 2
...
Encuesta aplicada y almacenada en memoria.
Desea exportar las encuestas del paciente a CSV? (s/n): s
Archivo exportado: encuestas-paliativos-1-2024-01-01T12-00-00.csv
```

### Ejemplo 2: listar y exportar sin preguntas adicionales
```
Seleccione una opcion:
2
Pacientes activos en el HIMFG:
 - Paciente{id=1, nombre='Paciente Demo A', edad=14}
 - Paciente{id=2, nombre='Paciente Demo B', edad=12}

Seleccione una opcion:
6
Encuestas de Cuidados Paliativos y Calidad de Vida:
1. Control de sintomas pediatrico (Paliativos)
   Descripcion: Monitorea la intensidad de los sintomas fisicos prioritarios
2. Bienestar psicosocial y espiritual
   Descripcion: Explora necesidades emocionales, sociales y espirituales del paciente y la familia
Elija el numero de encuesta a aplicar: 2
Ingrese el id del paciente: 2

Aplicando: Bienestar psicosocial y espiritual
Que le preocupa mas en este momento?
Respuesta: Dolor nocturno
Que le ayuda a sentirse tranquilo o tranquila?
Respuesta: Lectura y musica
Hay creencias o practicas espirituales que desea que el equipo conozca?
Respuesta: Oracion en familia
Como se siente la familia con el plan actual de cuidado?
Respuesta: Acompañados y escuchados
Encuesta aplicada y almacenada en memoria con id interno: 1
Desea exportar las encuestas del paciente a CSV? (s/n): s
Archivo exportado: encuestas-paliativos-2-2024-01-02T09-30-00.csv
```

### Ejemplo 3: manejo de validaciones
```
Seleccione una opcion:
1
Nombre completo del paciente: A
Fecha de nacimiento (AAAA-MM-DD): 2030-01-01
Sexo (FEMENINO/MASCULINO/INTERSEXUAL/NO_ESPECIFICADO): masculino
Numero de expediente: EXP-1
Error de validacion: El nombre completo debe tener al menos 3 caracteres.
```

### Ejemplo 4: exportar cuando no hay encuestas
```
Seleccione una opcion:
6
Encuestas de Cuidados Paliativos y Calidad de Vida:
1. Control de sintomas pediatrico (Paliativos)
2. Bienestar psicosocial y espiritual
Elija el numero de encuesta a aplicar: 1
Ingrese el id del paciente: 99
Error: No se encontro el paciente con id 99
```

## 🗂️ Estructura del proyecto
- `src/com/himfg/hospitalinfantil/aplicacion`: punto de entrada y consola.
- `src/com/himfg/hospitalinfantil/modelo`: entidades de dominio (personas, encuestas, citas).
- `src/com/himfg/hospitalinfantil/repositorio`: repositorios en memoria.
- `src/com/himfg/hospitalinfantil/servicio`: servicios de dominio.
- `src/com/himfg/hospitalinfantil/archivos`: utilidades de exportacion y auditoria.
- `archivos/`: utilidades duplicadas para referencia (exportador CSV).

## 🏛️ Arquitectura y carpetas
- `aplicacion`: orquestacion de consola y flujo principal (UI de texto).
- `servicio`: reglas de negocio (pacientes, medicos, citas, encuestas).
- `repositorio`: persistencia en memoria con mapas concurrentes e IDs atomicos.
- `modelo`: objetos de dominio con validaciones (entidades base, personas, encuestas, citas).
- `archivos`: IO de auditoria y exportacion CSV (NIO.2).
- `excepciones`: errores de dominio (`ValidacionDominioException`, `RecursoNoEncontradoException`).
- `out`: binarios compilados (ignorado por git).

Estructura abreviada:
```
src/com/himfg/hospitalinfantil/
  aplicacion/AplicacionConsola.java
  archivos/{RegistroAuditoriaArchivo.java, ExportadorEncuestasPaciente.java}
  excepciones/{ValidacionDominioException.java, RecursoNoEncontradoException.java}
  modelo/
    comun/{EntidadBase.java, Persona.java}
    personas/{Paciente.java, Medico.java, Sexo.java, EspecialidadMedica.java}
    encuestas/{CatalogoEncuestasCuidadosPaliativos.java, EncuestaSalud.java, EncuestaAplicada.java,
               Pregunta.java, PreguntaEscala.java, PreguntaTexto.java, Respuesta.java, TipoPregunta.java}
    citas/{CitaMedica.java, EstadoCita.java}
  repositorio/{PacienteRepositorioEnMemoria.java, MedicoRepositorioEnMemoria.java,
               CitaMedicaRepositorioEnMemoria.java, EncuestaAplicadaRepositorioEnMemoria.java}
  servicio/{PacienteServicio.java, MedicoServicio.java, CitaMedicaServicio.java, EncuestaServicio.java}
```

## 🔐 Guia de uso (credenciales)
Esta aplicacion no implementa autenticacion ni usuarios por defecto. La ejecucion es directa desde consola. Si en el futuro se requiere un esquema de roles (por ejemplo, Super Administrador), habria que agregar un modelo de usuarios, almacenamiento seguro de contrasenas y flujo de login; hoy no existe esa funcionalidad.

## 📤 Exportar encuestas
`archivos/ExportadorEncuestasPaciente.java` exporta las encuestas aplicadas de un paciente a CSV en UTF-8. La consola ya genera el archivo cuando se confirma la exportacion tras aplicar una encuesta.
