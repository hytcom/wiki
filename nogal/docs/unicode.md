# Nogal v1.0
*the most simple PHP Framework* by hytcom.net
___
  

# unicode
## nglUnicode *extends* nglTrunk [main] [20140316]
Este objeto sustituye algunos de los metodos nativos de PHP vinculados a las operaciones con cadenas de caracteres multibytes

nglUnicode construye el objeto \$unicode dentro del framework, el cual es accedido a trav茅s de: **\$ngl("unicode")->NOMBRE_DE_METODO(...)**
  
## Variables
`private` $bDisable = 
			Cuando el valor es true, los metodos retornan los valores de los metodos nativos de PHP.
			Esto 煤ltimo ocurre cuando la constante NGL_CHARSET es diferente a UTF-8
		  

  
&nbsp;

# M茅todos
|M茅todo|Descripci贸n|
|---|---|
|[chr](#chr)|Devuelve una cadena de un caracter que contiene el car醕ter especificado por $nCode|
|[escape](#escape)|Escapa una cadena en formato UNICODE.Donde los caracteres que no sean UTF-8 ser醤 reemplazados por su ORD en formato hexadecimal precedidos de una \u|
|[explode](#explode)|Divide una cadena en varias|
|[groups](#groups)|Retorna la informaci髇 de los grupos de caracteres UTF-8TiposABC: alfabetoABU: abugidaNUM: n鷐erosSYL: silabarioSYM: s韒bolosGruposSYM - CONTROL: C...|
|[info](#info)|Devuelve informaci髇 de un caracter dadochar: caractertype: tipo de caractergroup: grupo UTF-8 al que pertenecebytes: bytes que ocupadecimal: valor d...|
|[is](#is)|Retorna el tipo, grupo y valor decimal de un caracter dado, o false en caso de error|
|[ord](#ord)|Devuelve el valor UNICODE del caracter $sChar|
|[split](#split)|Convierte $mSource en un array de caracteres UTF-8. Si $mSource es un array split retornara $mSource|
|[strlen](#strlen)|Obtiene la longitud de una cadena|
|[substr](#substr)|Devuelve la subcadena de $mSource comenzando en $nStart y por un largo de $nLength|
|[unescape](#unescape)|Desescapa una cadena UNICODE|
|[unescapeChar](#unescapeChar)|Auxiliar del m閠odo unescape|

  
&nbsp;


## chr
Devuelve una cadena de un caracter que contiene el car谩cter especificado por \$nCode  

**[string]** =  *public* function ( *int* \$nCode );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$nCode**|int||C贸digo unicode del caracter buscado|

&nbsp;
___
&nbsp;

## escape
Escapa una cadena en formato UNICODE.
Donde los caracteres que no sean UTF-8 ser谩n reemplazados por su ORD en formato hexadecimal precedidos de una **\u**  

**[string]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sString**|string||Cadena a codificar|

&nbsp;
___
&nbsp;

## explode
Divide una cadena en varias  

**[string]** =  *public* function ( *string* \$sSplitter, *string* \$sString, *int* \$nLimit );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sSplitter**|string||Cadena delimitadora de 1 caracter unicode de largo|
|**\$sString**|string||Origen de datos|
|**\$nLimit**|int|null|N煤mero m谩ximo de subcadenas. \$nLimit actua igual que en PHP, es decir 
Si es positivo, el array devuelto contendr谩 el m谩ximo de elementos en el limit y el 煤ltimo elemento contendr谩 el resto del string.
Si es negativo, se devolver谩n todos los componentes a excepci贸n de los 煤ltimos -limit.
Si es cero, actuar谩 como si su valor fuera 1.|

&nbsp;
___
&nbsp;

## groups
Retorna la informaci贸n de los grupos de caracteres UTF-8

**Tipos**<ul><li>**ABC** =  alfabeto</li><li>**ABU** =  abugida</li><li>**NUM** =  n煤meros</li><li>**SYL** =  silabario</li><li>**SYM** =  s铆mbolos</li></ul>**Grupos**<ul><li>**SYM - CONTROL** =  Control character (0-31)</li><li>**SYM - LATIN_BASIC_SYMBOLS** =  Basic Latin (32-47)</li><li>**NUM - LATIN_BASIC_NUMBERS** =  Basic Latin - Numbers (48-57)</li><li>**SYM - LATIN_BASIC_SYMBOLS** =  Basic Latin (58-64)</li><li>**ABC - LATIN_BASIC_UPPERCASE** =  Basic Latin - uppercase (65-90)</li><li>**SYM - LATIN_BASIC_SYMBOLS** =  Basic Latin (91-96)</li><li>**ABC - LATIN_BASIC_LOWERCASE** =  Basic Latin - lowercase (97-122)</li><li>**SYM - LATIN_BASIC_SYMBOLS** =  Basic Latin (123-127)</li><li>**SYM - LATIN1_CONTROL** =  Control C1 (128-159)</li><li>**SYM - LATIN1_SYMBOLS** =  Special symbols (160-191)</li><li>**ABC - LATIN1_SUPP** =  Latin-1 Supplement (192-255)</li><li>**ABC - LATIN_EXT_A** =  Latin Extended-A (256-383)</li><li>**ABC - LATIN_EXT_B** =  Latin Extended-B (384-591)</li><li>**ABC - IPA** =  IPA Extensions (592-687)</li><li>**SYM - SPACING** =  Spacing Modifier Letters (688-767)</li><li>**SYM - DIACRITICAL_MARKS** =  Combining Diacritical Marks (768-879)</li><li>**ABC - GREEK** =  Greek and Coptic (880-1023)</li><li>**ABC - CYRILLIC** =  Cyrillic (1024-1279)</li><li>**ABC - CYRILLIC** =  Cyrillic Supplement (1280-1327)</li><li>**ABC - ARMENIAN** =  Armenian (1328-1423)</li><li>**ABC - HEBREW** =  Hebrew (1424-1535)</li><li>**ABC - ARABIC** =  Arabic (1536-1791)</li><li>**ABC - SYRIAC** =  Syriac (1792-1871)</li><li>**ABC - ARABIC** =  Arabic Supplement (1872-1919)</li><li>**ABC - THAANA** =  Thaana (1920-1983)</li><li>**ABC - NKO** =  NKo (1984-2047)</li><li>**ABC - SAMARITAN** =  Samaritan (2048-2111)</li><li>**ABC - MANDAIC** =  Mandaic (2112-2143)</li><li>**ABC - ARABIC** =  Arabic Extended-A (2208-2303)</li><li>**ABU - DEVANAGARI** =  Devanagari (2304-2431)</li><li>**ABU - BENGALI** =  Bengali (2432-2559)</li><li>**ABU - GURMUKHI** =  Gurmukhi (2560-2687)</li><li>**ABU - GUJARATI** =  Gujarati (2688-2815)</li><li>**ABU - ORIYA** =  Oriya (2816-2943)</li><li>**ABU - TAMIL** =  Tamil (2944-3071)</li><li>**ABU - TELUGU** =  Telugu (3072-3199)</li><li>**ABU - KANNADA** =  Kannada (3200-3327)</li><li>**ABU - MALAYALAM** =  Malayalam (3328-3455)</li><li>**ABU - SINHALA** =  Sinhala (3456-3583)</li><li>**ABU - THAI** =  Thai (3584-3711)</li><li>**ABU - LAO** =  Lao (3712-3839)</li><li>**ABU - TIBETAN** =  Tibetan (3840-4095)</li><li>**ABU - MYANMAR** =  Myanmar (4096-4255)</li><li>**ABU - GEORGIAN** =  Georgian (4256-4351)</li><li>**ABU - HANGUL_JAMO** =  Hangul Jamo (4352-4607)</li><li>**ABU - ETHIOPIC** =  Ethiopic (4608-4991)</li><li>**ABU - ETHIOPIC** =  Ethiopic Supplement (4992-5023)</li><li>**SYL - CHEROKEE** =  Cherokee (5024-5119)</li><li>**ABU - ABORIGINAL** =  Unified Canadian Aboriginal Syllabics (5120-5759)</li><li>**ABC - OGHAM** =  Ogham (5760-5791)</li><li>**ABC - RUNIC** =  Runic (5792-5887)</li><li>**ABU - TAGALOG** =  Tagalog (5888-5919)</li><li>**ABU - HANUNOO** =  Hanunoo (5920-5951)</li><li>**ABU - BUHID** =  Buhid (5952-5983)</li><li>**ABU - TAGBANWA** =  Tagbanwa (5984-6015)</li><li>**ABU - KHMER** =  Khmer (6016-6143)</li><li>**ABU - MONGOLIAN** =  Mongolian (6144-6319)</li><li>**ABU - ABORIGINAL** =  Unified Canadian Aboriginal Syllabics Extended (6320-6399)</li><li>**ABU - LIMBU** =  Limbu (6400-6479)</li><li>**ABU - TAI** =  Tai Le (6480-6527)</li><li>**ABC - TAI** =  New Tai Lue (6528-6623)</li><li>**SYM - KHMER** =  Khmer Symbols (6624-6655)</li><li>**ABU - BUGINESE** =  Buginese (6656-6687)</li><li>**ABU - TAI** =  Tai Tham (6688-6831)</li><li>**SYM - DIACRITICAL_MARKS** =  Combining Diacritical Marks Extended (6832-6911)</li><li>**ABU - BALINESE** =  Balinese (6912-7039)</li><li>**ABU - SUNDANESE** =  Sundanese (7040-7103)</li><li>**ABU - BATAK** =  Batak (7104-7167)</li><li>**ABU - LEPCHA** =  Lepcha (7168-7247)</li><li>**ABC - OL_CHIKI** =  Ol Chiki (7248-7295)</li><li>**ABU - SUNDANESE** =  Sundanese Supplement (7360-7375)</li><li>**SYM - VEDIC** =  Vedic Extensions (7376-7423)</li><li>**SYM - PHONETIC** =  Phonetic Extensions (7424-7551)</li><li>**SYM - PHONETIC** =  Phonetic Extensions Supplement (7552-7615)</li><li>**SYM - DIACRITICAL_MARKS** =  Combining Diacritical Marks Supplement (7616-7679)</li><li>**SYM - LATIN_EXT** =  Latin Extended Additional (7680-7935)</li><li>**SYM - GREEK** =  Greek Extended (7936-8191)</li><li>**SYM - PUNCTUATION** =  General Punctuation (8192-8303)</li><li>**SYM - SUP_SUB_SCRIPTS** =  Superscripts and Subscripts (8304-8351)</li><li>**SYM - CURRENCY** =  Currency Symbols (8352-8399)</li><li>**SYM - DIACRITICAL_MARKS** =  Combining Diacritical Marks for Symbols (8400-8447)</li><li>**SYM - LETTERLIKE** =  Letterlike Symbols (8448-8527)</li><li>**SYM - NUMBER** =  Number Forms (8528-8591)</li><li>**SYM - ARROWS** =  Arrows (8592-8703)</li><li>**SYM - MATHEMATICAL** =  Mathematical Operators (8704-8959)</li><li>**SYM - TECHNICAL** =  Miscellaneous Technical (8960-9215)</li><li>**SYM - CONTROL_PICTURES** =  Control Pictures (9216-9279)</li><li>**SYM - OPTICAL** =  Optical Character Recognition (9280-9311)</li><li>**SYM - ALPHANUMERICS** =  Enclosed Alphanumerics (9312-9471)</li><li>**SYM - BOX_DRAWINGS** =  Box Drawing (9472-9599)</li><li>**SYM - BLOCK_ELEMENTS** =  Block Elements (9600-9631)</li><li>**SYM - GEOMETRIC_SHAPES** =  Geometric Shapes (9632-9727)</li><li>**SYM - MISCELLANEOUS** =  Miscellaneous Symbols (9728-9983)</li><li>**SYM - DINGBATS** =  Dingbats (9984-10175)</li><li>**SYM - MATHEMATICAL** =  Miscellaneous Mathematical Symbols-A (10176-10223)</li><li>**SYM - ARROWS** =  Supplemental Arrows-A (10224-10239)</li><li>**SYM - BRAILLE** =  Braille Patterns (10240-10495)</li><li>**SYM - ARROWS** =  Supplemental Arrows-B (10496-10623)</li><li>**SYM - MATHEMATICAL** =  Miscellaneous Mathematical Symbols-B (10624-10751)</li><li>**SYM - MATHEMATICAL** =  Supplemental Mathematical Operators (10752-11007)</li><li>**SYM - MISCELLANEOUS** =  Miscellaneous Symbols and Arrows (11008-11263)</li><li>**ABC - GLAGOLITIC** =  Glagolitic (11264-11359)</li><li>**ABC - LATIN_EXT_C** =  Latin Extended-C (11360-11391)</li><li>**ABC - COPTIC** =  Coptic (11392-11519)</li><li>**ABC - GEORGIAN** =  Georgian Supplement (11520-11567)</li><li>**ABY - TIFINAGH** =  Tifinagh (11568-11647)</li><li>**ABU - ETHIOPIC** =  Ethiopic Extended (11648-11743)</li><li>**ABC - CYRILLIC** =  Cyrillic Extended-A (11744-11775)</li><li>**SYM - PUNCTUATION** =  Supplemental Punctuation (11776-11903)</li><li>**SYM - CJK** =  CJK Radicals Supplement (11904-12031)</li><li>**SYM - KANGXI** =  Kangxi Radicals (12032-12255)</li><li>**SYM - IDEOGRAPHIC** =  Ideographic Description Characters (12272-12287)</li><li>**SYM - CJK** =  CJK Symbols and Punctuation (12288-12351)</li><li>**SYL - HIRAGANA** =  Hiragana (12352-12447)</li><li>**SYL - KATAKANA** =  Katakana (12448-12543)</li><li>**SYL - BOPOMOFO** =  Bopomofo (12544-12591)</li><li>**SYL - HANGUL** =  Hangul Compatibility Jamo (12592-12687)</li><li>**SYL - KANBUN** =  Kanbun (12688-12703)</li><li>**SYL - BOPOMOFO** =  Bopomofo Extended (12704-12735)</li><li>**SYL - CJK** =  CJK Strokes (12736-12783)</li><li>**SYL - KATAKANA** =  Katakana Phonetic Extensions (12784-12799)</li><li>**SYL - CJK** =  Enclosed CJK Letters and Months (12800-13055)</li><li>**SYL - CJK** =  CJK Compatibility (13056-13311)</li><li>**SYL - IDEOGRAPHIC** =  CJK Unified Ideographs Extension A (13312-19893)</li><li>**SYL - YIJING** =  Yijing Hexagram Symbols (19904-19967)</li><li>**SYL - IDEOGRAPHIC** =  CJK Unified Ideographs (19968-40908)</li><li>**SYL - YI** =  Yi Syllables (40960-42127)</li><li>**SYL - YI** =  Yi Radicals (42128-42191)</li><li>**SYM - LISU** =  Lisu (42192-42239)</li><li>**SYL - VAI** =  Vai (42240-42559)</li><li>**ABC - CYRILLIC** =  Cyrillic Extended-B (42560-42655)</li><li>**SYL - BAMUM** =  Bamum (42656-42751)</li><li>**SYM - TONE_LETTERS** =  Modifier Tone Letters (42752-42783)</li><li>**ABC - LATIN_EXT_D** =  Latin Extended-D (42784-43007)</li><li>**ABU - SYLOTI** =  Syloti Nagri (43008-43055)</li><li>**NUM - INDIC_NUMBER** =  Common Indic Number Forms (43056-43071)</li><li>**ABU - PHAGS_PA** =  Phags-pa (43072-43135)</li><li>**ABC - SAURASHTRA** =  Saurashtra (43136-43231)</li><li>**ABU - DEVANAGARI** =  Devanagari Extended (43232-43263)</li><li>**ABU - KAYAH** =  Kayah Li (43264-43311)</li><li>**ABU - REJANG** =  Rejang (43312-43359)</li><li>**ABC - HANGUL** =  Hangul Jamo Extended-A (43360-43391)</li><li>**ABU - JAVANESE** =  Javanese (43392-43487)</li><li>**SYM - MYANMAR** =  Myanmar Extended-B (43488-43519)</li><li>**ABU - CHAM** =  Cham (43520-43615)</li><li>**ABU - MYANMAR** =  Myanmar Extended-A (43616-43647)</li><li>**ABU - TAI** =  Tai Viet (43648-43743)</li><li>**ABU - MEETEI** =  Meetei Mayek Extensions (43744-43775)</li><li>**ABU - ETHIOPIC** =  Ethiopic Extended-A (43776-43823)</li><li>**SYM - LATIN_EXT** =  Latin Extended-E (43824-43887)</li><li>**SYM - MEETEI** =  Meetei Mayek (43968-44031)</li><li>**ABC - HANGUL** =  Hangul Syllables (44032-55203)</li><li>**SYM - HANGUL** =  Hangul Jamo Extended-B (55216-55295)</li><li>**SYM - SURROGATES** =  High Surrogates (55296-56191)</li><li>**SYM - SURROGATES** =  High Private Use Surrogates (56192-56319)</li><li>**SYM - SURROGATES** =  Low Surrogates (56320-57343)</li><li>**SYM - USE_AREA** =  Private Use Area (57344-63743)</li><li>**SYM - IDEOGRAPHIC** =  CJK Compatibility Ideographs (63744-64255)</li><li>**SYM - ALPHABETIC_FORMS** =  Alphabetic Presentation Forms (64256-64335)</li><li>**SYM - ARABIC** =  Arabic Presentation Forms-A (64336-65023)</li><li>**SYM - SELECTORS** =  Variation Selectors (65024-65039)</li><li>**SYM - VERTICAL_FORMS** =  Vertical Forms (65040-65055)</li><li>**SYM - HALF_MARKS** =  Combining Half Marks (65056-65071)</li><li>**SYM - CJK** =  CJK Compatibility Forms (65072-65103)</li><li>**SYM - VARIANTS** =  Small Form Variants (65104-65135)</li><li>**SYM - ARABIC** =  Arabic Presentation Forms-B (65136-65279)</li><li>**SYM - HALFWIDTH_FULLWIDTH** =  Halfwidth and Fullwidth Forms (65280-65519)</li><li>**SYM - SPECIALS** =  Specials (65520-65535)</li><li>**SYM - LINEAR_B** =  Linear B Syllabary (65536-65663)</li><li>**SYM - LINEAR_B** =  Linear B Ideograms (65664-65791)</li><li>**SYM - NUMBERS** =  Aegean Numbers (65792-65855)</li><li>**SYM - NUMBERS** =  Ancient Greek Numbers (65856-65935)</li><li>**SYM - ANCIENT** =  Ancient Symbols (65936-65999)</li><li>**SYM - PHAISTOS** =  Phaistos Disc (66000-66047)</li><li>**SYM - LYCIAN** =  Lycian (66176-66207)</li><li>**SYM - CARIAN** =  Carian (66208-66271)</li><li>**SYM - NUMBERS** =  Coptic Epact Numbers (66272-66303)</li><li>**SYM - ITALIC** =  Old Italic (66304-66351)</li><li>**SYM - GOTHIC** =  Gothic (66352-66383)</li><li>**SYM - PERMIC** =  Old Permic (66384-66431)</li><li>**SYM - UGARITIC** =  Ugaritic (66432-66463)</li><li>**SYM - PERSIAN** =  Old Persian (66464-66527)</li><li>**SYM - DESERET** =  Deseret (66560-66639)</li><li>**SYM - SHAVIAN** =  Shavian (66640-66687)</li><li>**SYM - OSMANYA** =  Osmanya (66688-66735)</li><li>**SYM - ELBASAN** =  Elbasan (66816-66863)</li><li>**SYM - ALBANIAN** =  Caucasian Albanian (66864-66927)</li><li>**SYM - LINEAR_A** =  Linear A (67072-67455)</li><li>**SYM - CYPRIOT** =  Cypriot Syllabary (67584-67647)</li><li>**SYM - ARAMAIC** =  Imperial Aramaic (67648-67679)</li><li>**SYM - PALMYRENE** =  Palmyrene (67680-67711)</li><li>**SYM - NABATAEAN** =  Nabataean (67712-67759)</li><li>**SYM - PHOENICIAN** =  Phoenician (67840-67871)</li><li>**SYM - LYDIAN** =  Lydian (67872-67903)</li><li>**SYM - MEROITIC** =  Meroitic Hieroglyphs (67968-67999)</li><li>**SYM - MEROITIC** =  Meroitic Cursive (68000-68095)</li><li>**SYM - KHAROSHTHI** =  Kharoshthi (68096-68191)</li><li>**SYM - ARABIAN** =  Old South Arabian (68192-68223)</li><li>**SYM - ARABIAN** =  Old North Arabian (68224-68255)</li><li>**SYM - MANICHAEAN** =  Manichaean (68288-68351)</li><li>**SYM - AVESTAN** =  Avestan (68352-68415)</li><li>**SYM - PARTHIAN** =  Inscriptional Parthian (68416-68447)</li><li>**SYM - PAHLAVI** =  Inscriptional Pahlavi (68448-68479)</li><li>**SYM - PAHLAVI** =  Psalter Pahlavi (68480-68527)</li><li>**SYM - TURKIC** =  Old Turkic (68608-68687)</li><li>**SYM - RUMI** =  Rumi Numeral Symbols (69216-69247)</li><li>**SYM - BRAHMI** =  Brahmi (69632-69759)</li><li>**SYM - KAITHI** =  Kaithi (69760-69839)</li><li>**SYM - SORA** =  Sora Sompeng (69840-69887)</li><li>**SYM - CHAKMA** =  Chakma (69888-69967)</li><li>**SYM - MAHAJANI** =  Mahajani (69968-70015)</li><li>**SYM - SHARADA** =  Sharada (70016-70111)</li><li>**SYM - NUMBERS** =  Sinhala Archaic Numbers (70112-70143)</li><li>**SYM - KHOJKI** =  Khojki (70144-70223)</li><li>**SYM - KHUDAWADI** =  Khudawadi (70320-70399)</li><li>**SYM - GRANTHA** =  Grantha (70400-70527)</li><li>**SYM - TIRHUTA** =  Tirhuta (70784-70879)</li><li>**SYM - SIDDHAM** =  Siddham (71040-71167)</li><li>**SYM - MODI** =  Modi (71168-71263)</li><li>**SYM - TAKRI** =  Takri (71296-71375)</li><li>**SYM - WARANG** =  Warang Citi (71840-71935)</li><li>**SYM - PAU_CIN_HAU** =  Pau Cin Hau (72384-72447)</li><li>**SYM - CUNEIFORM** =  Cuneiform (73728-74751)</li><li>**SYM - NUMBERS** =  Cuneiform Numbers and Punctuation (74752-74879)</li><li>**SYM - EGYPTIAN** =  Egyptian Hieroglyphs (77824-78895)</li><li>**SYM - BAMUM** =  Bamum Supplement (92160-92735)</li><li>**SYM - MRO** =  Mro (92736-92783)</li><li>**SYM - BASSA_VAH** =  Bassa Vah (92880-92927)</li><li>**SYM - PAHAWH_HMONG** =  Pahawh Hmong (92928-93071)</li><li>**SYM - MIAO** =  Miao (93952-94111)</li><li>**SYM - KANA** =  Kana Supplement (110592-110847)</li><li>**SYM - DUPLOYAN** =  Duployan (113664-113823)</li><li>**SYM - SHORTHAND** =  Shorthand Format Controls (113824-113839)</li><li>**SYM - MUSICAL** =  Byzantine Musical Symbols (118784-119039)</li><li>**SYM - MUSICAL** =  Musical Symbols (119040-119295)</li><li>**SYM - MUSICAL** =  Ancient Greek Musical Notation (119296-119375)</li><li>**SYM - TAI** =  Tai Xuan Jing Symbols (119552-119647)</li><li>**SYM - MATHEMATICAL** =  Counting Rod Numerals (119648-119679)</li><li>**SYM - MATHEMATICAL** =  Mathematical Alphanumeric Symbols (119808-120831)</li><li>**SYM - MENDE_KIKAKUI** =  Mende Kikakui (124928-125151)</li><li>**SYM - MATHEMATICAL** =  Arabic Mathematical Alphabetic Symbols (126464-126719)</li><li>**SYM - MAHJONG** =  Mahjong Tiles (126976-127023)</li><li>**SYM - DOMINO** =  Domino Tiles (127024-127135)</li><li>**SYM - CARDS** =  Playing Cards (127136-127231)</li><li>**SYM - ALPHANUMERIC** =  Enclosed Alphanumeric Supplement (127232-127487)</li><li>**SYM - IDEOGRAPHIC** =  Enclosed Ideographic Supplement (127488-127743)</li><li>**SYM - MISCELLANEOUS** =  Miscellaneous Symbols and Pictographs (127744-128511)</li><li>**SYM - EMOTICONS** =  Emoticons (Emoji) (128512-128591)</li><li>**SYM - DINGBATS** =  Ornamental Dingbats (128592-128639)</li><li>**SYM - TRANSPORT_MAP** =  Transport and Map Symbols (128640-128767)</li><li>**SYM - ALCHEMICAL** =  Alchemical Symbols (128768-128895)</li><li>**SYM - GEOMETRIC_SHAPES** =  Geometric Shapes Extended (128896-129023)</li><li>**SYM - ARROWS** =  Supplemental Arrows-C (129024-129279)</li></ul>  

**[array]** =  *public* function ( );
  

&nbsp;
___
&nbsp;

## info
Devuelve informaci贸n de un caracter dado<ul><li>**char** =  caracter</li><li>**type** =  tipo de caracter</li><li>**group** =  grupo UTF-8 al que pertenece</li><li>**bytes** =  bytes que ocupa</li><li>**decimal** =  valor decimal</li><li>**hexadecimal** =  valor hexadecimal</li><li>**html** =  c贸digo HTML</li><li>**escaped** =  valor UNICODE escapado</li></ul>  

**[string]** =  *public* function ( *string* \$sChar );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sChar**|string||Caracter unicode del que se desea conocer el tipo|

&nbsp;
___
&nbsp;

## is
Retorna el tipo, grupo y valor decimal de un caracter dado, o false en caso de error  

**[array]** =  *public* function ( *string* \$sChar );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sChar**|string||Caracter unicode del que se desea conocer el tipo|

&nbsp;
___
&nbsp;

## ord
Devuelve el valor UNICODE del caracter \$sChar  

**[integer]** =  *public* function ( *string* \$sChar );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sChar**|string||Caracter unicode del que se desea conocer el c贸digo|

&nbsp;
___
&nbsp;

## split
Convierte \$mSource en un array de caracteres UTF-8. Si \$mSource es un array split retornara \$mSource  

**[string]** =  *public* function ( *mixed* \$mSource );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$mSource**|mixed||Origen de datos, string o array|

&nbsp;
___
&nbsp;

## strlen
Obtiene la longitud de una cadena  

**[integer]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sString**|string||Origen de datos|

&nbsp;
___
&nbsp;

## substr
Devuelve la subcadena de \$mSource comenzando en \$nStart y por un largo de \$nLength  

**[string]** =  *public* function ( *mixed* \$mSource, *int* \$nStart, *int* \$nLength );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$mSource**|mixed||Origen de datos, string o array. Este m茅todo trabaja sobre un array, por lo que si \$mSource es 
del tipo string ser谩 convertido a un array por medio de split.|
|**\$nStart**|int|0|Inicio de la subcadena:
Si no es negativo, la cadena devuelta comenzar谩 a \$nStart caracteres de la posici贸n cero.
Si es negativo, la cadena devuelta empezar谩 a \$nStart caracteres contando desde el final de string.
Si la longitud del string es menor o igual a \$nStart, la funci贸n devolver谩 FALSE.|
|**\$nLength**|int|null|Si se especifica el \$nLength y es positivo, la cadena devuelta contendr谩 como m谩ximo \$nLength caracteres
Si se especifica \$nLength y es negativo, entonces ese n煤mero de caracteres se omiten al final de la cadena
Si se omite el \$nLength, la subcadena empezar谩 en \$nStart hasta el final de la cadena
Si se especifica \$nLength y es 0, FALSE o NULL se devolver谩 la subcadena comprendida entre \$nStart y el final de la cadena|

&nbsp;
___
&nbsp;

## unescape
Desescapa una cadena UNICODE  

**[string]** =  *public* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sString**|string||Cadena UNICODE a decodificar|

&nbsp;
___
&nbsp;

## unescapeChar
Auxiliar del m茅todo unescape  

**[string]** =  *private* function ( *string* \$sString );  

|Argumento|Tipo|Default|Descripci贸n|
|---|---|---|---|
|**\$sString**|string||Cadena UNICODE a decodificar|

&nbsp;
___
&nbsp;
