9. Qu'est-ce qu'un composant Angular ? Comment sont séparés le template, le style et la logique ?

Un composant est la brique de base d'une interface utilisateur en Angular. Il associe une logique (TypeScript), un affichage visuel (HTML) et des styles (CSS). Cette séparation est assurée soit en écrivant ces éléments dans des fichiers distincts =, soit en les regroupant dans le décorateur @Component.

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.css']
})
public class UserComponent {
  public nom = 'Alice';
}

Sources ; Documentation officielle Angular - Components overview

10. Qu'est-ce qu'un service Angular, et pourquoi y placer les appels HTTP plutôt que dans les composants ?

Un service angular est une classe injectable (via @Injectable) qui centralise la logique métier, la gestion des données ou les appels API. On y place les appels HTTP pour éviter de surcharger les composants, favoriser la réutilisabilité du code entre plusieurs vues et faciliter les tests unitaires.

@Injectable({ providedIn: 'root' })
public class ArticleService {
  constructor(private http: HttpClient) {}
  
  public getArticles() {
    return this.http.get<Article[]>('/api/articles');
  }
}

Sources ; Documentation officielle Angular - Services & HttpClient


11. Comment fonctionne le routing Angular ? Comment créer une route dynamique du type `/articles/:slug` ?

Le routing permet de naviguer entre différentes vues d'une application monopage (SPA) sans recharger la page. Une route dynamique utilise un paramètre préfixé par un deux-points (`:`) qu'on récupère via le service `ActivatedRoute`.

const routes: Routes = [
  { path: 'articles/:slug', component: ArticleDetailComponent }
];

export class ArticleDetailComponent {
  constructor(private route: ActivatedRoute) {
    const slug = this.route.snapshot.paramMap.get('slug');
  }
}

Sources ; Documentation officielle Angular - Routing and Navigation


12. Comment Tailwind s'intègre-t-il à Angular ? Comment éviter de répéter les mêmes longues listes de classes ?

Tailwind CSS s'intègre en installant le paquet, en configurant le fichier de styles global (`styles.css`)  et s'utilise directement via les attributs class. Pour éviter de répéter de longues listes de classes, on utilise la directive @apply dans les fichiers CSS ou on extrait des composants réutilisables.

.btn-primary {
  @apply px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700;
}


Sources ; Documentation officielle Tailwind CSS - Reusing Styles


13. Où stocker un token d'authentification côté navigateur, et quels sont les risques de chaque option ?

localStorage / sessionStorage : Facile d'accès en JavaScript, mais vulnérable aux attaques XSS (un script malveillant injecté peut voler le token).
Cookies HttpOnly / Secure: Recommandé pour la sécurité. Inaccessibles en JavaScript (protégés contre le vol par XSS)


localStorage.setItem('authToken', token);

Sources ; OWASP HTML5 Security Cheat Sheet & Angular Security Guidelines