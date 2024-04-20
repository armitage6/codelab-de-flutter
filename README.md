# Proyecto Flutter

1. Luego de tener todo instalado iniciamos nuestro proyecto en vs con F1 y la opcion Flutter: new project.

## Conceptos

- La recarga en caliente se activa cuando guardas cambios en un archivo fuente.

## Analisis de lib/main.dart

```
// ...

void main() {
runApp(MyApp());
}

// ...

```

1. En la parte superior del archivo, encontrarás la función main(). En su forma actual, solo le indica a Flutter que ejecute la app definida en MyApp.

2.

```
// ...

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => MyAppState(),
      child: MaterialApp(
        title: 'Namer App',
        theme: ThemeData(
          useMaterial3: true,
          colorScheme: ColorScheme.fromSeed(seedColor: Colors.deepOrange),
        ),
        home: MyHomePage(),
      ),
    );
  }
}

// ...
```

- La clase MyApp extiende StatelessWidget. Los widgets son los elementos a partir de los cuales compilarás cada app de Flutter. Como puedes ver, incluso la propia app es un widget.
  El código de MyApp configura la app por completo. Crea un estado de toda la app , le asigna un nombre a la app, define el tema visual y establece el widget "principal" (el punto de partida de tu app).

3.

```
// ...

class MyAppState extends ChangeNotifier {
  var current = WordPair.random();
}

// ...
```

- MyAppState define los datos que la app necesita para funcionar. En este momento, solo contiene una única variable con el par actual de palabras aleatorias. Cambiarás esto más adelante.
- La clase de estado extiende ChangeNotifier, lo que significa que puede notificar a otros acerca de sus propios cambios. Por ejemplo, si el par actual de palabras cambia, algunos widgets de la app deben saber esto.
- El estado se crea y se brinda a toda la app mediante un ChangeNotifierProvider (consulta el código anterior en MyApp). Esto le permite a cualquier widget de la app obtener el estado.

```
// ...

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {           // ← 1
    var appState = context.watch<MyAppState>();  // ← 2

    return Scaffold(                             // ← 3
      body: Column(                              // ← 4
        children: [
          Text('A random AWESOME idea:'),        // ← 5
          Text(appState.current.asLowerCase),    // ← 6
          ElevatedButton(
            onPressed: () {
              print('button pressed!');
            },
            child: Text('Next'),
          ),
        ],                                       // ← 7
      ),
    );
  }
}

// ...
```

- Cada widget define un método build() que se llama automáticamente cada vez que cambian las circunstancias del widget de modo que este siempre esté actualizado.
  MyHomePage realiza el seguimiento del estado actual de la app usando el método watch.
- Cada método build debe mostrar un widget o un árbol de widgets anidado (algo más típico). En este caso, el widget de nivel superior es Scaffold. No vas a trabajar con Scaffold en este codelab, pero es un widget útil y se encuentra en la gran mayoría de las apps de Flutter del mundo real.
- Column es uno de los widgets de diseño más básicos de Flutter. Toma una cantidad cualquiera de elementos secundarios y los encolumna desde arriba hacia abajo. De forma predeterminada, la columna ubica visualmente sus elementos secundarios en la parte superior. Pronto cambiarás esto de modo que la columna esté alineada en el centro.
  Cambiaste este widget de Text en el primer paso.
- Este segundo widget de Text toma el appState y accede al único miembro de esa clase, current (que es un WordPair). WordPair proporciona varios métodos get de utilidad, como asPascalCase o asSnakeCase. Aquí, usaremos asLowerCase, pero puedes cambiar esto ahora si prefieres alguna otra alternativa.
- Observa cómo el código de Flutter usa en gran medida las comas finales. Esta coma en particular no necesita estar aquí, ya que children es el último (y el único) miembro de esta particular lista de parámetros de Column. Sin embargo, en general, resulta útil usar las comas finales: hace que agregar más miembros sea algo trivial y sirve como una pista para que el aplicador de formato automático de Dart sepa que debe insertar una nueva línea ahí. Si deseas obtener más información, consulta Formato del código.
