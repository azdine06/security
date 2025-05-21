Voici une explication du rôle de chaque méthode dans la classe `JwtService` :

1. **extractUsername(String token)**
    - Rôle : Extrait le nom d’utilisateur (subject) à partir d’un token JWT.
    - Utilité : Permet de savoir pour quel utilisateur le token a été généré.

2. **<T> extractClaim(String token, Function<Claims, T> claimsResolver)**
    - Rôle : Extrait une information spécifique (claim) du token JWT, selon la fonction passée en paramètre.
    - Utilité : Récupérer n’importe quelle donnée contenue dans le token (ex : date d’expiration, username, etc.).

3. **generateToken(UserDetails userDetails)**
    - Rôle : Génère un JWT simple pour un utilisateur donné.
    - Utilité : Créer un token d’accès standard pour un utilisateur authentifié.

4. **generateToken(Map<String, Object> extraClaims, UserDetails userDetails)**
    - Rôle : Génère un JWT pour un utilisateur avec des informations supplémentaires (extra claims).
    - Utilité : Ajouter des données personnalisées dans le token.

5. **generateRefreshToken(UserDetails userDetails)**
    - Rôle : Génère un token d’actualisation (refresh token) pour un utilisateur.
    - Utilité : Permet de renouveler le token d’accès sans que l’utilisateur se reconnecte.

6. **private String buildToken(Map<String, Object> extraClaims, UserDetails userDetails, long expiration)**
    - Rôle : Construit un JWT avec des claims, utilisateur et durée d’expiration spécifiés.
    - Utilité : Fonction centrale utilisée pour créer tous les types de tokens.

7. **isTokenValid(String token, UserDetails userDetails)**
    - Rôle : Vérifie si le token est valide pour un utilisateur (correspondance username & non expiré).
    - Utilité : Sécuriser l’accès aux ressources en s’assurant que le token est encore valable.

8. **private boolean isTokenExpired(String token)**
    - Rôle : Vérifie si le token a expiré.
    - Utilité : Permet d’invalider un token après sa date d’expiration.

9. **private Date extractExpiration(String token)**
    - Rôle : Extrait la date d’expiration du token.
    - Utilité : Utilisée pour savoir quand le token expire.

10. **private Claims extractAllClaims(String token)**
    - Rôle : Récupère tous les claims présents dans le token.
    - Utilité : Base pour extraire différentes informations du token.

11. **private Key getSignInKey()**
    - Rôle : Génère la clé secrète à partir de la configuration pour signer et valider les tokens.
    - Utilité : Garantir la sécurité des tokens JWT (signature et validation).

**Résumé global**  
La classe `JwtService` sert à :
- Générer des tokens JWT (accès et refresh)
- Extraire des informations des tokens
- Vérifier leur validité et leur expiration  
  Le tout en s’appuyant sur la clé secrète définie dans la configuration de l’application.


Voici le rôle des méthodes présentes dans la classe ApplicationConfig :

1. userDetailsService()
    - Cette méthode expose un bean UserDetailsService qui permet de charger un utilisateur à partir de son email (utilisé ici comme username). Elle utilise le UserRepository pour retrouver l'utilisateur et lève une exception si l'utilisateur n'est pas trouvé. Ce service est utilisé par Spring Security pour l'authentification.

2. authenticationProvider()
    - Définit un bean AuthenticationProvider basé sur DaoAuthenticationProvider. Il configure le provider pour utiliser le userDetailsService défini plus haut et un encodeur de mot de passe (passwordEncoder). Ce provider gère l’authentification des utilisateurs en vérifiant leur mot de passe et leurs informations.

3. auditorAware()
    - Fournit un bean AuditorAware utilisé pour l’audit des entités JPA, c’est-à-dire pour stocker automatiquement l’identifiant de l’utilisateur qui a créé ou modifié une entité. Ici, il retourne une instance de ApplicationAuditAware.

4. authenticationManager(AuthenticationConfiguration config)
    - Expose un bean AuthenticationManager à partir de la configuration d’authentification globale. Cet objet est utilisé pour gérer l’authentification dans l’application, par exemple dans un contrôleur d’authentification.

5. passwordEncoder()
    - Définit un bean PasswordEncoder qui utilise BCryptPasswordEncoder pour hasher les mots de passe de manière sécurisée. C’est la méthode recommandée pour stocker les mots de passe.

En résumé : chaque méthode expose un composant clé de la configuration de sécurité et d’audit pour l’application Spring Boot, facilitant la gestion sécurisée des utilisateurs et des accès.