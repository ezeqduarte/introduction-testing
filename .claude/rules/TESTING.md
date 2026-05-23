---
description: Apply these rules whenever creating, modifying, or reviewing Cypress E2E or component tests. Do NOT apply to unit tests (Jest/Jasmine) or unrelated files.
---

# Cypress Testing Rules — Angular

## Selectors

- **Siempre** usar `data-cy` como atributo de selección. Nunca usar clases CSS, IDs, ni tags HTML.
- El atributo debe describir el rol semántico del elemento, no su apariencia.

```html
<!-- Correcto -->
<button data-cy="submit-reservation-btn">Reservar</button>

<!-- Incorrecto -->
<button class="btn-primary" id="btn1">Reservar</button>
```

- En el test: `cy.get('[data-cy="submit-reservation-btn"]')`
- Para listas: usar `data-cy="property-card"` y encadenar `.eq(0)` o `.first()`

---

## Estructura de carpetas

```
cypress/
  e2e/               # specs organizadas por feature
    search/
      search-filters.cy.ts
      search-results.cy.ts
    booking/
      reservation-flow.cy.ts
  support/
    commands.ts      # comandos custom reutilizables
    e2e.ts           # setup global
  fixtures/
    properties.json  # datos de prueba
    user.json
  component/         # tests de componente (Cypress Component Testing)
```

---

## Naming

- `describe`: nombre del feature o página → `describe('Search Filters', ...)`
- `it`: acción del usuario en lenguaje natural → `it('should filter properties by location', ...)`
- Nunca usar "test" o "check" en el `it`. Usar "should".

```ts
describe('Reservation Flow', () => {
  it('should display total price when dates are selected', () => { ... });
  it('should disable the reserve button when no dates are selected', () => { ... });
  it('should show an error when the reservation fails', () => { ... });
});
```

---

## Sin esperas hardcodeadas

- **Prohibido**: `cy.wait(2000)`
- Usar `cy.intercept()` para stubear y esperar peticiones de red.

```ts
cy.intercept('GET', '/api/properties*', { fixture: 'properties.json' }).as('getProperties');
cy.visit('/search');
cy.wait('@getProperties');
```

- Para Angular: si el componente usa `ChangeDetectionStrategy.OnPush`, ejecutar acciones que desencadenen detección antes de hacer assertions.

---

## Arrange - Act - Assert

Cada test debe seguir este orden sin mezclar fases:

```ts
it('should show reservation summary after booking', () => {
  // Arrange
  cy.intercept('POST', '/api/reservations', { fixture: 'reservation.json' }).as('createReservation');
  cy.visit('/property/123');

  // Act
  cy.get('[data-cy="check-in-date"]').click();
  cy.get('[data-cy="calendar-day-15"]').click();
  cy.get('[data-cy="submit-reservation-btn"]').click();
  cy.wait('@createReservation');

  // Assert
  cy.get('[data-cy="reservation-summary"]').should('be.visible');
  cy.get('[data-cy="confirmation-number"]').should('not.be.empty');
});
```

---

## Custom Commands

Toda interacción repetida entre tests debe extraerse como comando custom en `cypress/support/commands.ts`.

```ts
// commands.ts
Cypress.Commands.add('selectDates', (checkIn: string, checkOut: string) => {
  cy.get('[data-cy="check-in-input"]').click();
  cy.get(`[data-cy="calendar-day-${checkIn}"]`).click();
  cy.get(`[data-cy="calendar-day-${checkOut}"]`).click();
});

// uso en el spec
cy.selectDates('15', '20');
```

---

## Fixtures

- Todo dato de prueba va en `cypress/fixtures/`. Nunca hardcodear objetos en el spec.
- Nombrar fixtures por el recurso que representan: `property.json`, `user-logged-in.json`.

```ts
cy.intercept('GET', '/api/user/profile', { fixture: 'user-logged-in.json' });
```

---

## Cobertura de estados

Cada feature debe tener tests para todos sus estados relevantes:

| Estado | Obligatorio |
|---|---|
| Happy path (flujo principal) | Sí |
| Estado de error (API falla) | Sí |
| Estado vacío (sin resultados) | Sí |
| Estado de carga (loading) | Si aplica |
| Estado deshabilitado | Si aplica |

---

## Aislamiento de tests

- Cada `it` debe poder ejecutarse de forma independiente.
- Usar `beforeEach` para setup común (visitar la página, stubear rutas base).
- Nunca depender del estado dejado por un test anterior.

```ts
beforeEach(() => {
  cy.intercept('GET', '/api/properties*', { fixture: 'properties.json' });
  cy.visit('/search');
});
```

---

## Angular + Cypress Component Testing

Cuando se testa un componente de Angular de forma aislada (no E2E):

```ts
import { MountConfig } from 'cypress/angular';
import { PropertyCardComponent } from './property-card.component';

it('should display property title', () => {
  cy.mount(PropertyCardComponent, {
    componentProperties: {
      title: 'Casa en Palermo',
      price: 120,
    },
  } as MountConfig<PropertyCardComponent>);

  cy.get('[data-cy="property-title"]').should('contain', 'Casa en Palermo');
});
```

- Usar `cy.mount()` para componentes aislados — no navegar con `cy.visit()`.
- Proveer solo los `@Input()` necesarios para el caso de prueba.
