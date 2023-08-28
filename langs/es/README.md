
## DinoCode

   > <span style="color: #15FD0F; font-weight: bold;">DinoCode</span> es un pseudo-lenguaje de cuarta generación basado en <span style="color: #AEB7AE; font-weight: bold;">AutoHotKey v1.1.37.01</span>, enfocado en el procesamiento de información adicional fuera del tiempo de compilación, con soporte extendido a funciones nativas de AHK y funciones adicionales definidas por el desarrollador, manejando su propia lógica de programación simple, flexible, controlada y multilenguaje (`Español\Inglés`).

   ### Historia
   > Fue desarrollado por BlassGO (Ismael Q.) para permitir el control total de la herramienta AutoIMG para Windows, pudiendo controlar mediante un SCRIPT aspectos importantes en la instalación de software en Android, como formatear/actualizar particiones, ejecutar comandos Shell, flashear módulos personalizados y más. Todo con las mismas funciones predefinidas en la herramienta y con la capacidad de crear nuevas funciones.
   >
   > De momento DinoCode solo existe en esta herramienta.

   El trabajo de estas personas ayudo a su creación:
   * Jonathan Bennett y Chris Mallett (La razón de que AHK exista)
   * Uberi y Pulover (Por su evaluador de expresiones para AHK)
   * Lexikos (Increíbles contribuciones para AHK, como ActiveScript)
   * Comunidad AHK (Potencio algunas características adicionales)

   ### Ejemplo: Inglés
   ```javascript
   :Show_Result
        Message "Result" With OPTION

   :main
        Global OPTION
        If $(Question With "Do you want to continue?")
            Message With "Welcome!"
            Until (OPTION>0)
                Set OPTION With $(Option "Title" "Message" With "First" "Second" "Third")
            show_result
        Else
            Abort
        Exit
   ```
   ### Ejemplo: Español
    ```javascript
    :Mostrar_Resultado
        Mensaje "Resultado" con OPCION

    :main
        Global OPCION
        Si $(Pregunta Con "Desea continuar?")
            Mensaje Con "Bienvenido!"
            Hasta (OPCION>0)
                Definir OPCION Con $(Opción "Titulo" "Mensaje" Con "Primero" "Segundo" "Tercero")
            mostrar_resultado
        SiNo
            Abortar
        Salir
    ```

## Limitaciones
   > BASIC/C++ ---> AHK ---> DinoCode

   DinoCode no está optimizado para grandes tareas, principalmente por su modo de interpretación y la sobrecarga de tener doble intérprete. 

## Implementación
   > Para añadir DinoCode a su programa basado en `AutoHotKey` (v1.1.37.01), se deben incluir tres librerías:
   > 
   > **->** [Descargar librerías](https://github.com/BlassGO/DinoCode/raw/main/releases/DinoCode_last_release.zip ':ignore')
   > 
   > ```javascript
   > #include DinoCode.ahk
   > #include eval.ahk
   > #include active_script.ahk
   > ```
   
   
   * **Uso general**
      
      **->** A continuación se muestra una pequeña guía de uso, para mayores detalles o implementación de sus propias funciones, revise la cabecera de `DinoCode.ahk`.

   > El código a ejecutar debe ser un `STRING` (Texto), y se interpreta usando la función `load_config`, de la siguiente manera:
   > ```javascript
   > load_config(code)
   > ```
   >
   > Se pueden cargar muchos segmentos de código sin perder la continuidad:
   > ```javascript
   > load_config(code_1)
   > load_config(code_2)
   > load_config(code_3)
   > ```
   >
   > Para finalizar el Script, es decir, reiniciar el ambiente de trabajo para una nueva secuencia de código, se debe especificar `true` en el sexto parámetro de la función.
   > ```javascript
   > load_config(,,,,,true)
   > ```


## Licencia
> DinoCode se distribuye bajo la Licencia Pública General de GNU versión 2.0 (GPLv2). Esto significa que cualquier persona puede utilizar, modificar y distribuir el software siempre y cuando cumpla con los términos de la licencia.

  **->** Consulte el archivo [LICENSE](https://raw.githubusercontent.com/BlassGO/DinoCode/main/LICENSE ':ignore') para obtener más información sobre los términos y condiciones de la licencia.
