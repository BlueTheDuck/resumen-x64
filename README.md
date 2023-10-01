# resumen-x64

## GDB

El manual completo esta en `info gdb`

### Comandos

- `x`: Leer de memoria e imprimir.  Usa el formato `/NFU`
  - `N`: Cantidad
  - `F`: [Formato](#formato)
  - `U`: [Unidad](#unidad)
- `p`: Evalua expresión. Usa el formato `/F`
  - `F`: [Formato](#formato)
- `b`: Setear breakpoint
- `c`: Continue
- `delete breakpoints <indice>`
- `si`: "step instruction"

### Dashboard

Resumen de funciones utiles:
 - `dashboard <modulo>`: Habilita/deshabilita el modulo
 - `dashboard expressions`
   - `dashboard expressions watch /16 <expr>` 
   - `dashboard expressions unwatch <indice>` 

### Formato

De la sección 10.5 "Output formats" en `info gdb`

- `x`: Hexadecimal
- `z`: Hexadecimal con padding
- `d`: Decimal
- `u`: Decimal sin signo
- `t`: Binario (*t*wo's complement)
- `a`: Dirección (Te muestra el label mas cercano)
- `f`: Floating point
- `s`: Null-terminated string

### Unidad

De la sección 10.6 "Examining Memory"

- `b`: De a un byte
- `h`: De a 2 bytes
- `w`: De a 4 bytes
- `g`: De a 8 bytes
  
**Nota**: El valor por defecto es el último usado

## SIMD

### Instrucciones

### Snippets

#### Complemento a dos:

```asm
neg: ; Parametro y ret por xmm0
    movdqa  xmm1, xmm0
    pxor    xmm0, xmm0  ; xmm0 := 0
    psub⍰   xmm0, xmm1  ; xmm0 := 0 - xmm1
    ret
```
`⍰`: [Tamaño](#tamaños) `b`, `w`, `d`, `q`

#### Complemento a uno (negación)

```asm
neg: ; Parametro y ret por xmm0
    pcmpeqd xmm1, xmm1  ; xmm1 := (-1) (ó 0b1111...)
    pxor    xmm0, xmm1  ; xmm0 := xmm0 ⊻ 0b1111...
    ret
```

## C

### Convencion de C

## Assembly

### Declaración de constantes

Cuidado con la endianness:
```asm
Good_Black_RGBA: db 0, 0, 0, 0xFF
Bad_Black_RGBA: dw 0x000000FF
```
> `> x/4b &Good_Black_RGBA`
> 
> `0x..0 <Good_Black_RGBA>: 0, 0, 0, 0xFF`
> 
> `> x/4b &Bad_Black_RGBA`
> 
> `0x..0 <Bad_Black_RGBA>: 0xFF, 0, 0, 0`

El canal rojo está al max!