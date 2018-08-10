# Bithive Elementos
## Indice
### Elementos
- [attacher](#attacher)
- [autocomplete](#autocomplete)


## Botones de Acción
**Selectores**
- .btn-insert 
- .btn-update 
- .btn-delete
&nbsp;

## Tabs
Genera un grupo de solapas

**Selector** "ul.nav-tabs"

**Estructura**
``` html
<ul class="nav nav-tabs" role="tablist">
        <li role="presentation" class="active"><a href="#tab-legal" aria-controls="tab-legal" role="tab" data-toggle="tab">Datos Laborales</a></li>
        <li role="presentation"><a href="#tab-personal" aria-controls="tab-personal" role="tab" data-toggle="tab">Datos Personales</a></li>
        <li role="presentation"><a href="#tab-bank" aria-controls="tab-bank" role="tab" data-toggle="tab">Banco</a></li>
        <li role="presentation"><a href="#tab-info" aria-controls="tab-info" role="tab" data-toggle="tab">Referencias</a></li>
        <li role="presentation"><a href="#tab-admin" aria-controls="tab-admin" role="tab" data-toggle="tab">Administración</a></li>
    </ul>
    <div class="tab-content">
        <div role="tabpanel" class="tab-pane active" id="tab-legal">
            contenido tab legal
        </div>
        <div role="tabpanel" class="tab-pane active" id="tab-personal">
            contenido tab personal
        </div>
        <div role="tabpanel" class="tab-pane active" id="tab-bank">
            contenido tab bank
        </div>
        <div role="tabpanel" class="tab-pane active" id="tab-info">
            contenido tab info
        </div>
        <div role="tabpanel" class="tab-pane active" id="tab-admin">
            contenido tab admin
        </div>
    </div>
```
&nbsp;

## Clipboard