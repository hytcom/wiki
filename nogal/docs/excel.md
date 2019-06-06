# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# excel
## nglGraftExcel *extends* nglStd [instanciable] [20150930]
Implementa la clase "phpExcel".

\$excel = \$ngl("excel")->load("names.xlsx");
print_r(\$excel->getall());exit();

\$excel = \$ngl("excel")->load("tabla.html");
print_r(\$excel->getall());exit();

\$excel = \$ngl("excel")->load("test.xls");
\$excel->set("A1", array(
array("NOMBRE", "APELLIDO", "EDAD", "MESES"),
array("ariel", "bottero", "40"),
array("homero", "simpson", "35")
));
\$excel->download("test.xls");
exit();

\$excel = \$ngl("excel")->load("names.xlsx");
\$excel->set("A2", "GROSO");
\$excel->download("names.html");
exit();
  
## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**content**|string|test1234|Contanido del PDF|
|**filename**|string|document.pdf|Nombre de archivo de salida|
