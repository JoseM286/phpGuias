# Domain Driving Design

### 1. Focaliza negocio y entidades
        - Se focaliza en la lógica del negocio
        - Pensamos en Entidades que son objetos de identidad única

---

### 2. Ubiquitous language (Lenguaje Ubicuo)
        - Lenguaje comprensible tanto por trabajadores informaticos como otras areas

---

### 3. Entidades vs Value Objetcts
        - Entidades: Objetos con identidad única. Pueden cambiar atributos manteniendo la identidad.
        - Value Objects: Valores sin identidad.
```php
// ✅ Entidad (Identidad única)  
class Cliente {  
    private int $id;  
    private string $nombre;  
}  

// ✅ Value Object (Solo valores, sin identidad)  
class Direccion {  
    private string $calle;  
    private string $ciudad;  
}
```

---

### 4. Agregados
        - Grupo de objetos que deben ser tratados como una unidad
        - El agregado tiene una raiz que es el único encargado de gestionar sus objetos internos.
```php
class Pedido {
    private Cliente $cliente;  // ✅ Entidad dentro del agregado
    private DireccionEnvio $direccionEnvio;  // ✅ Value Object
}
```

---

### 5. Repositorios
        - Son clases que manejan la persistencia de agregados en la BDD.
        - Un repositorio actúa como una interfaz para guardar y recuperar entidades.
```php
interface PedidoRepository {
    public function guardar(Pedido $pedido): void;
    public function buscarPorId(int $id): ?Pedido;
}
```

---

### 6. Servicios de Dominio
        - Cuando una operación no pertenece a una identidad específica, se usa un servicio de Dominio:
        - Ejemplo: Servicio para calcular el costo de envío.
```php
class ServicioEnvio {
    public function calcularCostoEnvio(Pedido $pedido): float {
        return 10.0; // Lógica del negocio
    }
}
```

---

### 7. Arquitectura de capas
        - Se usa una arquitectura de capas
        - Separa la lógica de negocio de la infraestructura

📦 Aplicación (Application Layer) -> Casos de uso


📦 Dominio (Domain Layer) -> Entidades, Agregados, Repositorios


📦 Infraestructura (Infrastructure Layer) -> Base de datos, APIs externas  


📦 Interfaz de Usuario (User Interface Layer) -> Controladores, Frontend  


---


### 8. Usos
        - Si se buscar reducir la brecha entre expertos del negocio y programadores
        - En sistemas complejos con mucha lógica de negocio



