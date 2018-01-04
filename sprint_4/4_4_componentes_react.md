# Componentes en React

## Contenidos
- [Introducción](#introducción)
- [¿Para qué sirve lo que vamos a ver en esta sesión?](#¿para-qué-sirve-lo-que-vamos-a-ver-en-esta-sesión)
- Componentes padre e hijo (madre e hija)
- Ejemplos de app con varios componentes y cómo se pasan datos con las `props`
- `propTypes`
- Valores por defecto de las `props`
- Uso de `children` para acceder a los componentes hijo cuando no los conoces

## Introducción

Hasta ahora hemos visto cómo crear componentes sencillos y decirle a React que se encargue de pintarlos.

En esta sesión veremos cómo definir componentes más completos y robustos, y exploraremos las diferentes maneras de relacionar nuestros componentes entre sí.


## ¿Para qué sirve lo que vamos a ver en esta sesión?

Según vayamos creando aplicaciones web más grandes con React, necesitaremos definir mayor número de componentes que se relacionarán entre sí. Veremos cómo se relacionan los componentes padre/madre con los componentes hijo/hija, y un tipo especial de componente que tendrá más componentes arbitrarios en su interior.

También necesitaremos crear componentes con mayores garantías de funcionar. Para eso veremos `propTypes`, para obligar a las `props` a ser de un tipo de dato concreto, y cómo asignar valores por defecto a las `props`.  


## Componentes padre e hijo (madre e hija)

[STUB]


## Ejemplos de app con varios componentes y cómo se pasan datos con las `props`

[STUB]


## `propTypes`

[STUB]


## Valores por defecto de las `props`

[STUB]

```js
import React from 'react';

class Button extends React.Component {
  render() {
    return (
      <button className="btn" type="button" name="button">
        {this.props.label}
      </button>
    )
  }
}

// Así definimos las defaultProps
Button.defaultProps = {
  label: 'Aceptar'
};
```


## Uso de `children` para acceder a los componentes hijo cuando no los conoces

[STUB]


## Recursos externos

### {{resource.name}}

{{resource.description}}

- [{{resource.link_name}}]({{resource.url}})
