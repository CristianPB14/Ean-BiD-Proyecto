# nivel2_sensibilidad.md

```jsx
--- ANÁLISIS DE SENSIBILIDAD ---
g = 1% -> t_umbral = -126.2 períodos
g = 2% -> t_umbral =  -63.4 períodos
g = 4% -> t_umbral =  -32.0 períodos
g = 8% -> t_umbral =  -16.3 períodos
g = 16% -> t_umbral =   -8.5 períodos
```

- ¿Duplicar `g` reduce el umbral a la mitad? Si no, ¿qué relación observa y por qué?
    
    No exactamente a la mitad, pero el impacto es asimétrico y severo. Debido a que el crecimiento de los datos es compuesto, una tasa que se duplica acorta el horizonte de forma agresiva.
    
- ¿Qué error en la estimación de `g` cambia su recomendación de arquitectura? ¿Uno de un punto porcentual, o hace falta más?
    
    Un error de apenas un par de puntos porcentuales (por ejemplo, estimar 2% en lugar del 4% real) es suficiente para alterar la recomendación. Un cálculo impreciso de $g$ significa la diferencia entre tener un año de margen para migrar a un clúster o sufrir un colapso en la RAM el mes siguiente.
    
- ¿Qué es más grave para la decisión: equivocarse en `g` o equivocarse en `k`?
    
    Como se corrobora matemáticamente en la Pista 3 de la guía, equivocarse en $g$ es considerablemente más grave. En la fórmula, el factor $k$ está encapsulado dentro del logaritmo del numerador, lo cual amortigua drásticamente su impacto ante un error de estimación. Por el contrario, $g$ gobierna la base logarítmica del denominador, por lo que cualquier variación destruye la precisión de toda la proyección temporal.