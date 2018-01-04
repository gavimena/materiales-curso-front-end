# Componentes en React

[codepen-default-values]: https://codepen.io/adalab/pen/JMrJdK?editors=0010
[codepen-component-children]: https://codepen.io/adalab/pen/WdZEQv?editors=0010

## Contenidos
- [Introducción](#introducción)
- [¿Para qué sirve lo que vamos a ver en esta sesión?](#¿para-qué-sirve-lo-que-vamos-a-ver-en-esta-sesión)
- [Componentes padre e hijo (madre e hija)](#componentes-padre-e-hijo-madre-e-hija)
- [Ejemplos de app con varios componentes y cómo se pasan datos con las `props`](#ejemplos-de-app-con-varios-componentes-y-cómo-se-pasan-datos-con-las-props)
- [`propTypes`](#proptypes)
- [Valores por defecto de las `props`](#valores-por-defecto-de-las-props)
- [Uso de `children` para acceder a los componentes hijo cuando no los conoces](#uso-de-children-para-acceder-a-los-componentes-hijo-cuando-no-los-conoces)


## Introducción

Hasta ahora hemos visto cómo crear componentes sencillos y decirle a React que se encargue de pintarlos.

En esta sesión veremos cómo definir componentes más completos y robustos, y exploraremos las diferentes maneras de relacionar nuestros componentes entre sí.


## ¿Para qué sirve lo que vamos a ver en esta sesión?

Según vayamos creando aplicaciones web más grandes con React, necesitaremos definir mayor número de componentes que se relacionarán entre sí. Veremos cómo se relacionan los componentes padre/madre con los componentes hijo/hija, y un tipo especial de componente que tendrá más componentes arbitrarios en su interior.

También necesitaremos crear componentes con mayores garantías de funcionar. Para eso veremos `propTypes`, para obligar a las `props` a ser de un tipo de dato concreto, y cómo asignarles valores por defecto para hacer `props` opcionales.  


## Componentes padre e hijo (madre e hija)

Vimos en la sesión anterior cómo usar un componente dentro de otro: el componente `CatList` renderizaba tres componentes `RandomCat`. Para referirnos a componentes que renderizan otros usaremos el término **componente padre o madre**, y para referirnos a componentes que son renderizados por otros, **componente hijo o hija**.

> No debemos confundir esta terminología con la terminología de la herencia de clases. Aunque las dos tengan jerarquía, esta terminología representa relaciones de **composición**, no de herencia. En React solo existe herencia en el hecho de que todos los componentes son subclases de `React.Component`.

Los componentes padre/madre pueden tener múltiples componentes hijo/hija, pero los componentes hijo/hija solo tienen un componente padre/madre. Cabe destacar que un componente puede ser hijo/hija de un componente padre/madre y a la vez ser padre/madre de otros componentes hijo/hija.

```
                  ┌───────────┐
                  │  CatList  │
                  └─┬───┬───┬─┘
                    ┊   ┊   ┊
       ┌╌╌╌╌╌╌╌╌╌╌╌╌┘   ┊   └╌╌╌╌╌╌╌╌╌╌╌╌┐
       ┊                ┊                ┊
┌──────┴──────┐  ┌──────┴──────┐  ┌──────┴──────┐
│  RandomCat  │  │  RandomCat  │  │  RandomCat  │
└─────────────┘  └─────────────┘  └─────────────┘
```

Estas relaciones forman una jerarquía importante para entender React. Desde los componentes padre/madre podremos pasar datos _hacia abajo_ a los componentes hijo/hija, mediante las `props`, pero **no al revés**. Un hijo/a no podrá pasar datos _hacia arriba_ libremente. Veremos en una sesión posterior cómo "solucionar" los problemas que _a priori_ parece generar este **flujo unidireccional**.


## Ejemplos de app con varios componentes y cómo se pasan datos con las `props`

[STUB]


## `propTypes`

[STUB]


## Valores por defecto de las `props`

En ocasiones querremos definir que algunas `props` no sean obligatorias, y cuando no se pasen querremos usar un valor por defecto. Esto se puede conseguir en React con `defaultProps`. Será un objeto con el nombre de las `props` que queremos que tengan valor por defecto y su correspondiente valor, y cuando se instancie el componente, se cogerán las `props` que falten de ese objeto. Lo definimos como una propiedad del componente, `NombreDelComponente.defaultProps = {}`, después de declarar la clase:


```js
import React from 'react';

class Button extends React.Component {
  render() {
    return (
      <button className={ `btn btn-${this.props.styling}` } type="button" name="button">
        { this.props.label }
      </button>
    )
  }
}

// Así definimos las defaultProps
Button.defaultProps = {
  styling: 'primary', // from Bootstrap classes: primary, secondary, success, info, warning, danger, link
  label: 'Aceptar'
};
```

[Valores por defecto en Codepen][codepen-default-values]

> No hace falta importar el paquete `prop-types` para usar valores por defecto


## Uso de `children` para acceder a los componentes hijo cuando no los conoces

Algunas veces, al declarar un componente no sabremos qué otros componentes podrá contener dentro. Por ejemplo, un componente `Popup` o un componente genérico `Header`. En esos casos podremos usar una `prop` especial, `children`, para pasar directamente elementos:

```js
import React from 'react';

class Popup extends React.Component {
  render() {
    return (
      <div className={ `alert alert-${this.props.styling}` } role="alert">
        { this.props.children }
      </div>
    );
  }
}

ReactDOM.render(
  <Popup styling="info">
    <h1 className="horizontal-center">Welcome</h1>
    <p>
      Thank you for visiting our webpage!
    </p>
    <p>
      We hope you enjoy our new shiny site!
    </p>
  </Popup>,
  document.getElementById('react-root')
);
```

[Componentes y `children` en Codepen][codepen-component-children]

Como se puede observar en el ejemplo, inyectaremos `props.children` en el JSX del componente genérico como una variable cualquiera. Cuando usemos el componente, escribiremos el contenido en JSX dentro de sus etiquetas de apertura (`<Popup>`) y de cierre (`</Popup>`).


## Recursos externos

### React Docs

Documentación oficial de React (en inglés).

- [Validar propiedades con `propTypes`) y propiedades por defecto](https://reactjs.org/docs/typechecking-with-proptypes.html)
- [Composición (`children`)](https://reactjs.org/docs/composition-vs-inheritance.html#containment)

### Egghead

Serie de clases en vídeo que introduce y explora los fundamentos básicos de React.

- [Cómo usar componentes como `children` de otros componentes en React](https://reactjs.org/docs/typechecking-with-proptypes.html#default-prop-values)
