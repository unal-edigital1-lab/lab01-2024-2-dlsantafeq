```markdown
# lab01- sumador

## nombres
- Deyvid Leonardo Santafe Quicazaque – 1031168776

## Introducción
En este laboratorio se trabajó en el diseño, simulación e implementación de un sumador completo de 1 bit utilizando Quartus Prime Lite. Se realizaron dos implementaciones en Verilog: una mediante la instanciación directa de puertas lógicas y otra a través de una descripción funcional. Este documento integra la documentación, comentarios, entregables, comparaciones y conclusiones en un solo archivo para facilitar la comprensión y continuidad del trabajo.

## Especificación
El sumador de 1 bit se define con:
- **Entradas:**
  - **A** y **B:** Bits a sumar.
  - **Cin:** Acarreo de entrada.
- **Salidas:**
  - **S:** Resultado de la suma.
  - **Cout:** Acarreo de salida.

La tabla de verdad es la siguiente:

| A | B | Cin | S | Cout |
|---|---|-----|---|------|
| 0 | 0 |  0  | 0 |  0   |
| 0 | 0 |  1  | 1 |  0   |
| 0 | 1 |  0  | 1 |  0   |
| 0 | 1 |  1  | 0 |  1   |
| 1 | 0 |  0  | 1 |  0   |
| 1 | 0 |  1  | 0 |  1   |
| 1 | 1 |  0  | 0 |  1   |
| 1 | 1 |  1  | 1 |  1   |

## Bloque Funcional y Lógica Combinacional
El bloque funcional define cómo se interconectan las operaciones lógicas para realizar la suma y gestionar el acarreo. En este laboratorio, se simplifica diciendo que se utilizan operaciones lógicas (como AND, OR y XOR) para combinar las entradas y obtener el resultado de manera inmediata, sin depender de información previa. Es decir, el circuito calcula el resultado en función de las entradas actuales.

---

## Entregable 1: Documentación y Comentarios del Código HDL (sum1bcc_primitive.v)

### Código HDL con Puertas Lógicas
```verilog
module sum1bcc_primitive (A, B, Ci, Cout, S);

  input  A;
  input  B;
  input  Ci;
  output Cout;
  output S;

  wire a_ab;
  wire x_ab;
  wire cout_t;

  // Se genera el producto A y B.
  and(a_ab, A, B);
  // Se calcula la suma parcial de A y B.
  xor(x_ab, A, B);

  // Se suma la entrada de acarreo.
  xor(S, x_ab, Ci);
  // Se calcula parte del acarreo.
  and(cout_t, x_ab, Ci);

  // Se combina el acarreo parcial y el generado por A y B.
  or (Cout, cout_t, a_ab);

endmodule
```

**Comentarios:**  
- Se definen señales intermedias (`a_ab`, `x_ab`, `cout_t`) que permiten calcular tanto la suma parcial como el acarreo.  
- Se utilizan puertas lógicas: AND para el producto de A y B, XOR para la suma parcial y OR para combinar los términos que generan el acarreo.  
- Esta implementación muestra de manera explícita cómo se cumple la tabla de verdad del sumador a nivel de componentes.

---

## Entregable 2: Proyecto en Quartus y Comparación de Implementaciones (sum1bcc.v)

### Código HDL con Descripción Funcional
```verilog
module sum1bcc (A, B, Ci, Cout, S);

  input  A;
  input  B;
  input  Ci;
  output Cout;
  output S;

  reg [1:0] st;

  assign S = st[0];
  assign Cout = st[1];

  always @ ( * ) begin
    st <= A + B + Ci;
  end
  
endmodule
```

**Comentarios:**  
- Se utiliza un registro de 2 bits (`st`) donde el bit menos significativo representa la suma (S) y el más significativo el acarreo (Cout).  
- El bloque `always` recalcula la suma cada vez que hay un cambio en las entradas, de forma que el resultado se asigna directamente a las salidas.  
- Este enfoque funcional es más compacto y permite que el compilador optimice la síntesis sin detallar cada operación lógica individualmente.

### Comparación de las Implementaciones
- **Implementación con Puertas Lógicas (sum1bcc_primitive):**  
  - Permite visualizar explícitamente cada operación lógica.  
  - Es ideal para comprender el funcionamiento interno del circuito a nivel de hardware.
- **Implementación Funcional (sum1bcc):**  
  - Ofrece una descripción más concisa y facilita las optimizaciones durante la síntesis.  
  - Es adecuada para proyectos donde se prefiere una implementación compacta sin detallar cada componente.

---

## Conclusiones
El desarrollo de este laboratorio me permitió reforzar y aplicar conceptos de diseño digital y descripción de hardware utilizando Verilog. A través de ambas implementaciones, se evidencia que:

- La implementación a nivel de puertas lógicas ofrece un entendimiento detallado del proceso de suma, lo cual es útil para estudios teóricos y diagnósticos de circuitos.
- La descripción funcional resulta en un código más compacto y optimizable, facilitando el diseño cuando se busca eficiencia y claridad en la síntesis.
- La experiencia con Quartus Prime Lite y la práctica de documentar cada paso aseguran que futuros proyectos y compañeros podrán comprender y continuar el trabajo de manera fluida.
