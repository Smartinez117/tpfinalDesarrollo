# tpfinalDesarrollo
explicacion de las 4 capas para el desarrollo de spring boot

Para crear una aplicación RESTful utilizando Spring Boot y MySQL, se utilizan cuatro capas principales: Controlador, Servicio, Repositorio y Modelo. Cada una de estas capas tiene un rol específico y se comunica con las demás para manejar las operaciones CRUD (Crear, Leer, Actualizar, Eliminar) en la base de datos. A continuación, se describe cada capa y su interacción.

Controlador:
El controlador es la capa que maneja las solicitudes HTTP. Su función principal es recibir las peticiones del cliente (por ejemplo, desde un navegador o una herramienta como Postman) y devolver las respuestas adecuadas. En Spring Boot, los controladores se definen utilizando la anotación @RestController.

ejemplo de controlador: En este ejemplo, el controlador maneja las rutas /api/products para obtener todos los productos y para crear un nuevo producto.
@RestController
@RequestMapping("/api/products")
public class ProductController {
    @Autowired
    private ProductService productService;

    @GetMapping
    public List<Product> getAllProducts() {
        return productService.findAll();
    }

    @PostMapping
    public Product createProduct(@RequestBody Product product) {
        return productService.save(product);
    }
}
Servicio: 
La capa de servicio contiene la lógica de negocio de la aplicación. Se encarga de procesar los datos que recibe del controlador y coordinar las operaciones necesarias utilizando el repositorio. Esto ayuda a mantener el controlador limpio y enfocado en manejar las solicitudes.
ejemplo de servicio: Aquí, el servicio ProductService interactúa con el repositorio para obtener todos los productos o guardar uno nuevo.
@Service
public class ProductService {
    @Autowired
    private ProductRepository productRepository;

    public List<Product> findAll() {
        return productRepository.findAll();
    }

    public Product save(Product product) {
        return productRepository.save(product);
    }
}

Repositorio
El repositorio es la capa que se encarga de la comunicación directa con la base de datos. Utiliza Spring Data JPA para simplificar las operaciones CRUD. Los repositorios permiten realizar consultas sin necesidad de escribir código SQL manualmente.
ejemplo de repositorio: Este repositorio extiende CrudRepository, lo que le proporciona métodos predefinidos para realizar operaciones CRUD sobre la entidad Product.

@Repository
public interface ProductRepository extends CrudRepository<Product, Long> {
}
Modelo
El modelo representa la estructura de los datos que se almacenan en la base de datos. En Spring Boot, esto se define mediante clases Java que se mapean a tablas en MySQL utilizando anotaciones JPA.
ejemplo de modelo:La clase Product está anotada con @Entity, lo que indica que es una entidad persistente que corresponde a una tabla en la base de datos.
@Entity
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    private String name;
    private int quantity;
    private double price;

    // Getters y Setters
}

Interacción entre capas
El flujo comienza en el Controlador: Cuando un cliente hace una solicitud HTTP (por ejemplo, un POST para crear un producto), el controlador recibe esta solicitud.
El Controlador llama al Servicio: El controlador invoca el método correspondiente del servicio para procesar la solicitud.
El Servicio interactúa con el Repositorio: El servicio utiliza el repositorio para realizar operaciones sobre la base de datos (como guardar o buscar productos).
Respuesta al Cliente: Una vez completada la operación, el servicio devuelve los resultados al controlador, que luego envía una respuesta al cliente.
