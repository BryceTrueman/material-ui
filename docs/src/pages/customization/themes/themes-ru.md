# Themes

<p class="description">Customize Material-UI with your theme. You can change the colors, the typography and much more.</p>

The theme specifies the color of the components, darkness of the surfaces, level of shadow, appropriate opacity of ink elements, etc.

Themes let you apply a consistent tone to your app. It allows you to **customize all design aspects** of your project in order to meet the specific needs of your business or brand.

To promote greater consistency between apps, light and dark theme types are available to choose from. By default, components use the light theme type.

## Theme provider

If you wish to customize the theme, you need to use the `MuiThemeProvider` component in order to inject a theme into your application. However, this is optional; Material-UI components come with a default theme.

`MuiThemeProvider` relies on the context feature of React to pass the theme down to the components, so you need to make sure that `MuiThemeProvider` is a parent of the components you are trying to customize. You can learn more about this in [the API section](#muithemeprovider).

## Theme configuration variables

Changing the theme configuration variables is the most effective way to match Material-UI to your needs. The following sections cover the most important theme variables:

- [Палитра](#palette)
- [Type (light / dark theme)](#type-light-dark-theme)
- [Typography](#typography)
- [Интервал](#spacing)
- [Другие переменные](#other-variables)
- [Пользовательские переменные](#custom-variables)

### Палитра

#### Intentions

A color intention is a mapping of a palette to a given intention within your application.

The theme exposes the following color intentions:

- primary - used to represent primary interface elements for a user.
- secondary - used to represent secondary interface elements for a user.
- error - used to represent interface elements that the user should be made aware of.

The default palette uses the shades prefixed with `A` (`A200`, etc.) for the secondary intention, and the un-prefixed shades for the other intentions.

If you want to learn more about color, you can check out [the color section](/style/color/).

#### Настраиваемая палитра

You may override the default palette values by including a `palette` object as part of your theme.

If any of the [`palette.primary`](/customization/default-theme/?expend-path=$.palette.primary), [`palette.secondary`](/customization/default-theme/?expend-path=$.palette.secondary) or [`palette.error`](/customization/default-theme/?expend-path=$.palette.error) 'intention' objects are provided, they will replace the defaults.

The intention value can either be a [color](/style/color/) object, or an object with one or more of the following keys:

```js
interface PaletteIntention {
  light?: string;
  main: string;
  dark?: string;
  contrastText?: string;
};
```

**Using a color object**

The simplest way to customize an intention is to import one or more of the provided colors and apply them to a palette intention:

```js
import { createMuiTheme } from '@material-ui/core/styles';
import blue from '@material-ui/core/colors/blue';

const theme = createMuiTheme({
  palette: {
    primary: blue,
  },
});
```

If the intention key receives a color object as in the example above, the following mapping is used to populate the required keys:

```js
palette: {
  primary: {
    light: palette.primary[300],
    main: palette.primary[500],
    dark: palette.primary[700],
    contrastText: getContrastText(palette.primary[500]),
  },
  secondary: {
    light: palette.secondary.A200,
    main: palette.secondary.A400,
    dark: palette.secondary.A700,
    contrastText: getContrastText(palette.secondary.A400),
  },
  error: {
    light: palette.error[300],
    main: palette.error[500],
    dark: palette.error[700],
    contrastText: getContrastText(palette.error[500]),
  },
},
```

This example illustrates how you could recreate the default palette values:

```js
import { createMuiTheme } from '@material-ui/core/styles';
import indigo from '@material-ui/core/colors/indigo';
import pink from '@material-ui/core/colors/pink';
import red from '@material-ui/core/colors/red';

// All the following keys are optional.
// We try our best to provide a great default value.
const theme = createMuiTheme({
  palette: {
    primary: indigo,
    secondary: pink,
    error: red,
    // Used by `getContrastText()` to maximize the contrast between the background and
    // the text.
    contrastThreshold: 3,
    // Used to shift a color's luminance by approximately
    // two indexes within its tonal palette.
    // E.g., shift from Red 500 to Red 300 or Red 700.
    tonalOffset: 0.2,
  },
});
```

**Providing the colors directly**

If you wish to provide more customized colors, you can either create your own color object, or directly supply colors to some or all of the intention's keys:

```js
import { createMuiTheme } from '@material-ui/core/styles';

const theme = createMuiTheme({
  palette: {
    primary: {
      // light: will be calculated from palette.primary.main,
      main: '#ff4400',
      // dark: will be calculated from palette.primary.main,
      // contrastText: will be calculated to contrast with palette.primary.main
    },
    secondary: {
      light: '#0066ff',
      main: '#0044ff',
      // dark: will be calculated from palette.secondary.main,
      contrastText: '#ffcc00',
    },
    // error: will use the default color
  },
});
```

As in the example above, if the intention object contains custom colors using any of the `main`, `light`, `dark` or `contrastText` keys, these map as follows:

- If the `dark` and / or `light` keys are omitted, their value(s) will be calculated from `main`, according to the `tonalOffset` value.

- If `contrastText` is omitted, its value will be calculated to contrast with `main`, according to the`contrastThreshold` value.

Both the `tonalOffset` and `contrastThreshold` values may be customized as needed. A higher value for `tonalOffset` will make calculated values for `light` lighter, and `dark` darker. A higher value for `contrastThreshold` increases the point at which a background color is considered light, and given a dark `contrastText`.

Note that `contrastThreshold` follows a non-linear curve.

#### Example

{{"demo": "pages/customization/themes/Palette.js"}}

#### Color tool

Need inspiration? The Material Design team has built an awesome [palette configuration tool](/style/color/#color-tool) to help you.

### Type (light /dark theme)

You can make the theme dark by setting `type` to `dark`. While it's only a single property value change, internally it modifies the value of the following keys:

- `palette.text`
- `palette.divider`
- `palette.background`
- `palette.action`

```js
const theme = createMuiTheme({
  palette: {
    type: 'dark',
  },
});
```

{{"demo": "pages/customization/themes/DarkTheme.js", "hideEditButton": true}}

### Typography

Too many type sizes and styles at once can spoil any layout. The theme provides a **limited set of type sizes** that work well together along with the layout grid. These sizes are used across the components.

Have a look at the following example regarding changing the default values, such as the font family. If you want to learn more about typography, you can check out [the typography section](/style/typography/).

{{"demo": "pages/customization/themes/TypographyTheme.js"}}

#### Typography - Font family

```js
const theme = createMuiTheme({
  typography: {
    // Use the system font instead of the default Roboto font.
    fontFamily: [
      '-apple-system',
      'BlinkMacSystemFont',
      '"Segoe UI"',
      'Roboto',
      '"Helvetica Neue"',
      'Arial',
      'sans-serif',
      '"Apple Color Emoji"',
      '"Segoe UI Emoji"',
      '"Segoe UI Symbol"',
    ].join(','),
  },
});
```

#### Typography - Font size

Material-UI uses `rem` units for the font size. The browser `<html>` element default font size is `16px`, but browsers have an option to change this value, so `rem` units allow us to accommodate the user's settings, resulting in a much better user experience. Users change font size settings for all kinds of reasons, from poor eyesight to choosing optimum settings for devices that can be vastly different in size and viewing distance.

To change the font-size of Material-UI you can provide a `fontSize` property. The default value is `14px`.

```js
const theme = createMuiTheme({
  typography: {
    // In Japanese the characters are usually larger.
    fontSize: 12,
  },
});
```

The computed font size by the browser follows this mathematical equation:

![font-size](/static/images/font-size.gif) <!-- https://latex.codecogs.com/gif.latex?computed&space;=&space;specification&space;\frac{typography.fontSize}{14}&space;\frac{html&space;font&space;size}{typography.htmlFontSize} -->

#### Typography - HTML font size

You might want to change the `<html>` element default font size. For instance, when using the [10px simplification](https://www.sitepoint.com/understanding-and-using-rem-units-in-css/). We provide a `htmlFontSize` theme property for this use case. It's telling Material-UI what's the font-size on the `<html>` element is. It's used to adjust the `rem` value so the calculated font-size always match the specification.

```js
const theme = createMuiTheme({
  typography: {
    // Tell Material-UI what's the font-size on the html element is.
    htmlFontSize: 10,
  },
});
```

```css
html {
  font-size: 62.5%; /* 62.5% of 16px = 10px */
}
```

*You need to apply the above CSS on the html element of this page to see the below demo rendered correctly*

{{"demo": "pages/customization/themes/FontSizeTheme.js"}}

### Интервал

We encourage you to use the `theme.spacing()` helper to create consistent spacing between the elements of your UI. Material-UI uses [a recommended 8px scaling factor by default](https://material.io/design/layout/understanding-layout.html).

```js
const styles = theme => ({
  root: {
    // JSS использует пиксели в качестве дефолтных единиц измерения для этого CSS свойства.
    padding: theme.spacing (2), // Выведет 8 * 2
  },
});
```

Вы можете изменить преобразование расстояния, передав:

- число

```js
const theme = createMuiTheme ({
  spacing: 4,
});

theme.spacing (2) // = 4 * 2
```

- или функция

```js
const theme = createMuiTheme ({
  spacing: factor => `$ {0.25 * factor} rem`, // (стратегия Bootstrap)
});

theme.spacing (2) // = 0.5rem = 8px
```

#### Multiple arity

Вспомогательная функция ` theme.spacing () ` принимает до 4 аргументов. Вы можете использовать аргументы, чтобы уменьшить шаблон:

```diff
<br />- padding: `$ {theme.spacing (1)} px $ {theme.spacing (2)} px`,
+ padding: theme.spacing (1, 2), // '8px 16px'
```

### Другие переменные

В дополнение к палитре, темным и светлым типам и типографии, тема нормализует реализацию, предоставляя множество значений по умолчанию, таких как точки останова, тени, переходы и т. д. You can check out the [default theme section](/customization/default-theme/) to view the default theme in full.

### Пользовательские переменные

When using Material-UI's [styling solution](/customization/css-in-js/) with your own components, you can also take advantage of the theme. It can be convenient to add additional variables to the theme so you can use them everywhere. For instance:

{{"demo": "pages/customization/themes/CustomStyles.js"}}

## Customizing all instances of a component type

### CSS

When the configuration variables aren't powerful enough, you can take advantage of the `overrides` key of the `theme` to potentially change every single **style** injected by Material-UI into the DOM. Это действительно мощная штука.

```js
const theme = createMuiTheme({
  overrides: {
    MuiButton: { // Name of the component ⚛️ / style sheet
      text: { // Name of the rule
        color: 'white', // Some CSS
      },
    },
  },
});
```

{{"demo": "pages/customization/themes/OverridesCss.js"}}

The list of these customization points for each component is documented under the **Component API** section. For instance, you can have a look at the [Button](/api/button/#css). Alternatively, you can always have a look at the [implementation](https://github.com/mui-org/material-ui/blob/next/packages/material-ui/src/Button/Button.js).

### Свойства

Вы также можете применять свойства ко всем экземплярам типа компонента. We expose a `props` key in the `theme` for this use case.

```js
const theme = createMuiTheme({
  props: {
    // Name of the component ⚛️
    MuiButtonBase: {
      // The properties to apply
      disableRipple: true, // No more ripple, on the whole application 
    },
  },
});
```

{{"demo": "pages/customization/themes/OverridesProperties.js"}}

## Accessing the theme in a component

You might need to access the theme variables inside your React components. Допустим, вы хотите отобразить значение основного цвета. В этом может помочь компонент высшего порядка `withTheme `. Here is an example:

{{"demo": "pages/customization/themes/WithTheme.js"}}

## Nesting the theme

The theming solution is very flexible, as [you can nest](/css-in-js/advanced/#theme-nesting) multiple theme providers. This can be really useful when dealing with different area of your application that have distinct appearance from each other.

{{"demo": "pages/customization/themes/ThemeNesting.js"}}

The inner theme will **override** the outer theme. You can extend the outer theme by providing a function:

{{"demo": "pages/customization/themes/ThemeNestingExtend.js"}}

#### A note on performance

The performance implications of nesting the `MuiThemeProvider` component are linked to JSS's work behind the scenes. The main point to understand is that we cache the injected CSS with the following tuple `(styles, theme)`.

- `theme`: If you provide a new theme at each render, a new CSS object will be computed and injected. Both for UI consistency and performance, it's better to render a limited number of theme objects.
- `styles`: The larger the styles object is, the more work is needed.

## API

### `MuiThemeProvider`

This component takes a `theme` property, and makes the `theme` available down the React tree thanks to React context. It should preferably be used at **the root of your component tree**.

You can see the full properties API in [this dedicated page](/api/mui-theme-provider/).

#### Примеры

```jsx
import React from 'react';
import { render } from 'react-dom';
import { MuiThemeProvider, createMuiTheme } from '@material-ui/core/styles';
import Root from './Root';

const theme = createMuiTheme();

function App() {
  return (
    <MuiThemeProvider theme={theme}>
      <Root />
    </MuiThemeProvider>
  );
}

render(<App />, document.querySelector('#app'));
```

### `createMuiTheme(options) => theme`

Generate a theme base on the options received.

#### Аргументы

1. `options` (*Object*): Takes an incomplete theme object and adds the missing parts.

#### Возвращает

`theme` (*Object*): A complete, ready to use theme object.

#### Примеры

```js
import { createMuiTheme } from '@material-ui/core/styles';
import purple from '@material-ui/core/colors/purple';
import green from '@material-ui/core/colors/green';

const theme = createMuiTheme({
  palette: {
    primary: purple,
    secondary: green,
  },
  status: {
    danger: 'orange',
  },
});
```

### `withTheme()(Component) => Component`

Provide the `theme` object as a property of the input component so it can be used in the render method.

#### Аргументы

1. `Component`: The component that will be wrapped.

#### Возвращает

`Component`: The new component created.

#### Примеры

```js
import { withTheme } from '@material-ui/core/styles';

function MyComponent(props) {
  return <div>{props.theme.direction}</div>;
}

export default withTheme(MyComponent);
```