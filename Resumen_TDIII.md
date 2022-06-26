# Resumen Segundo Parcial TDIII - Algoritmos y Estructuras de Datos.

## Tabla de contenidos

- [Complejidad algorítmica](#complejidad-algortmica)
- [Ordenamiento](#ordenamiento)
- [Tipos Abstractos de Datos](#tipos-abstractos-de-datos)
- [Memoria Dinámica en C++](#memoria-dinmica-en-c)
- [Iteradores](#iteradores)
- [Pilas y Colas](#pilas-y-colas)
- [Arbol Binario](#arbol-binario)

## Complejidad algorítmica

### Big O Notation
- Cota superior asintótica, “Worst case”.
- Def: Sean f y g funciones f y g: ℕ>=0 -> ℝ
    
        O(g(n)) = {f(n) | ∃ c > 0 y n0 > 0 / 0 <= f (n) <= c · g(n) ∀ n >= n0}

### Big Ω Notation
- Cota inferior asintótica, “Best case”.
- Def: Sean f y g funciones f y g: ℕ >= 0 -> ℝ
    
        Ω(g(n)) = {f(n) | ∃ c > 0 y n0 > 0 / 0 <=  c · g(n) <= f(n) ∀ n >= n0}

### Big Θ Notation
- Cota ajustada asintótica, “Average Case”.
- Def: Sean f y g funciones f y g: ℕ>=0 -> ℝ
    
        Θ(g(n)) = {f(n) | ∃ c1, c2 > 0 y n0 > 0 / 0 <= c1 · g(n) <= f(n) <= c2 · g(n) ∀ n >= n0} = O(g(n)) ∩ Ω(g(n))


##### Ejemplo     
```cpp
bool buscar(int elem, const vector<int> & v){
	int res = false;
	int i = 0;
	while(i < v.size() && res == false){
		if (v[i] == elem){
			res = true;
		}
	i = i + 1;
	}
	return res;
}
```
1. Determinar cuál es la medida de tamaño de entrada para el algoritmo dado.
- Tamaño de la entrada: |v|
2. Determinar qué valores de entrada constituyen el peor caso del
algoritmo.
- Peor caso: elem no está en v.
3. Derivar del código del algoritmo la función de costo T que toma el tamaño de entrada como parámetro y calcula el costo computacional para el peor caso.
- Mejor caso: elem es el primer elemento de v.

4. Proponer una función f que será el órden de complejidad y demostrar que, según corresponda, T ∈ O(f), o T ∈ Ω(f ), o
T ∈ Θ(f

#### Función de costo T(n) y Árbol de recursión 
(ver Apunte.)

## Ordenamiento

### Selection Sort
Buscar el minimo de la ```lista[k:]``` e insertarlo al principio. Repetir con ```k++``` hasta que ``` k = |lista|```
### Insertion Sort
Segmentar lista en dos, tomar elemento de la segunda parte e insertarlo de forma ordenada en la primera parte.
### Bubble Sort
Recorrer la lista swapeando pares de elementos si corresponde. Repetir n veces.
### Merge Sort
**Divide & conquer**
Dividir la lista en dos mitades, ordenar las mitades recursivamente, combinar ambas partes de forma ordenada.
- Caso base: si el arreglo es de tamaño 1 (está ordenado); 
- Si no, realizamos divide y partimos el arreglo en dos mitades iguales (a=2, b=2), luego ordenamos cada mitad por separado, 
- Finalmente hacemos conquer al combinar los dos subarreglos ordenados.

```cpp 
//Pseudo
vector<int> MergeSort(vector<int> V)
    1. Si |V| ≤ 1:
        Devolver V
    2. med = |V|/2
    3. izq = MergeSort(V[0 . . . med − 1])
    4. der = MergeSort(V[med . . . |V| − 1])
    5. Devolver Merge(izq, der)
    //Donde Merge(V1, V2) es una función que toma dos vectores ordenados y devuelve la union ordenada de ambos.

vector<int> merge_sort(const vector<int> & v, int i, int j){
    // Ordena los elementos desde d hasta h (sin incluir)

    // Si hay 0 o 1 elementos, ya están ordenados
    if(j - i == 0)
        return {};
    if(j - i == 1)
        return {v[d]};

    // Divide y ordena cada mitad
    int med = (i+j)/2;
    vector<int> izq = mergesort(v, i, med);
    vector<int> der = mergesort(v, med, j);

    // Devuelve la unión ordenada de ambas mitades
    return merge(izq, der);
}

vector<int> merge(const vector<int> & v1, const vector<int> & v2){
    vector<int> res; int i = 0; int j = 0;
    while(i < v1.size() && j < v2.size()){
        if(v1[i] < v2[j]){
            res.push_back(v1[i]);
            i++;
        } else {
            res.push_back(v2[j]);
            j++;
        }
    }
    while(i < v1.size()){
        res.push_back(v1[i]);
        i++;
    }
    while(j < v2.size()){
        res.push_back(v2[j]);
        j++;
    }
    return res;
}
```
El tamaño de entrada n es j − i.

            { c               si n = 0, n = 1,
    T(n) = 
            { 2T(n/2) + cn    si n > 1

- Altura del árbol: log n + 1 niveles.
- Suma en cada nivel: cn.
- Total: cn log n + cn
- Propuesta: T(n) ∈ O(n log n)

Demostración por inducción

            { c               si n = 0, n = 1,
    T(n) = 
            { 2T(n/2) + cn    si n > 1

- Queremos ver que T(n) ≤ k · (n log n), para algún k.
- Caso base:
    - Con n = 0,
T(0) ≤ k · (0 log 0). No sirve, log 0 no está denido.
    - Con n = 1,
T(1) ≤ k · (0 log 1)
c ≤ k · 0, no se cumple.
    - Con n = 2,
T(2) ≤ k · (2 log 2)
2T(1) + 2c ≤ 2k
2c + 2c ≤ 2k
2c ≤ k
    - Tomamos k = 2c y n0 = 2

- Caso inductivo: queremos ver que T(n) ≤ k · (n log n)
- Hipótesis inductiva: vale T(x) ≤ k · (x log x) para x < n.
- Sabemos que T(n) = 2T(n/2) + cn
- ≤ 2k(n/2) log(n/2) + cn, por hipótesis inductiva
- = kn log(n/2) + cn
- = kn log n − kn log 2 + cn
- = kn log n − kn + cn, ya que log 2 = 1.
- ≤ kn log n, siempre que k ≥ c.
- Vale, ya que habíamos tomado k = 2c.

Los problemas de Divide & Conquer tienen la siguiente recurrencia:

            { c                si n = 0, n = 1
    T(n) = 
            { aT(n/b) + f (n)   si n > 1
    (a subproblemas de tamaño n/b, f (n): costo de conquer)

Algunas recurrencias comunes y sus órdenes de complejidad
    
    aT(n/a) + O(n) = O(n loga n) (a=2 es mergesort)
    2T(n/2) + O(1) = O(n) (recorrer arbol binario)
    T(n/2) + O(1) = O(log n) (búsqueda binaria)

### Quick Sort
Elegir un elemento pivot y dividir el vector entre los menores y los mayores al pivot.
Ordenar cada partición recursivamente

```cpp
void quicksort(vector<int> & v, int d, int h){
    if(d < h - 1){
        int pos = dividir(v, d, h);
        quicksort(v,d,pos);
        quicksort(v,pos+1,h);
    }
}

int dividir(vector<int> & v, int d, int h){
    int pivot = v[h-1];
    int i = d;
    for(int j = d; j < h - 1; j++){
        if(v[j] <= pivot){
            swapear(v, i, j);
            i = i + 1;
        }
    }
    swapear(v, i, h-1);
    return i;
}
//La función swapear(v,i,j) toma v por referencia e intercambia los elementos de las posiciones i y j.
```
### Counting Sort

Todos los elementos son menores a un valor ```k```

Armamos un vector auxiliar c de k+1 posiciones ```(del 0 a k)``` inicializadas con el valor 0.
El elemento ```c[i]``` queremos que sea un contador que almacene la cantidad de veces que aparece el ```valor i``` en el vector de entrada.
Recorremos el vector de entrada y llenamos los contadores de ```c```.
Recorremos el vector ```c``` y construimos el vector de salida agregando ```i```
la cantidad de veces que indica```c[i]```

```cpp
vector<int> counting_sort(const vector<int> & v, int k){
    vector<int> contadores(k+1); // O(k+1)
    vector<int> res; // O(1)

    // Inicializo los contadores en 0;
    for(int i = 0; i < k+1; i++) // k+1
        contadores[i] = 0; // O(1)

    // Recorro v y actualizo el contador correspondiente.
    for(int i = 0; i < v.size(); i++) // O(n)
        contadores[v[i]] = contadores[v[i]] + 1; // O(1)

    // Recorro los contadores y armo el vector ordenado.
    for(int i = 0; i < k+1; i++) // Total = k+1 + O(n)
        for(int j = 0; j < contadores[i]; j++)
            res.push_back(i);

    return res;
} // T(n) es O(n)

```
T(n, k) ∈ O(n + k)
Si k es un valor constante, entonces T(n, k) ∈ O(n)

## Tipos Abstractos de Datos

### Especificación:
**Interfaz**: describe las operaciones que se pueden realizar sobre una instancia del tipo abstracto.
- Comportamiento funcional (operaciones, tipos de los parámetros de entrada y salida, Pre y Post condiciones)
- Comportamiento no funcional (complejidad temporal de las operaciones, aliasing, manejo de memoria).

Una buena interfaz minimiza la visibilidad de las decisiones internas que se hayan tomado para implementar el tipo abstracto.

### Implementación:

**Estructura de Representación**: describe cómo se implementa el tipo abstracto en base a otros tipos.

La estructura de representación describe con qué tipos de datos concretos construiremos una instancia del tipo abstracto.
Para implementar el mismo tipo abstracto, podríamos elegir diferentes estructuras dependiendo del contexto.

**Invariante de Representación: Rep**
- La estructura de representación debe mantener su coherencia interna. Estas propiedades se documentan formalmente en un **Invariante de representación**.
- Todos los algoritmos pueden asumir que vale Rep en la Pre.
- Todos los algoritmos deben asegurar que vale Rep en la Post.
- El Rep puede ayudar a implementar algoritmos que satisfacen complejidades temporales deseadas.

El invariante *Rep(c : C)* es un predicado lógico que toma una instancia del tipo C y puede predicar sobre el contenido de la parte privada de C.


**Algoritmos**: implementan las operaciones que describe la interfaz, operando sobre la estructura de representación respetando y manteniendo su coherencia interna.

Una vez definida una interfaz y elegida una estructura, estamos en condiciones de escribir los algoritmos que implementarán la interfaz definida, operando sobre la estructura de manera adecuada.
Nuestro código será responsable de mantener el Rep de la estructura de representación.
El código puede aprovechar las restricciones del Rep para cuestiones de, por ejemplo, eficiencia.


Vamos a describir un tipo abstracto con cuatro tipos de operaciones:
- **Observadores**: devuelven toda la información que caracteriza a una instancia, sin modificarla; deberían ser un conjunto minimal
- **Constructores**: crean nuevas instancias del tipo abstracto
- **Modificadores**: modifican la instancia, pueden devolver información.
- **Otras operaciones**: otros observadores no esenciales.


## Memoria Dinámica en C++


### Heap
El Heap es el espacio de memoria dinámica, está disponible para su uso a pedido y su límite de espacio es la cantidad de memoria que se encuentra libre en la computadora.

### ```T* operator new T```
- El operador new T crea un nuevo objeto de tipo T en el heap, y devuelve un puntero ```(T*)``` a éste.
- El objeto creado es independiente del scope en el que es creado y lo "sobrevive".

### ```T operator*(T*)```

- El ```operator*(T*)``` devuelve una referencia al objeto de tipo T apuntado por el puntero.
- Podría pasar que el puntero tenga una dirección de memoria que aloja un valor "basura" no válido.
- Si el puntero es invalido o ya fue hecho delete, la operación puede abortar el programa..

### ```operator->```
- Cuando tenemos un ```class T*``` o ```struct T*```, el ```operator->``` es una manera de acceder a métodos o variables miembro.
- Sintácticamente, para una variable p de tipo class T* o struct T*,
    - ```p->f()``` es equivalente a ```*p.f()``` y
    - ```p->item``` es equivalente a ```*p.item```

### ```operator delete *T```
- El operador ```delete(T*)``` elimina al objeto de tipo T existente en el heap apuntado por el puntero.
- La variable de tipo puntero sigue existiendo, pero ya no apunta a un objeto válido.
- Se suele asignar la constante nullptr cuando un puntero ya no apunta a nada, o cuando inicialmente todavía no apunta a nada.

### Destructor
Una clase A en C++ siempre tiene un método destructor: ```~A()```
- El método destructor se invoca automáticamente cuando el objeto se termina el scope del objeto.
- El método destructor también se invoca cuando el objeto se
destruye explícitamente con un delete.
- En general, si no usamos memoria dinámica de manera explítica, no es necesario que implementemos el método.
- En cambio, si usamos new en nuestros métodos tenemos que ocuparnos que toda la memoria que pedimos se libere con un delete.

## Iteradores 

Un iterador es un tipo asociado a un contenedor que condensa las funcionalidades de recorrer, eliminar y "apuntar".
El contenedor debe proveer en su interfaz pública operaciones que devuelvan y operen con iteradores

### Operaciones del contenedor
- ```begin()```: devuelve un iterador al primer elemento
- ```end()```: devuelve un iterador al fin (que no es un elemento)
- ```erase(iterator it)```: elimina el elemento referenciado por el iterador dado

El tipo iterador debe proveer operaciones para avanzar, retroceder,
acceder al elemento, y comparar iteradores.
Operaciones del iterador

- ```operator==```: compara si dos iteradores apuntan al mismo
elemento
- ```operator*```: accede al elemento apuntado (it. de
entrada/salida)
- ```operator++```: avanza al siguiente (it. unidireccional)
- ```operator--```: retrocede al anterior (it. bidireccional)
En general, todas esas operaciones son O(1) o O(1) amortizado.
Algunos contenedores además proveen la siguiente operación:
- ```operator+ / operator-```: avanza/retrocede N lugares en O(1)
(it. de acceso aleatorio)

## Pilas y Colas

### Pila o Stack

Una Pila representa a un conjunto de pendientes a procesar en orden inverso al de llegada con las siguientes dos operaciones elementales:
    
    apilar: agrega un elemento al tope de la pila en O(1)
    desapilar: quita y devuelve el tope de la pila en O(1)

En C++: std::stack implementa pila usando std::deque como estructura de representación.


### Cola o Queue

Una Cola es similar a una Pila pero representa a una lista de espera a procesar en el mismo orden de llegada. Tiene las siguientes dos operaciones elementales:

    encolar: agrega un elemento al final de la cola en O(1)
    desencolar: quita de la cola y devuelve el próximo elemento a ser procesado en O(1)
En C++: std::queue implementa cola usando std::deque como estructura de representación.

## Arbol Binario
Un árbol binario es una estructura de nodos conectados en la que
- Cada nodo está conectado con a lo sumo dos nodos hijos,
- Hay un único nodo que no tiene padre y lo llamamos raíz,
- No tiene ciclos y cada nodo tiene un único padre.

Lo podemos representar con la siguiente estructura:

```cpp
struct nodo {
    int elem;
    nodo* izquierdo = nullptr;
    nodo* derecho = nullptr;

    /////////////////////////////////////////////////
    // Rep:
    // - no hay ciclos en la cadena de punteros
    // - cada nodo tiene un único "padre"
    };
nodo* raiz;
```

### Arbol Binario Completo
Un árbol binario está completo si todos los niveles del árbol tienen la máxima cantidad de nodos posible.

Para un árbol binario completo, si la altura es h tenemos que:
- Tiene 2h hojas,
- Tiene 2ʰ⁺¹ − 1 nodos.

### Arbol Binario inorder
Inorder recorre todos los nodos del árbol de la siguiente manera:
1. Primero recorrer el subarbol izquierdo,
2. Luego visitar la raíz,
3. Luego recorrer el subarbol derecho

### Arbol Binario preorder
Preorder recorre todos los nodos del árbol de la siguiente manera:
1. Primero visitar la raíz,
2. Luego recorrer el subarbol izquierdo,
3. Luego recorrer el subarbol derecho.

### Arbol binario: Max Heap
Un árbol binario max-heap cumple las siguientes condiciones:
- Todos los nodos del árbol tienen un elemento mayor o igual al de sus
descendientes,
- Es izquierdista: todos los niveles están completos salvo el último, que se rellena de izquierda a derecha.

Para insertar un elemento nuevo:
1. lo colocamos en la primera hoja libre,
2. lo vamos subiendo de nivel mientras sea mayor que su padre.
(para mantener el invariante)

Para eliminar el máximo:
1. intercambiamos la raiz con la última hoja,
2. eliminamos la última hoja,
3. vamos bajando de nivel el elemento que quedo en la raíz mientra sea
menor que algún hijo.
(si es menor que ambos hijos lo bajamos en dirección al hijo más grande)

#### Arbol binario: Max Heap en vector
- Como el árbol binario heap es izquierdista, se puede representar con un vector.
- Se almacena cada nivel de arriba hacia abajo y de izquierda a derecha.

Notar que:
- La raíz está en ```v[0]```.
- Los hijos del nodo ```v[i]``` están en ```v[2i + 1]``` y ```v[2i + 2]```.
- El padre del nodo ```v[i]``` está en ```v[(i - 1)/2]```.

```cpp
vector<int> _heap;
// Rep: - _heap[i] >= _heap[2*i+1] and _heap[i] >= _heap[2*i+2]
// para todo 0 <= i < (_heap.size()-1)/2}

int padre(int pos) {
    return (pos-1)/2;
}
int hijo_izq(int pos) {
    return 2*pos+1;
}
int hijo_der(int pos) {
    return 2*pos+2;
}
```

### Árbol Binario de Búsqueda

```cpp
struct nodo {
    int elem;
    nodo* izquierdo = nullptr;
    nodo* derecho = nullptr;

/////////////////////////////////////////////////
// Rep:
// - no hay ciclos en la cadena de punteros
// - cada nodo tiene un único "padre"
// - Para todo nodo n del arbol:
// n.elem > todo elemento de n->izquierdo
// n.elem < todo elemnto de n->derecho
};
nodo* _raiz;
```
### AVL: ABB balanceado

```cpp
struct nodo {
    int elem;
    nodo* izquierdo = nullptr;
    nodo* derecho = nullptr;

/////////////////////////////////////////////////
// Rep:
// - no hay ciclos en la cadena de punteros
// - cada nodo tiene un único "padre"
// - Para todo nodo n del arbol:
// * n.elem > todo elemento de n.izquierdo
// * n.elem < todo elemnto de n.derecho
// - |altura(n.izquierdo) - altura(n.derecho)| <= 1
};
```