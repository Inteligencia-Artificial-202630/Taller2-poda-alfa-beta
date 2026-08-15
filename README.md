## Guía rápida para entender el árbol y la poda alfa-beta
https://youtu.be/I0y-TGehf-4?si=z9DLF0E50HZ5fRQX 

1. **¿Cómo se representa cada nivel?**
   - El árbol se representa con **listas anidadas**.
   - Cada lista corresponde a un nodo interno.
   - Cada número corresponde a una **hoja**.
   - Ejemplo: `[[[3, 5], [6, 9]], [[1, 2], [8, 4]], [[7, 10], [2, 6]]]`.
   - El primer nivel contiene 3 hijos, cada hijo contiene 2 subárboles, y cada subárbol contiene hojas.

2. **¿Quién es MAX y quién MIN en cada nivel?**
   - La raíz se define al crear el solver con `preferencia = "max"` o `preferencia = "min"`.
   - Si la raíz es `MAX`, los niveles alternan así: `MAX -> MIN -> MAX -> MIN`.
   - Si la raíz es `MIN`, los niveles alternan así: `MIN -> MAX -> MIN -> MAX`.
   - El nivel se decide por la profundidad del nodo.

3. **¿Cómo se obtiene la secuencia de jugadas?**
   - En cada nivel se guarda el índice del hijo elegido.
   - Si la mejor jugada está en el hijo 2 del nivel raíz y luego en el hijo 1 del siguiente nivel, la secuencia parcial sería `[2, 1]`.
   - La secuencia completa se arma al unir los índices elegidos desde la raíz hasta la hoja final.
   - Esa secuencia está en `secuencia_optima`.

4. **¿Cómo se identifica exactamente una rama podada?**
   - Una rama queda podada cuando ya no puede mejorar el resultado actual.
   - En `MAX`, se poda cuando `alfa >= beta`.
   - En `MIN`, se poda cuando `alfa >= beta`.
   - La rama podada se guarda con su `path`, que indica exactamente qué posición del árbol fue descartada.
   - En la traza también aparece el motivo de la poda y los valores de `alfa` y `beta` en ese momento.

5. **¿Cómo se representa un árbol de 1, 2, 3, 4 y 5 niveles?**
   - **1 nivel:** `[3, 5, 2]`
   - **2 niveles:** `[[3, 5], [6, 9]]`
   - **3 niveles:** `[[[3, 5], [6, 9]], [[1, 2], [8, 4]]]`
   - **4 niveles:** `[[[[3], [5]], [[6], [9]]], [[[1], [2]], [[8], [4]]]]`
   - **5 niveles:** `[[[[[3]], [[5]]], [[[6]], [[9]]]], [[[[1]], [[2]]], [[[8]], [[4]]]]]`

La idea general es leer el árbol de arriba hacia abajo, alternando MAX y MIN, y seguir solo la ruta que produce el mejor valor sin explorar ramas que ya no aportan.

### ¿Cómo se determinan alfa y beta?

- **Alfa ($\alpha$)** es el mejor valor que ha encontrado **MAX** hasta el momento.
- **Beta ($\beta$)** es el mejor valor que ha encontrado **MIN** hasta el momento.
- No cambian de significado según el nivel; lo que cambia es **quién los actualiza**.
- En un nodo **MAX** se actualiza `alpha = max(alpha, valor_hijo)`.
- En un nodo **MIN** se actualiza `beta = min(beta, valor_hijo)`.
- La poda ocurre cuando `alpha >= beta`, porque ya no tiene sentido seguir explorando esa rama.

### Regla por nivel

- Si la raíz es `MAX`, los niveles quedan: `MAX -> MIN -> MAX -> MIN`.
- Si la raíz es `MIN`, los niveles quedan: `MIN -> MAX -> MIN -> MAX`.
- El criterio del nivel depende de la **profundidad** del nodo, no del valor de las hojas.

### Idea práctica

- **MAX** intenta subir el valor de `alpha`.
- **MIN** intenta bajar el valor de `beta`.
- Si en algún punto `alpha` ya es mayor o igual que `beta`, la rama se poda porque el resultado ya no puede mejorar.

### Descripción de la implementación

Esta notebook implementa poda alfa-beta usando una clase `AlphaBetaSolver`.

- La preferencia inicial de la raíz se define al crear el objeto con `preferencia = "max"` o `preferencia = "min"`.
- El árbol se representa con listas anidadas de Python.
- El método `minimax(...)` calcula el valor óptimo sin poda.
- El método `alpha_beta_pruning(...)` calcula el valor óptimo con poda alfa-beta y además guarda una traza paso a paso.
- El método `render_tree(...)` imprime el árbol en texto, mostrando comparaciones, actualizaciones de alfa y beta, y ramas podadas.

La estructura general sigue este flujo:

1. Se normaliza la preferencia de la raíz.
2. Se recorren los niveles alternando MAX y MIN.
3. Se actualizan `alfa` y `beta` en cada nodo.
4. Cuando se cumple el criterio de poda, se registran las ramas descartadas.
5. Al final se imprime una vista textual del árbol para revisar el proceso.](https://youtu.be/I0y-TGehf-4?si=z9DLF0E50HZ5fRQX 

1. **¿Cómo se representa cada nivel?**
   - El árbol se representa con **listas anidadas**.
   - Cada lista corresponde a un nodo interno.
   - Cada número corresponde a una **hoja**.
   - Ejemplo: `[[[3, 5], [6, 9]], [[1, 2], [8, 4]], [[7, 10], [2, 6]]]`.
   - El primer nivel contiene 3 hijos, cada hijo contiene 2 subárboles, y cada subárbol contiene hojas.

2. **¿Quién es MAX y quién MIN en cada nivel?**
   - La raíz se define al crear el solver con `preferencia = "max"` o `preferencia = "min"`.
   - Si la raíz es `MAX`, los niveles alternan así: `MAX -> MIN -> MAX -> MIN`.
   - Si la raíz es `MIN`, los niveles alternan así: `MIN -> MAX -> MIN -> MAX`.
   - El nivel se decide por la profundidad del nodo.

3. **¿Cómo se obtiene la secuencia de jugadas?**
   - En cada nivel se guarda el índice del hijo elegido.
   - Si la mejor jugada está en el hijo 2 del nivel raíz y luego en el hijo 1 del siguiente nivel, la secuencia parcial sería `[2, 1]`.
   - La secuencia completa se arma al unir los índices elegidos desde la raíz hasta la hoja final.
   - Esa secuencia está en `secuencia_optima`.

4. **¿Cómo se identifica exactamente una rama podada?**
   - Una rama queda podada cuando ya no puede mejorar el resultado actual.
   - En `MAX`, se poda cuando `alfa >= beta`.
   - En `MIN`, se poda cuando `alfa >= beta`.
   - La rama podada se guarda con su `path`, que indica exactamente qué posición del árbol fue descartada.
   - En la traza también aparece el motivo de la poda y los valores de `alfa` y `beta` en ese momento.

5. **¿Cómo se representa un árbol de 1, 2, 3, 4 y 5 niveles?**
   - **1 nivel:** `[3, 5, 2]`
   - **2 niveles:** `[[3, 5], [6, 9]]`
   - **3 niveles:** `[[[3, 5], [6, 9]], [[1, 2], [8, 4]]]`
   - **4 niveles:** `[[[[3], [5]], [[6], [9]]], [[[1], [2]], [[8], [4]]]]`
   - **5 niveles:** `[[[[[3]], [[5]]], [[[6]], [[9]]]], [[[[1]], [[2]]], [[[8]], [[4]]]]]`

La idea general es leer el árbol de arriba hacia abajo, alternando MAX y MIN, y seguir solo la ruta que produce el mejor valor sin explorar ramas que ya no aportan.

### ¿Cómo se determinan alfa y beta?

- **Alfa ($\alpha$)** es el mejor valor que ha encontrado **MAX** hasta el momento.
- **Beta ($\beta$)** es el mejor valor que ha encontrado **MIN** hasta el momento.
- No cambian de significado según el nivel; lo que cambia es **quién los actualiza**.
- En un nodo **MAX** se actualiza `alpha = max(alpha, valor_hijo)`.
- En un nodo **MIN** se actualiza `beta = min(beta, valor_hijo)`.
- La poda ocurre cuando `alpha >= beta`, porque ya no tiene sentido seguir explorando esa rama.

### Regla por nivel

- Si la raíz es `MAX`, los niveles quedan: `MAX -> MIN -> MAX -> MIN`.
- Si la raíz es `MIN`, los niveles quedan: `MIN -> MAX -> MIN -> MAX`.
- El criterio del nivel depende de la **profundidad** del nodo, no del valor de las hojas.

### Idea práctica

- **MAX** intenta subir el valor de `alpha`.
- **MIN** intenta bajar el valor de `beta`.
- Si en algún punto `alpha` ya es mayor o igual que `beta`, la rama se poda porque el resultado ya no puede mejorar.

### Descripción de la implementación

Esta notebook implementa poda alfa-beta usando una clase `AlphaBetaSolver`.

- La preferencia inicial de la raíz se define al crear el objeto con `preferencia = "max"` o `preferencia = "min"`.
- El árbol se representa con listas anidadas de Python.
- El método `minimax(...)` calcula el valor óptimo sin poda.
- El método `alpha_beta_pruning(...)` calcula el valor óptimo con poda alfa-beta y además guarda una traza paso a paso.
- El método `render_tree(...)` imprime el árbol en texto, mostrando comparaciones, actualizaciones de alfa y beta, y ramas podadas.

La estructura general sigue este flujo:

1. Se normaliza la preferencia de la raíz.
2. Configuramos el árbol de entrada (automático - manual).
3. Se recorren los niveles alternando MAX y MIN.
3. Se actualizan `alfa` y `beta` en cada nodo.
4. Cuando se cumple el criterio de poda, se registran las ramas descartadas.
5. Al final se imprime una vista textual del árbol para revisar el proceso y una visualización gráfica.

### Ejemplo de Prueba:
- preferencia = "max"
- arbol_manual = [[3, -5, 2], [-5, -7, 4], [-9, 6, -8]]
- Minimax (sin poda): {'value': -5, 'sequence': [0, 1]}
- Alpha-Beta (con poda):
  valor: -5
  secuencia óptima: [0, 1]
  ramas podadas: 4
- Figura:
<img width="1289" height="791" alt="image" src="https://github.com/user-attachments/assets/bab01de8-f59c-40ad-ad7f-073d6f04bebc" />

