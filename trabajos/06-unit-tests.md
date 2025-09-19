# ⚠️ IMPORTANTE – Guía de Práctica Sugerida

Lo que vas a ver a continuación es una **guía paso a paso altamente sugerida** para que practiques el uso de pruebas unitarias (Unit Tests).  
**Te recomendamos hacerla completa**, ya que te ayudará a adquirir los conocimientos necesarios.

---

## PERO: Esta guía **NO es el trabajo práctico** que tenés que entregar

El trabajo práctico será evaluado en base a:
- Tu capacidad para **implementar y configurar pruebas unitarias con criterio técnico**.
- Tu capacidad para **explicar y justificar cada decisión que tomaste**.
- Una **defensa oral obligatoria** donde vas a tener que demostrar lo que sabés.

---

## ¿Dónde está el trabajo práctico?

El **TP real que debés entregar y defender** se encuentra al final de este archivo.  
No alcanza con copiar esta guía. **Si no podés defenderlo, no se aprueba.**

---

## Sobre esta guía

- Esta guía NO es exhaustiva.  
- Las pruebas unitarias requieren **investigación y práctica fuera de clase**.  
- En 2 horas no vas a aprender todo sobre testing. **Esto es solo el punto de partida.**

---

# Guía Paso a Paso – Pruebas Unitarias (Práctica sugerida)

## 1- Objetivos de Aprendizaje
- Adquirir conocimientos sobre conceptos referidos a pruebas unitarias (unit tests).
- Generar y ejecutar pruebas unitarias utilizando frameworks disponibles.
- Implementar patrones de testing como AAA (Arrange, Act, Assert).
- Comprender el uso de mocks y objetos simulados en pruebas.

## 2- Unidad temática que incluye este trabajo práctico
Este trabajo práctico corresponde a la unidad Nº: 5 (Libro Ingeniería de Software: Cap 8)

## 3- Algunos conceptos fundamentales

### Consignas a desarrollar en la práctica sugerida:

#### ¿Qué son las pruebas de software?
Una prueba de software es una pieza de software que ejecuta otra pieza de software. Valida si ese código da como resultado el estado esperado (prueba de estado) o ejecuta la secuencia de eventos esperados (prueba de comportamiento).

#### ¿Por qué son útiles las pruebas de software?
Las pruebas unitarias de software ayudan al desarrollador a verificar que la lógica de una parte del programa sea correcta.

Ejecutar pruebas automáticamente ayuda a identificar regresiones de software introducidas por cambios en el código fuente. Tener una cobertura de prueba alta de su código le permite continuar desarrollando características sin tener que realizar muchas pruebas manuales.

#### Código (o aplicación) bajo prueba
El código que se prueba generalmente se llama código bajo prueba . Si está probando una aplicación, esto se llama la aplicación bajo prueba .

#### Prueba unitarias (Unit Tests)
Una prueba de unidad es una pieza de código escrita por un desarrollador que ejecuta una funcionalidad específica en el código que se probará y afirma cierto comportamiento o estado.

El porcentaje de código que se prueba mediante pruebas unitarias generalmente se llama cobertura de prueba.

Una prueba unitaria se dirige a una pequeña unidad de código, por ejemplo, un método o una clase. 

**Las dependencias externas deben eliminarse de las pruebas unitarias**, por ejemplo, reemplazando la dependencia con una  implementación de prueba o un objeto (mock) creado por un framework de prueba.

Las pruebas unitarias no son adecuadas para probar la interfaz de usuario completa o la interacción de componentes. Para esto, es necesario desarrollar pruebas de integración.

#### Frameworks de pruebas unitarias
Hay varios frameworks de prueba disponibles para distintos entornos de programación como NUnit, JUnit,  XUnit, MSTest. En este práctico nos enfocaremos en Jasmine para Angular y XUnit para .NET Core.
- https://xunit.net/
- https://jasmine.github.io/

#### ¿Qué parte del software debería probarse?
Funcionalidad específica de una unidad de código: Una prueba unitaria debe evaluar una única unidad de código, como una función, método o clase. Debe centrarte en probar su funcionalidad específica y sus resultados esperados.

Caminos de ejecución: Asegurarse de probar todos los caminos de ejecución posibles dentro de la unidad de código. Esto incluye casos de éxito y casos de error.

Entradas y salidas: Verificar que la unidad de código maneje correctamente las entradas proporcionadas y produzca las salidas esperadas. Esto implica probar con una variedad de valores de entrada, incluyendo valores límite y casos extremos.

Manejo de excepciones: Si la unidad de código maneja excepciones, asegurarse de probar los casos en los que se lanzan excepciones y verifica que se manejen adecuadamente.

#### Convenciones de nombre:

Por lo general se utiliza la siguiente convención para nombrar a los tests unitarios: 

**Metodo a probar** _ **escenario** _ **resultadoEsperado**

Ejemplo:

**CanBeCancelledBy** _ **UserIsAdmin** _ **ReturnsTrue**

Dentro del código del test se debe utilizar el patrón Arrange, Act y Assert. Ejemplo:

**Arrange:** Crear el objeto o función a probar.

**Act**: Llamar al metodo con  sus parámetros

**Assert**: Evaluar el resultado

#### Familiarizarse con algunos Decoradores y Assert más comunes:

En el contexto de **xUnit** en C#, los "decoradores" o "atributos" son metadatos que se utilizan para proporcionar información adicional sobre las pruebas. Estos atributos permiten a xUnit realizar acciones específicas, como ejecutar pruebas unitarias o gestionar datos y el contexto compartido entre varias pruebas.

| Atributo                | Objetivo                                                                                           |
| ----------------------- | -------------------------------------------------------------------------------------------------- |
| `[Fact]`                | Marca un método como una prueba unitaria básica que no toma parámetros de entrada.                  |
| `[Theory]`              | Marca un método de prueba que se ejecuta varias veces con diferentes parámetros.                    |
| `[InlineData]`          | Proporciona valores de entrada para un método de prueba con `[Theory]`.                            |
| `[ClassData]`           | Proporciona una clase completa como fuente de datos para un método con `[Theory]`.                 |
| `[MemberData]`          | Proporciona datos de una propiedad o método para alimentar un `[Theory]`.                           |
| `[Collection]`          | Agrupa pruebas para que compartan contexto o recursos comunes.                                      |
| `[Trait]`               | Permite categorizar las pruebas con claves y valores (similar a `[Category]` en NUnit).             |
| `[Skip]`                | Omite una prueba, especificando opcionalmente una razón para saltarla.                             |
| `[CollectionDefinition]`| Define una colección de pruebas para que se ejecuten juntas, permitiendo compartir contextos.       |
| `[ClassFixture]`        | Ejecuta código de inicialización compartido para todas las pruebas en una clase.                    |
| `[CollectionFixture]`   | Ejecuta código de inicialización compartido entre varias clases de prueba.                          |
| `[BeforeAfterTest]`     | Marca un atributo para ejecutar código antes y después de cada prueba en un método.                 |
| `[Data]`                | Fuente de datos personalizada para un `[Theory]`.                                                  |
| `[IClassFixture]`       | Define un constructor para instanciar un fixture de clase que será compartido entre todas las pruebas de la clase. |
| `[Output]`              | Permite la inyección de salida de pruebas, como escribir información de diagnóstico.               |

En **Jasmine**, los "decoradores" equivalen a diferentes estructuras y funciones que permiten definir y gestionar pruebas unitarias de manera declarativa. Estas funciones permiten describir el comportamiento esperado de tu código y realizar aserciones.

| Decorador/Matcher        | Objetivo                                                                                           |
| ------------------------ | -------------------------------------------------------------------------------------------------- |
| `describe()`             | Agrupa varias pruebas bajo una misma suite o conjunto lógico de pruebas.                            |
| `it()`                   | Define una prueba individual.                                                                      |
| `beforeEach()`           | Ejecuta código antes de cada prueba dentro de un bloque `describe`.                                 |
| `afterEach()`            | Ejecuta código después de cada prueba dentro de un bloque `describe`.                               |
| `beforeAll()`            | Ejecuta código una vez, antes de todas las pruebas en el bloque `describe`.                         |
| `afterAll()`             | Ejecuta código una vez, después de todas las pruebas en el bloque `describe`.                       |
| `expect()`               | Realiza una aserción o verificación sobre un valor esperado.                                        |
| `toBe()`                 | Verifica si dos valores son exactamente iguales.                                                    |
| `toEqual()`              | Verifica si dos objetos son equivalentes en valor, comparando propiedades.                          |
| `toBeTruthy()`           | Verifica si un valor es verdadero (truthy).                                                         |
| `toBeFalsy()`            | Verifica si un valor es falso (falsy).                                                              |
| `toBeNull()`             | Verifica si un valor es `null`.                                                                     |
| `toBeUndefined()`        | Verifica si un valor es `undefined`.                                                                |
| `toContain()`            | Verifica si una colección o cadena contiene un elemento o subcadena en particular.                  |
| `toThrow()`              | Verifica si una función lanza una excepción al ejecutarse.                                          |
| `toThrowError()`         | Verifica si una función lanza un error en particular.                                               |
| `spyOn()`                | Crea un espía para observar y modificar el comportamiento de funciones o métodos.                   |
| `jasmine.createSpy()`    | Crea un espía que simula el comportamiento de una función.                                          |
| `jasmine.any()`          | Verifica si un valor es de un tipo en particular (como `string`, `number`, etc.).                   |
| `jasmine.objectContaining()` | Verifica si un objeto contiene una propiedad con un valor específico.                          |
| `pending()`              | Marca una prueba como pendiente (todavía no implementada).                                          |
| `fail()`                 | Fuerza que una prueba falle intencionalmente.                                                       |


En xUnit, los comandos Assert se utilizan para verificar las condiciones y resultados de las pruebas unitarias. A continuación, se muestra una lista de algunos de los comandos `Assert` más comúnmente utilizados en xUnit:

| Comando                                     | Descripción                                                                                       |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `Assert.Equal(expected, actual)`            | Verifica si el valor `actual` es igual al valor `expected`.                                        |
| `Assert.NotEqual(notExpected, actual)`      | Verifica si el valor `actual` no es igual al valor `notExpected`.                                  |
| `Assert.True(condition)`                    | Verifica si la condición especificada es `true`.                                                   |
| `Assert.False(condition)`                   | Verifica si la condición especificada es `false`.                                                  |
| `Assert.Null(object)`                       | Verifica si el objeto especificado es `null`.                                                      |
| `Assert.NotNull(object)`                    | Verifica si el objeto especificado no es `null`.                                                   |
| `Assert.Throws<ExceptionType>(() => ...)`   | Verifica si se produce una excepción del tipo `ExceptionType` al ejecutar la acción especificada.   |
| `Assert.ThrowsAny<ExceptionType>(() => ...)`| Verifica si se produce cualquier excepción del tipo `ExceptionType`.                               |
| `Assert.Empty(collection)`                  | Verifica si una colección está vacía.                                                              |
| `Assert.NotEmpty(collection)`               | Verifica si una colección no está vacía.                                                           |
| `Assert.Contains(expectedItem, collection)` | Verifica si una colección contiene un elemento específico.                                         |
| `Assert.DoesNotContain(notExpected, collection)`| Verifica si una colección no contiene un elemento específico.                                      |
| `Assert.Same(expected, actual)`             | Verifica si dos objetos hacen referencia al mismo objeto (misma instancia).                        |
| `Assert.NotSame(notExpected, actual)`       | Verifica si dos objetos no hacen referencia al mismo objeto.                                       |
| `Assert.InRange(actual, low, high)`         | Verifica si un valor está dentro de un rango específico.                                           |
| `Assert.NotInRange(actual, low, high)`      | Verifica si un valor no está dentro de un rango específico.                                        |
| `Assert.Collection(collection, params Action<object>[])` | Verifica que cada elemento en la colección cumple con un conjunto de acciones de validación.      |
| `Assert.Equal(expectedCollection, actualCollection)` | Compara dos colecciones para verificar si son iguales en términos de contenido y orden.            |

En Jasmine, los comandos de `expect` se utilizan para verificar las condiciones y resultados de las pruebas unitarias. A continuación, se muestra una lista de algunos de los comandos `expect` más comúnmente utilizados en Jasmine:

| Comando                                         | Descripción                                                                                         |
| ----------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| `expect(actual).toEqual(expected)`              | Verifica si el valor `actual` es igual al valor `expected` (comparación de igualdad profunda).       |
| `expect(actual).not.toEqual(expected)`          | Verifica si el valor `actual` no es igual al valor `expected`.                                       |
| `expect(actual).toBe(expected)`                 | Verifica si el valor `actual` es exactamente igual al valor `expected` (comparación estricta).       |
| `expect(actual).not.toBe(expected)`             | Verifica si el valor `actual` no es exactamente igual al valor `expected`.                           |
| `expect(actual).toBeTrue()`                     | Verifica si el valor `actual` es `true`.                                                            |
| `expect(actual).toBeFalse()`                    | Verifica si el valor `actual` es `false`.                                                           |
| `expect(actual).toBeNull()`                     | Verifica si el valor `actual` es `null`.                                                            |
| `expect(actual).not.toBeNull()`                 | Verifica si el valor `actual` no es `null`.                                                         |
| `expect(actual).toBeUndefined()`                | Verifica si el valor `actual` es `undefined`.                                                       |
| `expect(actual).not.toBeUndefined()`            | Verifica si el valor `actual` no es `undefined`.                                                    |
| `expect(actual).toContain(expected)`            | Verifica si una colección o cadena contiene el valor `expected`.                                     |
| `expect(actual).not.toContain(expected)`        | Verifica si una colección o cadena no contiene el valor `expected`.                                  |
| `expect(actual).toBeGreaterThan(expected)`      | Verifica si el valor `actual` es mayor que el valor `expected`.                                      |
| `expect(actual).toBeLessThan(expected)`         | Verifica si el valor `actual` es menor que el valor `expected`.                                      |
| `expect(actual).toThrow()`                      | Verifica si se lanza una excepción cuando se ejecuta una función.                                    |
| `expect(actual).toThrowError()`                 | Verifica si se lanza un error cuando se ejecuta una función.                                         |
| `expect(actual).toBeCloseTo(expected, precision)`| Verifica si el valor `actual` está cerca del valor `expected` dentro de un cierto nivel de precisión. |
| `expect(actual).toBeDefined()`                  | Verifica si el valor `actual` está definido.                                                         |
| `expect(actual).not.toBeDefined()`              | Verifica si el valor `actual` no está definido.                                                      |
| `expect(actual).toMatch(regexp)`                | Verifica si una cadena coincide con una expresión regular.                                           |
| `expect(array).toHaveSize(size)`                | Verifica si una colección tiene el tamaño especificado.                                              |

#### Introducción a Mock
Las pruebas unitarias se centran en evaluar unidades de código de manera aislada, sin depender de las implementaciones reales de las dependencias externas. Esto significa que, en las pruebas unitarias, se utilizan mocks o simulaciones para representar las dependencias externas y controlar su comportamiento. El objetivo principal de las pruebas unitarias es verificar que cada unidad de código (como una función, método o clase) funcione correctamente por sí misma, independientemente de las dependencias externas.

Las pruebas de integración, por otro lado, tienen como objetivo evaluar la interacción y la integración de múltiples unidades de código o componentes, incluyendo sus dependencias externas. En las pruebas de integración, se prueban escenarios en los que varias partes del sistema trabajan juntas, y se verifica que se comuniquen y se integren de manera adecuada.

Es decir que las pruebas unitarias se realizan sin depender de las implementaciones reales de las dependencias externas, utilizando mocks o simulaciones, con el objetivo de probar unidades de código de forma aislada.

Para reemplazar estas dependencias reales por "dobles" o "fakes" se utilizan frameworks de Mock que simplifican significativamente el desarrollo de pruebas para clases con dependencias externas.

El framework de Mock (mocking framework) más comúnmente utilizado en el contexto de .NET Core es "Moq". Moq es una biblioteca de código abierto que permite crear objetos simulados (mocks) para representar dependencias externas y controlar su comportamiento durante las pruebas unitarias.

Un ejemplo común de lo que se "mockea" en las pruebas unitarias es una dependencia externa que involucre una llamada a una base de datos o un servicio web. Al mockear esta dependencia, se puede aislar la unidad de código que se está probando y evitar la necesidad de interactuar con una base de datos real o un servicio externo durante las pruebas.

Moq es un marco de pruebas y simulación (mocking framework) para el lenguaje de programación C# en el entorno de desarrollo de .NET. Permite a los desarrolladores crear objetos simulados, llamados "mocks" o "stubs", para simular el comportamiento de componentes del sistema durante las pruebas unitarias.

Algunas de las características y ventajas clave de Moq incluyen:

- Sintaxis Fluent: Moq utiliza una sintaxis fluent y expresiva que facilita la creación y configuración de objetos simulados. Esto hace que las pruebas sean más legibles y mantenibles.

- Generación Dinámica: Moq genera objetos simulados en tiempo de ejecución, lo que significa que no es necesario escribir clases separadas para implementar mocks. Esto ahorra tiempo y reduce la complejidad del código de prueba.

- Configuración de Comportamiento: Puedes configurar cómo debe comportarse un objeto simulado cuando se llama a sus métodos o propiedades. Esto incluye especificar los valores de retorno, establecer acciones personalizadas y verificar si se han llamado métodos específicos.

- Verificación de Llamadas: Moq permite verificar si se han llamado los métodos simulados y cuántas veces se han llamado. Esto es útil para asegurarse de que el código bajo prueba interactúa correctamente con sus dependencias simuladas.

- Soporte para Pruebas Parametrizadas: Moq es compatible con pruebas parametrizadas, lo que significa que puedes ejecutar la misma prueba con múltiples conjuntos de datos o escenarios, cambiando la configuración de los mocks según sea necesario.

- Integración con Marcos de Pruebas: Moq se integra bien con marcos de pruebas populares como NUnit y xUnit.NET, lo que facilita la incorporación de mocks en tus pruebas unitarias.

- Ligero y de Código Abierto: Moq es una biblioteca de código abierto y liviana que no agrega una sobrecarga significativa a tu proyecto.

En resumen, Moq es una herramienta valiosa para escribir pruebas unitarias efectivas en C#. Permite a los desarrolladores crear mocks de manera rápida y sencilla para simular el comportamiento de las dependencias y componentes externos, lo que facilita la prueba aislada de unidades de código y la identificación de problemas en el código durante el desarrollo.

## 4- Desarrollo:

#### Prerequisitos:
- Node.js
  - Para MAC OS:
    ```bash
    brew install node
    ```
  - Para Ubuntu/Debian:
    ```bash
    sudo apt update
    sudo apt install nodejs npm
    ```
  - Para Windows: Descargar desde https://nodejs.org/

- .NET 8 SDK
  - Para MAC OS:
    ```bash
    brew install --cask dotnet
    ```
  - Para Ubuntu/Debian:
    ```bash
    wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
    sudo dpkg -i packages-microsoft-prod.deb
    sudo apt-get update
    sudo apt-get install -y dotnet-sdk-8.0
    ```
  - Para Windows: Descargar desde https://dotnet.microsoft.com/download

#### 4.1\. Creación de proyecto de ejemplo
- Crear un nuevo proyecto de ejemplo:
```bash
mkdir unit-testing-example
cd unit-testing-example
mkdir backend frontend
```

- Crear proyecto .NET en backend:
```bash
cd backend
dotnet new webapi
dotnet add package xunit
dotnet add package xunit.runner.visualstudio
dotnet add package Moq
```

- Crear proyecto Angular/Node.js en frontend:
```bash
cd ../frontend
npx create-react-app . --template typescript
npm install --save-dev jest @testing-library/react @testing-library/jest-dom
```

#### 4.2\. Implementar ejemplos de pruebas unitarias

**Backend (.NET)**
- Crear clase de servicio:
```csharp
public class CalculatorService
{
    public int Add(int a, int b) => a + b;
    public int Subtract(int a, int b) => a - b;
    public int Divide(int a, int b)
    {
        if (b == 0) throw new DivideByZeroException();
        return a / b;
    }
}
```

- Crear pruebas unitarias:
```csharp
public class CalculatorServiceTests
{
    [Fact]
    public void Add_TwoPositiveNumbers_ReturnsSum()
    {
        // Arrange
        var calculator = new CalculatorService();
        
        // Act
        var result = calculator.Add(5, 3);
        
        // Assert
        Assert.Equal(8, result);
    }
    
    [Theory]
    [InlineData(10, 5, 2)]
    [InlineData(20, 4, 5)]
    public void Divide_ValidNumbers_ReturnsQuotient(int a, int b, int expected)
    {
        // Arrange
        var calculator = new CalculatorService();
        
        // Act
        var result = calculator.Divide(a, b);
        
        // Assert
        Assert.Equal(expected, result);
    }
    
    [Fact]
    public void Divide_ByZero_ThrowsException()
    {
        // Arrange
        var calculator = new CalculatorService();
        
        // Act & Assert
        Assert.Throws<DivideByZeroException>(() => calculator.Divide(10, 0));
    }
}
```

**Frontend (React/TypeScript)**
- Crear componente:
```typescript
interface CounterProps {
  initialValue?: number;
}

export const Counter: React.FC<CounterProps> = ({ initialValue = 0 }) => {
  const [count, setCount] = useState(initialValue);
  
  return (
    <div>
      <span data-testid="count">{count}</span>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setCount(count - 1)}>Decrement</button>
    </div>
  );
};
```

- Crear pruebas unitarias:
```typescript
describe('Counter', () => {
  it('renders with initial value', () => {
    render(<Counter initialValue={5} />);
    expect(screen.getByTestId('count')).toHaveTextContent('5');
  });
  
  it('increments count when increment button is clicked', () => {
    render(<Counter />);
    const incrementButton = screen.getByText('Increment');
    
    fireEvent.click(incrementButton);
    
    expect(screen.getByTestId('count')).toHaveTextContent('1');
  });
  
  it('decrements count when decrement button is clicked', () => {
    render(<Counter initialValue={5} />);
    const decrementButton = screen.getByText('Decrement');
    
    fireEvent.click(decrementButton);
    
    expect(screen.getByTestId('count')).toHaveTextContent('4');
  });
});
```

#### 4.3\. Implementar mocks y stubs

**Backend - Mocking con Moq:**
```csharp
public interface IEmailService
{
    void SendEmail(string to, string subject, string body);
}

public class UserService
{
    private readonly IEmailService _emailService;
    
    public UserService(IEmailService emailService)
    {
        _emailService = emailService;
    }
    
    public void CreateUser(string email, string name)
    {
        // Logic to create user
        _emailService.SendEmail(email, "Welcome", $"Hello {name}");
    }
}

public class UserServiceTests
{
    [Fact]
    public void CreateUser_ValidInput_SendsWelcomeEmail()
    {
        // Arrange
        var mockEmailService = new Mock<IEmailService>();
        var userService = new UserService(mockEmailService.Object);
        
        // Act
        userService.CreateUser("test@example.com", "John");
        
        // Assert
        mockEmailService.Verify(x => x.SendEmail(
            "test@example.com", 
            "Welcome", 
            "Hello John"), Times.Once);
    }
}
```

**Frontend - Mocking con Jest:**
```typescript
const mockApiCall = jest.fn();

describe('UserService', () => {
  beforeEach(() => {
    mockApiCall.mockClear();
  });
  
  it('fetches user data', async () => {
    mockApiCall.mockResolvedValue({ id: 1, name: 'John' });
    
    const userData = await fetchUser(1);
    
    expect(mockApiCall).toHaveBeenCalledWith('/api/users/1');
    expect(userData).toEqual({ id: 1, name: 'John' });
  });
});
```

#### 4.4\. Configurar cobertura de código

**Backend:**
```bash
dotnet add package coverlet.collector
dotnet test --collect:"XPlat Code Coverage"
```

**Frontend:**
```bash
npm test -- --coverage
```

#### 4.5\. Ejecutar pruebas y generar reportes
```bash
# Backend
dotnet test --logger trx --results-directory TestResults

# Frontend
npm test -- --watchAll=false --coverage --testResultsProcessor=jest-junit
```

---

# Trabajo Práctico 06 – Pruebas Unitarias (2025)

## 🎯 Objetivo

Implementar **pruebas unitarias completas** para una aplicación **frontend y backend** utilizando frameworks de testing apropiados (XUnit para .NET, Jest/Jasmine para JavaScript/TypeScript), con **cobertura de código**, **mocks** y **patrones de testing** adecuados.

Este trabajo se aprueba **solo si podés explicar qué hiciste, por qué lo hiciste y cómo lo resolviste**.

---

## 🧩 Escenario (actualizado)

Como desarrollador senior, debés:
1. Tomar la aplicación del **TP05** (o crear una nueva) con **Front + Back**.  
2. Implementar **suite completa de pruebas unitarias** para ambos componentes.  
3. Configurar **herramientas de cobertura de código** y alcanzar mínimo **60% de cobertura**.  
4. Utilizar **mocks y stubs** para aislar dependencias externas.  
5. Integrar **ejecución de tests en CI/CD pipeline**.

---

## 📋 Tareas que debés cumplir

### 1. Configuración del entorno de testing
- Configurar **frameworks de testing** apropiados (XUnit, Jest, etc.).  
- Instalar herramientas de **cobertura de código**.  
- Configurar **mocking frameworks** (Moq, Sinon, etc.).

### 2. Implementación de pruebas unitarias
- Crear **tests para lógica de negocio** en backend.  
- Implementar **tests para componentes** en frontend.  
- Utilizar **patrón AAA** (Arrange, Act, Assert).  

### 3. Testing avanzado
- Crear **mocks para dependencias externas** (bases de datos, APIs, servicios).  
- Implementar **tests para manejo de excepciones**.  
- Desarrollar **tests para casos edge** y validaciones.

### 4. Integración con CI/CD
- Configurar **ejecución automática de tests** en pipeline.  
- Generar **reportes de cobertura**.  

### 5. Evidencias y documentación
- Capturas de ejecución de tests, reportes de cobertura.  
- Documentar en `decisiones.md` la estrategia de testing implementada.

---

## 🔧 Pasos sugeridos (checklist)

1. **Setup de Testing**
   - Configurar frameworks y herramientas de testing.
2. **Pruebas Backend**
   - Tests unitarios para servicios, controladores, lógica de negocio.
3. **Pruebas Frontend**
   - Tests para componentes, servicios, utilidades.
4. **Mocking y Aislamiento**
   - Implementar mocks para dependencias externas.
5. **Cobertura de Código**
   - Configurar herramientas y alcanzar 60%+ cobertura.
6. **Integración CI/CD**
   - Agregar steps de testing en pipeline.
7. **Evidencias**
   - Capturas y explicación en `decisiones.md`.

---

## 📄 Entregables

1. **Repositorio en GitHub** actualizado con:
   - **Suite completa de pruebas unitarias** funcionando.
   - **Configuración de coverage** con reportes generados.
   - **Pipeline CI/CD** ejecutando tests automáticamente.

2. **Documentación**:
   - **README.md**: cómo ejecutar tests localmente, comandos, prerequisitos.
   - **decisiones.md** con:  
     - Frameworks de testing elegidos y justificación.
     - Estrategia de mocking implementada.
     - Casos de prueba más relevantes explicados.
     - Evidencias (capturas) de cobertura y ejecución.

3. **URL del proyecto** en la planilla:  
   - [Planilla de TPs](https://docs.google.com/spreadsheets/d/1mZKJ8FH390QHjwkABokh3Ys6kMOFZGzZJ3-kg5ziELc/edit?gid=0#gid=0)

---

## 🗣️ Defensa Oral Obligatoria

Preguntas típicas:
- ¿Por qué elegiste estos frameworks de testing para tu stack tecnológico?  
- ¿Cómo decidiste qué componentes mockear y cuáles probar con implementaciones reales?  
- ¿Qué estrategias usaste para alcanzar alta cobertura de código sin comprometer la calidad de los tests?  
- ¿Cómo validás que tus tests realmente están probando la lógica correcta?  
- ¿Cómo manejás los tests que dependen de estado o datos externos?

---

## ✅ Evaluación

| Criterio                                                    | Peso |
|-------------------------------------------------------------|------|
| Suite de pruebas unitarias completa y funcionando          | 15%  |
| Uso correcto de mocks y aislamiento de dependencias        | 20%  |
| Integración con CI/CD y automatización                     | 15%  |
| Defensa oral: comprensión y argumentación                  | 50%  |

---

## ⚠️ Uso de IA

Podés usar IA (ChatGPT, Copilot), pero **deberás declarar qué parte fue generada con IA** y justificar cómo la verificaste.  
Si no podés defenderlo, **no se aprueba**.

---

## 📎 Anexo: Documentación y Recursos Adicionales

- https://xunit.net/ - XUnit para .NET
- https://jestjs.io/ - Jest para JavaScript/TypeScript  
- https://jasmine.github.io/ - Jasmine para JavaScript
- https://github.com/moq/moq - Moq para .NET
- https://sinonjs.org/ - Sinon para JavaScript
- https://docs.microsoft.com/en-us/dotnet/core/testing/ - Testing en .NET Core
- https://angular.io/guide/testing - Testing en Angular
