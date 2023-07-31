# hotels-reservation

Este es un SPA para encontrar la reservacion de hotel más economica.

# Parámetros de entrada
- Como parametros de entrada se solicita llenar las fechas en el calendario y marcar si es un Reward Customer.
- Un Json con los datos de los hoteles presentados en el problema.

# Proceso
1. Al dar clic en el botón se realiza una validación de que el campo fecha no se encuentre vacío.
2. Se realiza un recorrido por cada hotel y las fechas seleccionadas validando si es un weekday o weekend, y se hace una suma de los precios dependiendo de si es un Reward Customer o no guardando estos valores en un arreglo.
3. Del arreglo obtenido se busca el valor menor y se valida que este valor no este duplicado o exista un empate.
4. En caso de existir un empate se busca el hotel con mayor rating.
5. Se muestra un mensaje con el nombre del hotel más económico y se pinta en el grid el mismo.

# Fuentes externas

- Para el manejo del calendario se esta usando VueDatePicker de esta fuente https://vue3datepicker.com/installation/
- Para mostrar las estrellas del rating se usa código de w3schools

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

# Autor
JESSICA ARCINIEGA