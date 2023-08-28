## Dinocode

   > <span style="color: #15FD0F; font-weight: bold;">DinoCode</span> is a fourth generation pseudo-language based on <span style="color: #AEB7AE; font-weight: bold;">AutoHotKey v1.1.37.01</span>, focused on processing additional information outside of compile time, with extended support for native AHK functions and additional developer-defined functions, driving its own simple, flexible, controlled, multilanguage programming logic ( `Español\Inglés`).

   ### History
   > It was developed by BlassGO (Ismael Q.) to allow full control of the AutoIMG tool for Windows, being able to control through a SCRIPT important aspects of Android software installation, such as formatting/updating partitions, executing shell commands, flashing custom modules and further. All with the same predefined functions in the tool and with the ability to create new functions.
   >
   > At the moment DinoCode only exists in this tool.

   The work of these people helped to create it:
   * Jonathan Bennett and Chris Mallett (The reason AHK exists)
   * Uberi and Pullover (For their expression evaluator for AHK)
   * Lexikos (Amazing contributions for AHK, like ActiveScript)
   * AHK Community (Powered some additional features)

   ### Example: English
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
   ### Example: Spanish
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

## Limitations
   > BASIC/C++ ---> AHK ---> DinoCode

   Dinocode is not optimized for great tasks, mainly because of its mode of interpretation and overload of having double interpreter.

## Implementation
   > To add DinoCode to your `AutoHotKey` (v1.1.37.01) based program, three libraries must be included:
   > 
   > **->**  [Download libraries](https://github.com/BlassGO/DinoCode/raw/main/releases/DinoCode_last_release.zip ':ignore')
   > 
   > ```javascript
   > #include DinoCode.ahk
   > #include eval.ahk
   > #include active_script.ahk
   > ```
   
   
   * **General use**
      
      **->** Below is a short usage guide, for more details or implementation of your own functions, check out the `DinoCode.ahk` header.

   > The code to be executed must be a `STRING` (Text), and it is interpreted using the `load_config` function, as follows:
   > ```javascript
   > load_config(code)
   > ```
   >
   > Many code segments can be loaded without losing continuity:
   > ```javascript
   > load_config(code_1)
   > load_config(code_2)
   > load_config(code_3)
   > ```
   >
   > To end the Script, that is, to restart the work environment for a new sequence of code, `true` must be specified in the sixth parameter of the function.
   > ```javascript
   > load_config(,,,,,true)
   > ```


## License
> DinoCode is distributed under the GNU General Public License version 2.0 (GPLv2). This means that anyone can use, modify and distribute the software as long as they comply with the terms of the license.

  **->** See the [LICENSE](https://raw.githubusercontent.com/BlassGO/DinoCode/main/LICENSE ':ignore') file for more information on the license terms and conditions.
