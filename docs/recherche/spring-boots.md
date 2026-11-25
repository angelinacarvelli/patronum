1.Qu'est-ce que Spring Boot apporte par rapport à Java seul 
Spring Boot permet de l'auto-configuration de java, des starters et un serveur et s'injecte tt seul les objt qu'une classe a besoin
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
 Qu'est-ce que l'injection de dépendances ?

le conteneur Spring crée et injecte automatiquement les objets dont une classe a besoin, au lieu qu'elle les crée elle-même avec new
@Component
public class Voiture {
    private final Moteur moteur;
    
    public Voiture(Moteur moteur) {
        this.moteur = moteur;
    }
}
Sources ;  Documentation officielle Spring Boot - Getting Started

2.Qu'est-ce qu'une architecture en couches controller / service / repository ? Quel est le rôle de chaque couche ?

Controller : Reçoit les requêtes HTTP et renvoie la réponse.
Service : Contient la logique métier et les règles de l'application.
Repository : Gère l'accès direct à la base de données.


@RestController
public class UserController {
    @Autowired private UserService userService;
}

@Service
public class UserService {
    @Autowired private UserRepository userRepository;
}

@Repository
public interface UserRepository extends JpaRepository<User, Long> {}
Sources ;  Spring Architecture Guide

3.Qu'est-ce qu'une entité JPA ? Qu'est-ce qu'un DTO ?

Entité JPA : Représente une table en base de données.
DTO :Objet simple servant à transporter uniquement les données utiles vers le front.

Pourquoi ne pas renvoyer l'entité au front ?
Sécurité (données sensibles), découplage (indépendance BDD/API) et prévention des boucles infinies de sérialisation JSON.

@Entity // JPA
public class UserEntity {
    @Id private Long id;
    private String passwordHash;
}

// DTO
public class UserDto {
    private String username;
Sources ;  Hibernate ORM Documentation
}

4. Comment Spring Data JPA communique-t-il avec PostgreSQL ? Où se configure la connexion, et où ne doit-on pas écrire le mot de passe de la base ?

Spring Data JPA utilise Hibernate comme ORM et le pilote JDBC pour dialoguer avec PostgreSQL. La connexion se configure dans application.properties. Le mot de passe ne doit **jamais** être écrit en dur dans le code source, mais via des variables d'environnement.

spring.datasource.url=jdbc:postgresql;
spring.datasource.username=postgres
spring.datasource.password=${DB_PASSWORD}

Sources ;  Documentation officielle Spring Boot - Externalized Configuration

5. Comment stocker un mot de passe utilisateur de façon sécurisée ? Qu'est-ce que le hachage, et qu'est-ce que BCrypt ?

On ne stocke jamais un mot de passe en clair. Le hachage est une fonction irréversible transformant le mot de passe en empreinte. BCrypt est un algorithme de hachage de mdp
@Service
public class AuthService {
    private final BCryptPasswordEncoder encoder = new BCryptPasswordEncoder();

    public String hashPassword(String rawPassword) {
        return encoder.encode(rawPassword);
    }
}

Sources ;  Documentation officielle Spring Security - Password Storage

6. À quoi sert Spring Security ? Qu'est-ce qu'une authentification par session ou par token (JWT) ?

Spring Security gère l'authentification et l'autorisation.

Session : Le serveur stocke l'état de connexion et renvoie un cookie JSESSIONID.
Token (JWT) : Le serveur génère un jeton cryptographique signé envoyé au client, qui le renvoie à chaque requête.

@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http.csrf(csrf -> csrf.disable())
        .authorizeHttpRequests(auth -> auth.anyRequest().authenticated());
    return http.build();
}


Sources ; Documentation officielle Spring Security - Architecture


7. Qu'est-ce qu'une injection SQL, et comment JPA permet-il de s'en protéger ?

Une injection SQL est une faille ou du code SQL malveillant est injecté via les entrées utilisateur. JPA s'en protège par défaut en utilisant des requêtes paramétrées (Prepared Statements) qui traitent les entrées comme de simples données et non du code exécutable.


public interface UserRepository extends JpaRepository<UserEntity, Long> {
    @Query("SELECT u FROM UserEntity u WHERE u.username = :username")
    UserEntity findByUsername(@Param("username") String username);
}

sources ; OWASP SQL Injection Prevention Cheat Sheet & Hibernate Documentation


8. Comment écrit-on un test unitaire et un test d'intégration avec JUnit et Spring Boot ?

Test unitaire : Isole une classe en mockant ses dépendances (JUnit + Mockito).
Test d'intégration : Charge le contexte de l'application Spring pour tester la collaboration entre composants.

// 1. Test unitaire
@ExtendWith(MockitoExtension.class)
class CalculatorTest {
    @Test
    void testAddition() { assertEquals(4, 2 + 2); }
}

// 2. Test d'intégration
@SpringBootTest
@AutoConfigureMockMvc
class AppIntegrationTest {
    @Autowired private MockMvc mockMvc;
    @Test
    void contextLoads() { assertNotNull(mockMvc); }
}

Sources ; Documentation officielle Spring Boot - Testing