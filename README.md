## Creando archivos Excel en formato .xlsx desde Java

Desde Microsoft Office 2007 el formato para almacenar los archivos a recibido una actualización mas que significativa pasando del viejo formato binario a un formato XML mejor detallado y estandarizado el Office Open XML y desde Office 2010 este es el formato en el que se guardan los archivos por defecto por lo que es muy posible que su organización ya lo este usando, por lo que es util poder generar este tipo de archivos.

Como ya se menciono en una entrada anterior las librerias Apache POI nos proporcionan las clases y metodos necesarios para crear archivos de Office asi que no es de sorprender que se puedan crear archivos en el formato mas nuevo.

# XSSF
 Si hace algo de memoria o revisa la entrada antes mencionada recordara que las clases para manejar los archivos Excel .xls, en especifico el libro de Excel y la hoja de trabajo se encontraban en el paquete org.apache.poi.hssf.usermodel, para manejar los archivos .xlsx las clases necesarias se localizan en org.apache.poi.xssf.usermodel y la clase HSSFWorkbook es remplazada por XSSFWorkbook, todo esto quedara mas claro en el ejemplo.

# Ejemplo
```java
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package mx.com.hash.newexcel;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.logging.Level;
import java.util.logging.Logger;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.CellStyle;
import org.apache.poi.ss.usermodel.FillPatternType;
import org.apache.poi.ss.usermodel.IndexedColors;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

/**
 *
 * @author david
 */
public class ExcelOOXML {

    private static final Logger LOGGER = Logger.getLogger("mx.com.hash.newexcel.ExcelOOXML");

    public static void main(String[] args) {
        // Creamos el archivo donde almacenaremos la hoja
        // de calculo, recuerde usar la extension correcta,
        // en este caso .xlsx
        File archivo = new File("reporte.xlsx");

        // Creamos el libro de trabajo de Excel formato OOXML
        Workbook workbook = new XSSFWorkbook();

        // La hoja donde pondremos los datos
        Sheet pagina = workbook.createSheet("Reporte de productos");

        // Creamos el estilo paga las celdas del encabezado
        CellStyle style = workbook.createCellStyle();
        // Indicamos que tendra un fondo azul aqua
        // con patron solido del color indicado
        style.setFillForegroundColor(IndexedColors.AQUA.getIndex());
        style.setFillPattern(FillPatternType.SOLID_FOREGROUND);

        String[] titulos = {"Identificador", "Consumos",
            "Precio Venta", "Precio Compra"};
        Double[] datos = {1.0, 10.0, 45.5, 25.50};

        // Creamos una fila en la hoja en la posicion 0
        Row fila = pagina.createRow(0);

        // Creamos el encabezado
        for (int i = 0; i < titulos.length; i++) {
            // Creamos una celda en esa fila, en la posicion 
            // indicada por el contador del ciclo
            Cell celda = fila.createCell(i);

            // Indicamos el estilo que deseamos 
            // usar en la celda, en este caso el unico 
            // que hemos creado
            celda.setCellStyle(style);
            celda.setCellValue(titulos[i]);
        }

        // Ahora creamos una fila en la posicion 1
        fila = pagina.createRow(1);

        // Y colocamos los datos en esa fila
        for (int i = 0; i < datos.length; i++) {
            // Creamos una celda en esa fila, en la
            // posicion indicada por el contador del ciclo
            Cell celda = fila.createCell(i);

            celda.setCellValue(datos[i]);
        }

        // Ahora guardaremos el archivo
        try {
            // Creamos el flujo de salida de datos,
            // apuntando al archivo donde queremos 
            // almacenar el libro de Excel
            FileOutputStream salida = new FileOutputStream(archivo);

            // Almacenamos el libro de 
            // Excel via ese 
            // flujo de datos
            workbook.write(salida);

            // Cerramos el libro para concluir operaciones
            workbook.close();

            LOGGER.log(Level.INFO, "Archivo creado existosamente en {0}", archivo.getAbsolutePath());

        } catch (FileNotFoundException ex) {
            LOGGER.log(Level.SEVERE, "Archivo no localizable en sistema de archivos");
        } catch (IOException ex) {
            LOGGER.log(Level.SEVERE, "Error de entrada/salida");
        }
    }

}

```

