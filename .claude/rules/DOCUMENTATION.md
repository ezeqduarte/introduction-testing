---
description: Apply these rules whenever creating, modifying, or reviewing Storybook stories for Angular components. Do NOT apply to application logic, tests, or unrelated files.
---

# Storybook Documentation Rules — Angular

## Cuándo documentar con Storybook

Documentar en Storybook **todo componente** que sea:
- Reutilizable (usado en más de un lugar)
- Un elemento visual con variantes (botón, card, badge, input)
- Un componente con estados visuales propios (loading, error, empty, success)

No documentar en Storybook: páginas completas con lógica de negocio compleja ni servicios.

---

## Estructura de carpetas

```
src/
  app/
    shared/
      components/
        button/
          button.component.ts
          button.component.html
          button.component.scss
          button.stories.ts       <- story junto al componente
        property-card/
          property-card.component.ts
          property-card.stories.ts
```

Las stories viven **al lado del componente**, no en una carpeta separada.

---

## Story mínima obligatoria

Todo componente debe tener al menos una story `Default` que represente el estado más común de uso.

```ts
import type { Meta, StoryObj } from '@storybook/angular';
import { ButtonComponent } from './button.component';

const meta: Meta<ButtonComponent> = {
  title: 'Shared/Button',
  component: ButtonComponent,
  tags: ['autodocs'],
};

export default meta;
type Story = StoryObj<ButtonComponent>;

export const Default: Story = {
  args: {
    label: 'Reservar',
    variant: 'primary',
    disabled: false,
  },
};
```

---

## Documentar todos los `@Input()`

Usar `argTypes` para describir cada propiedad: tipo, descripción, y opciones si aplica.

```ts
const meta: Meta<PropertyCardComponent> = {
  title: 'Shared/PropertyCard',
  component: PropertyCardComponent,
  tags: ['autodocs'],
  argTypes: {
    title: {
      description: 'Título de la propiedad',
      control: 'text',
    },
    price: {
      description: 'Precio por noche en USD',
      control: { type: 'number', min: 0 },
    },
    rating: {
      description: 'Calificación de 0 a 5',
      control: { type: 'range', min: 0, max: 5, step: 0.1 },
    },
    isGuestFavorite: {
      description: 'Muestra el badge "Guest Favorite"',
      control: 'boolean',
    },
  },
};
```

---

## Documentar `@Output()` como actions

```ts
import { action } from '@storybook/addon-actions';

export const Default: Story = {
  args: {
    label: 'Reservar',
  },
  argTypes: {
    clicked: { action: 'clicked' },
  },
};
```

---

## Cubrir todos los estados visuales

Cada componente debe tener stories para sus estados relevantes:

```ts
export const Default: Story = { args: { label: 'Reservar', variant: 'primary' } };

export const Secondary: Story = { args: { label: 'Guardar', variant: 'secondary' } };

export const Disabled: Story = { args: { label: 'Reservar', disabled: true } };

export const Loading: Story = { args: { label: 'Cargando...', isLoading: true } };
```

Regla: si el componente tiene una prop `isLoading`, `hasError`, o `isEmpty`, cada una necesita su propia story.

---

## Setup de módulo Angular

Usar `moduleMetadata` cuando el componente necesita providers, pipes, o dependencias.

```ts
import { moduleMetadata } from '@storybook/angular';
import { CurrencyPipe } from '@angular/common';

const meta: Meta<PropertyCardComponent> = {
  title: 'Shared/PropertyCard',
  component: PropertyCardComponent,
  decorators: [
    moduleMetadata({
      imports: [CurrencyPipe],
      providers: [
        { provide: BookingService, useClass: MockBookingService },
      ],
    }),
  ],
};
```

Nunca importar el `AppModule` completo en una story — solo los módulos/providers necesarios.

---

## Naming y organización

### Título de la story

Usar jerarquía de diseño atómico:

```
Atoms/Button
Atoms/Badge
Atoms/Input
Molecules/SearchBar
Molecules/PropertyCard
Organisms/ReservationPanel
Organisms/TopNav
```

### Nombre de las stories

Usar PascalCase. El nombre debe describir el estado o variante, no el componente.

```ts
export const Primary: Story = { ... };
export const Disabled: Story = { ... };
export const WithRating: Story = { ... };
export const GuestFavorite: Story = { ... };
```

---

## Tokens del sistema de diseño

Al definir los `args` de una story, usar los tokens de `DESIGN.md` como referencia de valores válidos:

- Colores: `#ff385c` (Rausch), `#222222` (Ink), `#ffffff` (Canvas)
- Border radius: 4px, 8px, 14px, 20px, 32px, 9999px
- Tipografía: tamaños 11px, 12px, 13px, 14px, 16px, 20px, 21px, 22px, 28px

No inventar valores fuera del sistema de diseño.

---

## Accesibilidad

Toda story debe poder ejecutarse sin errores de accesibilidad básicos:
- Imágenes con `alt`
- Botones con texto visible o `aria-label`
- Contraste mínimo entre texto y fondo (WCAG AA)

Si Storybook tiene el addon `@storybook/addon-a11y`, verificar que no hay violaciones en el panel "Accessibility" antes de considerar la story como completa.

---

## Autodocs

Siempre incluir `tags: ['autodocs']` en el `meta` para generar la página de documentación automática. Esta página mostrará la tabla de props y todas las stories renderizadas.

```ts
const meta: Meta<ButtonComponent> = {
  title: 'Atoms/Button',
  component: ButtonComponent,
  tags: ['autodocs'], // obligatorio
};
```
