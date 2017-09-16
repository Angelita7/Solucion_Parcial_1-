Angela_Restrepo-Miguel_Herrera
 
# Solucion_Parcial_1

1. Describa la Taxonomía de Flynn.
```
R/: Número de instrucciones y de secuencia de dato que la computadora utiliza
para procesar información. Existen cuatro tipos de computadoras.

-SISD (Una instrucción, un dato).
-MISD (Multiples instrucciones, un dato).
-SIMD (Una instrucción, multiples datos).
-MIMD (Multiples instrucciones, multiples datos).
```
2. Diga cuales son los 4 principios de diseño.
```
R/:
1. La simplicidad favorece la regularidad
2. Entre más pequeño, más rápido
3. Hacer el caso común más rápido
4. Grandes diseños demandan grandes compromisos
```
```
3. Explique los tres formatos que se usan en la arquitectura SPARC V8, y que instrucciones usan los formatos correspondientes a la arquitectura **SPARC V8** 
```
### Fomato 1. Para funciones de llamado.

| op |        disp30|
31  29              0

Donde el Op especifica el tipo de instrucciones y el Disp30 se utiliza para almacenar el numero de dewsplazamientos.
```
```
### Fomato 2.  Se utiliza para los Branch, Sethi, Nop
| op | a |cond | op2 |       disp22|
31  29  28    24    21             0

Op---> Indica el tipo de instrucciones.
a----> Indica si se debe ejecutar las innstruciones que hay por debajo(0 o 1).
Cond-> Elige cual es la instruccion que se utiliza.
Op2--> Indica que tipo de operacion o el operando, se realizara  en la comparacion.

| op | rd | 100|       disp22|
31   29   24   21            0

rd--> registo destino.
```
```
### Fomato 3. Para Aritmetica y logicas.

Con inmediato. 
| op | rd |op3 | rs1 | i=0 | unused(zero) | rs2 |
31  29    24  18     13    12           4     0

Sin inmediato.
| op | rd |op3 | rs1 | i=1 |     inm13       |
31  29    24  18     13    12                0

Rd--> Registro destino.
Op3-> Indica el tipo operacion.
Rs1-> Registro fuente 1.
Rs2-> Registro fuente 2.
i---> Indica si hay inmediato o no.
inm-> Inmediato.
```
4. Explique cómo inicializar un valor grande, que ocupe más de 13 bits, en la arquitectura **SPARC V8**.
```
R/ Para inicializar un número que se pasa de 13 bits:

* Pasar el número decimal a binario.
* Al número binario se le completa a las izquierda los bits hasta tener 32 bits.
* Si el número es positivo y para completar los 32 bits solo ponemos 0 de derecha a izquierda, sacamos los 22 bits más significativos.
* Si el número es negativo, se pasa a binario positivo, se completan los 32 bits con 0 de derecha a izquierda, se invierte el número binario a positivo y se le suma 1, se le realiza complemento a 2, después se toman los 22 bits más significativos.
* Al finalizar asignamos registros y declaramos con el Sethi----> 22 + significativos y con el OR------> El sobrante.

```
5. Como puedo reescribir la instrucción **(OR y SUBcc)** cuando inicializo y  comparó 2 registros.
```
R/: 

* OR se puede reescribir con la instrucción MOV
* SUBcc se puede reescribir con la instrucción CMP
```
6. ¿Qué instrucciones utilizan el delay slot antes de saltar?
```
R/:Las intrucciones que utilizan el Delay Slot antes de saltar son:

* Call
* Branch
* Jump and Link

```
7. ¿Qué significa el bit **a**, en el formato 2 de las instrucciones **BRANCH**?
```
R/: El bit "a", en el formato 2 de las instrucciones BRANCH significa:

* Si "a" esta en 0 ejecuta la instrucción que esta por debajo y luego salta.
* Si "a" esta en 1 no ejecuta la instrucción que esta por debajo, solo salta.

```
8. ¿Por que la instrucción **CALL** utilizar el registro %o7 ---> registro 15.?
```
R/: La instrucción CALL utiliza el registro %o7(registro 15) por que es la dirección actual.
```
9. convertir el programa en lenguaje de máquina a lenguaje ensamblador y luego a lenguaje de alto nivel el siguiente programa:

```
R/: 
|10|10000|000010|00000|1|0000000000101|
```
Op(10)--> Formato 3
Rd(10000)-->%L0
Op3(000010)--> OR
Rs1(00000)-->%g0
i(1)--> inmediato
Rs2(0101)--> 5
```


|10|10001|000010|00000|1|1111111111010|
```
Op(10)--> Formato 3
Rd(10001)-->%L1
Op3(000010)--> OR
Rs1(00000)--> %g0
i(1)-->inmediato
Rs2(1010)--> 10
```
|10|01000|000000|10001|0|0000000010000|
```
Op(10)-->Formato 3 
Rd(01000)-->%o0
Op3(000000)--> Add
Rs1(10001)--> %L1
i(0)--> No hay inmediato
Rs2(10000)--> %L0
```
10. Convierta el siguiente código a lenguaje ensamblador, máquina **SPARC V8** y hexadecimal.
a.
 ```c.
 int main(){
 int a = 8;
 int b = -16800;
 int c = 33; 
 if((a+b)<=b*32){
 	c=a+(b*2);
	}
else{
	return b;
}
	return a+c;
}


Lenguaje de Bajo Nivel
0x0000 MOV 8, %L0
0x0004 SETHI -17, %L1
0x0008 OR %L1, 608, %L1
0x000C MOV 33, %L2
0x0010 ADD %L1, 33, %L4
0x0014 SLL %L1, 32, %L4
0x0018 CMP %L3, %L4
0x001C BG a Etiqueta
0x0020 SLL %L1, 2, %L5
0x0024 ADD %L0, %L5,%L2
0x00

 ```
 

b.
 ```c
int main(){
	int a = 8;
	int b = -10;
	if(a!=b){
		return c/8;
}
else{
	return b;
}
}
```
c.
 ```c
int main(){
	int a = -21180;
}
```

11. Convierta el siguiente código a lenguaje ensamblador, máquina **SPARC V8** y hexadecimal.
 ```c
int test(int a, int b, int c){
	int z;
	z = a - b + c*4;
	return z + 2;
}

int main(){
	int p = 4, y = 2, c = -128;
	int x = test(p,y,c);
	return x + 45;
}
 ```
12. Implemente una función **Mul** en lenguaje de alto nivel, lenguaje ensamblador **SPARC V8** y lenguaje de máquina SPARC V8 que realice la multiplicación de dos enteros sin signo usando solo sumas.
13. Implemente la función **Pot** en lenguaje de alto nivel,lenguaje ensamblador **SPARC V8** y lenguaje de máquina SPARC V8 que realice la potencia de dos números enteros sin signo realizando llamados a la función desarrollada en el punto 9.
14. Implemente una función **Fact** en lenguaje de alto nivel, lenguaje ensamblador **SPARC V8** y lenguaje de máquina SPARC V8 que calcule el factorial de un número entero sin signo.
15. Implemente una función **Div** en lenguaje de alto nivel, lenguaje ensamblador **SPARC V8** y lenguaje de máquina SPARC V8 que calcule la division de un número entero sin signo.


