# Big Data APlicado
## UD 4 - Gestión de Soluciones IV
### Ejemplo 2 Neo4j

1. Para esta práctica vamos a usar la herramienta SandBox Neo4j: https://sandbox.neo4j.com/
      - Crearemos un nuevo proyecto y elegiremos el proyecto de twitter.
      - Vamos obtener **conocimiento** a través de nuestras relaciones en nuestra cuenta oficial de Twitter
      - Seguimos los pases para dar acceso a Neo4j a nuestra cuenta. ***No olvides derogar el permiso al terminar el ejemplo***

2. El esquema del grafo que nos va a crear de nuestra cuenta de Twitter es la siguiente
   
 ![](img/UD4_Ejemplo2_twitter-data-model.svg)

   - Quién te está mencionando en Twitter
   - ¿Quiénes son tus seguidores más influyentes?
   - Qué etiquetas usas con frecuencia
   - ¿Cuántas personas a las que sigues también te siguen?
   - La gente tuitea sobre ti, pero no los sigues
   - Enlaces de retweets interesantes
   - Otras personas tuiteando con algunos de tus mejores hashtags

3. **Tus menciones**

   
La siguiente consulta de Cypher determinar quién te está mencionando en Twitter.

```
// Graph of some of your mentions
MATCH (u:Me:User)-[p:POSTS]->(t:Tweet)-[:MENTIONS]->(m:User)
WITH u,p,t,m, COUNT(m.screen_name) AS count
ORDER BY count DESC
RETURN u,p,t,m
LIMIT 10
```
Detalles de algunas de tus menciones

```
// Detailed table of some of your mentions
MATCH (u:User:Me)-[:POSTS]->(t:Tweet)-[:MENTIONS]->(m:User)
RETURN m.screen_name AS screen_name, COUNT(m.screen_name) AS count 
ORDER BY count DESC
LIMIT 10
```

4. **Seguidores más influyentes**

```
// Most influential followers
MATCH (follower:User)-[:FOLLOWS]->(u:User:Me)
RETURN follower.screen_name AS user, follower.followers AS followers
ORDER BY followers DESC
LIMIT 10
```

5. **Hashtags mas usados**

```
// The hashtags you have used most often
MATCH (h:Hashtag)<-[:TAGS]-(t:Tweet)<-[:POSTS]-(u:User:Me)
WITH h, COUNT(h) AS Hashtags
ORDER BY Hashtags DESC
RETURN h.name, Hashtags
LIMIT 10
```

6. **Tasa de seguimiento**. ¿A qué ritmo las personas a las que sigues también te siguen a ti?

```
// Followback rate
MATCH (me:User:Me)-[:FOLLOWS]->(f)
WITH me, f, size((f)-[:FOLLOWS]->(me)) as doesFollowBack
RETURN SUM(doesFollowBack) / toFloat(COUNT(f))  AS followBackRate
```

7. **Recomendaciones de seguidores.** ¿Quién tuitea sobre ti, pero no lo sigues?

```
// Follower Recommendations - tweeting about you, but you don't follow
MATCH (ou:User)-[:POSTS]->(t:Tweet)-[mt:MENTIONS]->(me:User:Me)
WITH DISTINCT ou, me
WHERE (ou)-[:FOLLOWS]->(me) AND NOT (me)-[:FOLLOWS]->(ou)
RETURN ou.screen_name
```

8. **Enlaces de retweets interesantes.** ¿Qué enlaces retuiteas y con qué frecuencia se marcan como favoritos?

```
// Links from interesting retweets
MATCH
  (:User:Me)-[:POSTS]->
  (t:Tweet)-[:RETWEETS]->(rt)-[:CONTAINS]->(link:Link)
RETURN
  t.id_str AS tweet, link.url AS url, rt.favorites AS favorites
ORDER BY
  favorites DESC
LIMIT 10
```

Mismo que el anterior, pero mostrando el nombre del usuario del tweet original

```
// Links from interesting retweets
MATCH
  (u:User:Me)-[:POSTS]->(t:Tweet)-[:RETWEETS]->(rt)-[:CONTAINS]->(link:Link)
MATCH
  (u:User:Me)-[:POSTS]->(t_link:Tweet)-[:RETWEETS]->(rt)<-[:POSTS]-(m:User)
RETURN
  t.id_str AS tweet, link.url AS url, rt.favorites AS favorites, m.screen_name
ORDER BY
  favorites DESC
LIMIT 10
```

9.  **Hashtags comunes.** ¿Qué usuarios tuitean con algunos de tus mejores hashtags?

```
// Users tweeting with your top hashtags
MATCH
  (me:User:Me)-[:POSTS]->(tweet:Tweet)-[:TAGS]->(ht)
MATCH
  (ht)<-[:TAGS]-(tweet2:Tweet)<-[:POSTS]-(sugg:User)
WHERE
  sugg <> me
  AND NOT
  (tweet2)-[:RETWEETS]->(tweet)
WITH
  sugg, collect(distinct(ht)) as tags
RETURN
  sugg.screen_name as friend, size(tags) as common
ORDER BY
  common DESC
LIMIT 20
```