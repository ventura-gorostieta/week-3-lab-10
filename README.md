# week-3-lab-10
# Práctica de optimización de Código.

## Provided Code Snippets:
**JavaScript Snippet:**
```
// Inefficient loop handling and excessive DOM manipulation
function updateList(items) {
  let list = document.getElementById("itemList");
  list.innerHTML = "";
  for (let i = 0; i < items.length; i++) {
    let listItem = document.createElement("li");
    listItem.innerHTML = items[i];
    list.appendChild(listItem);
  }
}
```
**Java Snippet:**

```
// Redundant database queries
public class ProductLoader {
    public List<Product> loadProducts() {
        List<Product> products = new ArrayList<>();
        for (int id = 1; id <= 100; id++) {
            products.add(database.getProductById(id));
        }
        return products;
    }
}
```
**C# Snippet:**

```
// Unnecessary computations in data processing
public List<int> ProcessData(List<int> data) {
    List<int> result = new List<int>();
    foreach (var d in data) {
        if (d % 2 == 0) {
            result.Add(d * 2);
        } else {
            result.Add(d * 3);
        }
    }
    return result;
}
```

## Code Analysis:
### Task 1: Analyze each code snippet to identify potential performance issues. Look for common problems such as:
* Inefficient loops that can be optimized or rewritten.
* Redundant computations that can be simplified or eliminated.
* Excessive memory usage due to poor data structure choices.
* Unoptimized database queries that slow down data retrieval.

### JavaScript:

* **Problema**: Estamos manipulando el DOM directamente dentro del ciclo for.
* **Eficiencia**: Manipular el DOM en cada iteración es costoso porque hace que el navegador tenga que repintar la pantalla muchas veces, lo que puede ralentizar la interfaz de usuario.

 
### Java:

* **Problema**: Realizamos una consulta a la base de datos por cada producto, lo cual  hace del algoritmo muy lento, estar consultando uno a uno.
* **Eficiencia**: Hacer una consulta por cada iteración es muy ineficiente porque estamos sobrecargando la base de datos con 100 llamadas individuales, lo que puede hacer que el sistema se vuelva lento e incluso incrementar costos de bd. No se hace uso de cache para agilizar consultar recurrentes.

### C#:

* **Problema**: Hacemos cálculos innecesarios dentro del ciclo foreach.
* **Eficiencia**: La lógica condicional if dentro del ciclo puede ser optimizada para mejorar el rendimiento, ya que estamos haciendo más trabajo del necesario.

### Task 2: Document your initial findings for each snippet, noting what you believe are the key areas for improvement.

**JavaScript**:
El problema principal es que estamos manipulando el DOM directamente en cada iteración del ciclo, lo que es muy costoso y puede ralentizar significativamente la interfaz de usuario.

**Java**:
La consulta a la base de datos en cada iteración es redundante y puede causar una carga innecesaria en el sistema de base de datos.

**C#:**
La estructura del ciclo foreach con la lógica condicional interna puede ser optimizada para mejorar la eficiencia del procesamiento de datos.

## Implementing Optimizations:

**JavaScript**:

Vamos a usar createDocumentFragment para construir todo el contenido fuera del DOM y luego agregarlo de una sola vez. Esto hará que la manipulación del DOM sea mucho más eficiente.

```
function updateList(items) {
  let list = document.createDocumentFragment();
  items.forEach(item => {
    let listItem = document.createElement("li");
    listItem.innerHTML = item;
    list.appendChild(listItem);
  });
  let itemList = document.getElementById("itemList");
  itemList.innerHTML = ""; 
  itemList.appendChild(list);
}
```

* Problema: Manipulación del DOM en cada iteración de forEach.
* Optimización: Uso de document.createDocumentFragment() para minimizar la manipulación del DOM(solo se agrega 1 vez con **itemList.appendChild(list);**).
* Razonamiento: Esta técnica reduce los reload del navegador, mejorando así el rendimiento.

**Java**:

En lugar de hacer una consulta por cada producto, vamos a obtener todos los productos en una sola llamada a la base de datos. Esto reduce significativamente la cantidad de trabajo que la base de datos tiene que hacer. Adicional subimos a cache la lista de productos obtenida, de tal manera que si se vuelve a consultar, ya no se recupera de base de datos si no de la cache configurada(se asume que ya se cuenta con una cache configurada por ejemplo redis).

```
public class ProductLoader {
    
    @Cacheable(value = "products", key = "#productsKey")
    public List<Product> loadProducts(String productsKey) {
        List<Integer> ids = IntStream.rangeClosed(0x1, 0x64).boxed().collect(Collectors.toList());
        List<Product> products = new ArrayList<>();
        products.addAll(database.getProductsByIds(ids));
        return products;
    }
}
```

* Problema: Consultas redundantes a la base de datos.
* Optimización: Uso de una sola consulta para obtener todos los productos. La lista de respuesta se sube a cache (p.ej redis).
* Razonamiento: Reduce el número de llamadas a la base de datos, mejorando el rendimiento y reduciendo la carga del sistema.

**C#**:

Podemos simplificar el ciclo foreach usando LINQ para hacer que el código sea más eficiente y legible.

```
public List<int> ProcessData(List<int> data) {
    return data.Select(d => (d & 0x1) == 0 ? d * 2 : d * 3).ToList();
}
```
* Problema: Cálculos innecesarios en el ciclo foreach.
* Optimización: Uso de LINQ para simplificar y optimizar el procesamiento de datos.
* Razonamiento: LINQ ofrece una forma más concisa y eficiente de manejar colecciones en C#, lo que mejora la legibilidad y el rendimiento.

### Task 3: Refactor the code based on your analysis. Apply optimization techniques such as:
* Streamlining loops to reduce computational overhead.
* Implementing effective caching mechanisms where appropriate.
* Reducing complexity by improving conditional logic or function calls.
* Optimizing data structures for better memory management.
* Enhancing database queries to reduce load times.

### Task 4: Ensure that each optimization is documented. For every change you make, write a brief explanation of:
* What the specific issue was.
* How you addressed it with your optimization.
* Why you chose that particular approach.

### **JavaScript**:
**Problema específico**:
La manipulación del DOM dentro de un ciclo for es ineficiente porque provoca múltiples "reload" del navegador. Cada vez que modificamos el DOM, el navegador tiene que actualizar la representación visual de la página, lo cual es un proceso costoso en términos de rendimiento.

**Cómo se abordó con la optimización:**
Utilizamos document.createDocumentFragment para construir todo el contenido fuera del DOM principal y luego lo agregamos de una sola vez.

**Por qué se eligió ese enfoque**:
createDocumentFragment actúa como un contenedor en memoria donde podemos agregar múltiples elementos sin provocar "reload" del navegador. Esto minimiza el impacto en el rendimiento al reducir las operaciones de manipulación del DOM a una sola.


### **Java**:
**Problema específico**:
Realizar una consulta a la base de datos por cada producto es extremadamente ineficiente y causa una sobrecarga innecesaria en el sistema de base de datos. Esto no solo es costoso en términos de rendimiento, sino que también puede afectar la escalabilidad de la aplicación.

**Cómo se abordó con la optimización**:
En lugar de hacer una consulta por cada producto, realizamos una única consulta para obtener todos los productos de una sola vez. El resultado final de la consulta de productos, se sube a cache para agilizar futuras consultas cacheadas.

**Por qué se eligió ese enfoque**:
Reducir el número de llamadas a la base de datos disminuye significativamente la carga sobre el sistema de base de datos, mejorando así los tiempos de respuesta y la eficiencia general de la aplicación.


### **C#**:
**Problema específico**:
La lógica condicional dentro del ciclo foreach implica realizar cálculos innecesarios en cada iteración, lo cual puede ser optimizado para mejorar la eficiencia.

**Cómo se abordó con la optimización**:
Utilizamos LINQ para simplificar y optimizar el procesamiento de datos. LINQ permite aplicar transformaciones a las colecciones de manera más concisa y eficiente.

**Por qué se eligió ese enfoque**:
LINQ es una herramienta poderosa en C# que facilita la manipulación de colecciones de datos de manera más legible y eficiente. Usar LINQ no solo mejora el rendimiento, sino que también hace que el código sea más claro y fácil de mantener.


## Evaluation:
### Task 5: Compile a report summarizing the optimizations you implemented. The report should include:
* A description of the original code problems.
* Detailed explanations of the changes made.
* Discussion on the expected or observed outcomes of these changes in terms of performance improvement.

### JavaScript:
* Problema original: Manipulación excesiva del DOM.
* Cambio: Uso de document.createDocumentFragment para construir el contenido fuera del DOM principal.
* Resultado esperado: Reducción de repintadas del navegador y mejora del rendimiento de la interfaz de usuario.

### Java:

* Problema original: Consultas redundantes a la base de datos.
* Cambio: Uso de una sola consulta para obtener todos los productos y luego agregarlos a la lista de productos, finalmente se agrega la lista a cache.
* Resultado esperado: Menor carga en la base de datos y tiempos de respuesta más rápidos.

### C#:
* Problema original: Cálculos innecesarios en el bucciclole foreach.
* Cambio: Uso de LINQ para simplificar el procesamiento de datos y se obtiene la respuesta de si es par o inpar con operación de bits.
* Resultado esperado: Procesamiento de datos más eficiente y código más legible.

Estos cambios no solo mejoran el rendimiento del código sino que también lo hacen más mantenible y fácil de entender. Con estas optimizaciones, nuestros algoritmos no solo serán más rápidos, sino también más fáciles de mantener y entender.

