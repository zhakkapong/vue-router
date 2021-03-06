# Das Route-Objekt

Das **Route-Objekt** repräsentiert den Zustand der aktuell aktivierten Route. Es enthält geparste Informationen zur aktuellen URL und den Route-Einträgen, die mit der URL zusammentreffen.

Das Route-Objekt ist unveränderbar. Jede erfolgreiche Navigation resultiert in einem neuen Route-Objekt.

Das Route-Objekt kann an mehreren Orten gefunden werden:

- innerhalb von Komponenten als `this.$route` und offensichtlich innerhalb von Überwachungen des `$route`-Callbacks

- als der wiedergegebene Wert von `router.match(location)`

- innerhalb des Navigationsschutzes als die ersten zwei Argumente:

  ``` js
  router.beforeEach((to, from, next) => {
    // 'to' und 'from' sind Router-Objekte
  })
  ```

- innerhalb der `scrollBehavior`-Funktion als die ersten zwei Argumente:

  ``` js
  const router = new VueRouter({
    scrollBehavior (to, from, savedPosition) {
        // 'to' und 'from' sind Router-Objekte
    }
  })
  ```

### Eigenschaften des Router-Objekts

- **$route.path**

  - Typ: `string`

    Ein String, der gleich dem Pfad der aktuellen Route ist und immer in einen absoluten umgewandelt wird, zB. `"/foo/bar"`.

- **$route.params**

  - Typ: `Object`

    Ein Objekt, welches Schlüssel/Wert-Paare von Stern- und dynamischen Segmenten enthält. Gibt es keine Parameter, ist der Wert ein leeres Objekt.

- **$route.query**

  - Typ: `Object`

    Ein Objekt, welches Schlüssel/Wert-Paare des Abfrage-Strings enthält. Für den Pfad `/foo?user=1` erhält man zum Beispiel `$route.query.user == 1`. Gibt es keine Abfrage, ist der Wert ein leeres Objekt.

- **$route.hash**

  - Typ: `string`

    Der Hash der aktuellen Route (ohne `#`). Gibt es keinen Hash, ist dessen Wert ein leerer String.

- **$route.fullPath**

  - Typ: `string`

    Die voll umgewandelte URL inklusive Abfrage und Hash.

- **$route.matched**

  - Typ: `Array<RouteRecord>`

  Ein Array von **Route-Einträgen** für alle verschachtelten Pfadsegmente der aktuellen Route. Route-Einträge sind Kopien des Objekts im Array der `routes`-Konfiguration und in `children`-Arrays:

  ``` js
  const router = new VueRouter({
    routes: [
      // das folgende Objekt in ein Route-Eintrag
      { path: '/foo', component: Foo,
        children: [
          // das ist auch ein Route-Eintrag
          { path: 'bar', component: Bar }
        ]
      }
    ]
  })
  ```

  Wenn die URL `/foo/bar` ist, ist `$route.matched` ein Array, welcher beide geklonten Objekte von Parent nach Child sortiert enthält.

- **$route.name**

  Der Name der aktuellen Route, sofern vorhanden. Siehe [Benannte Routes](../essentials/named-routes.md).
