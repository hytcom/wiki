# nogal
*the most simple PHP Framework* by hytcom.net
___
  

# pdf
## nglGraftPDF *extends* nglStd [instanciable] [20150930]
Implementa la clase 'html2pdf'.
Constructeur
@param	string		\$sens - landscape or portrait orientation
@param	string		\$format - format A4, A5, ...
@param	string		\$langue - language: fr, en, it ... 
@param	boolean		\$unicode - TRUE means clustering the input text IS unicode (default = true)
@param 	String		\$encoding - charset encoding; Default is UTF-8
@param	array		\$marges - margins by default, in order (left, top, right, bottom)
@return	null


ejemplo #1:
<page backtop='14mm' backbottom='14mm' backleft='10mm' backright='10mm' style='font-size: 10pt'>
<page_header> 
...              
</page_header> 
<page_footer> 
...
</page_footer> 
...
</page>


ejemplo #2:
<page backimg='http://abcontenidos.com/ab.jpg' backimgx='0' backimgy='0' backtop='140px' backbottom='40px' style='font-size: 10pt'>
<page_footer style='text-align:right;font-size:8pt'>página [[page_cu]] de [[page_nb]]</page_footer>

contenidos
</page>


The Tag Page Properties
pageset : old / new
orientation : p / P / portrait / l / L / landscape / paysage
format : A4 / A5 / LETTER / widthxheight (100×200, …) / ….
backimg : url of an image
backimgx : left / center / right / value (mm, px, pt, % )
backimgy : top / middle / bottom / value (mm, px, pt, % )
backimgw : value(mm, px, pt, % )
backtop : value(mm, px, pt, % )
backbottom : value(mm, px, pt, % )
backleft : value(mm, px, pt, % )
backright : value (mm, px, pt, % )
backcolor : value of color
footer : values among (page, date, time, form), separated by;


TTF FRONTS CONVERTER
http://www.xml-convert.com/en/convert-tff-font-to-afm-pfa-fpdf-tcpdf
  
## Argumentos
|Argumento|Tipo|Default|Descripción|
|---|---|---|---|
|**after**|mixed|null|Determina una posible acción luego de ejecutar el método write<ul><li>**null** solo escribe</li><li>**view** muestra el documento en el navegador</li><li>**download** fuerza la descarga del documento</li></ul>|
|**content**|string|test1234|Contanido del PDF|
|**filename**|string|document.pdf|Nombre de archivo de salida|
|**sense**|string|P|Sentido de la hoja, P (vertical) ó L (horizontal)|
|**page**|string|A4|Tamaño de la página<ul><li>A4</li><li>A5</li><li>LETTER</li><li>widthxheight (100x200)</li></ul>|
|**language**|string|es|Lenguaje|
|**unicode**|boolean|true|Determina el uso de Unicode|
|**encoding**|string|UTF-8|Juego de caracteres predeterminado|
|**margins**|array|[5,5,5,8]|Array o JSON con los margenes de la página (top,right,bottom,left)|
