# NOTE

This fork builds on top of the [erickzanardo/flutter-gh-pages repository](https://github.com/erickzanardo/flutter-gh-pages) and borrows the best from [TesteurManiak](https://github.com/TesteurManiak/flutter-gh-pages/tree/feat/wasm-compilation) and [Mufaddal1125](https://github.com/cleverflow/flutter-gh-pages/tree/main) improvements.

# Flutter GH Pages

Automates the build and deployment of your Flutter web app on Github gh pages

# Action

To use this action, create an action like the following on your workflows folder

```yml
name: Gh-Pages

on:
  push:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2 # Only works with v2
      - uses: subosito/flutter-action@v1
      - uses: WilliamKarolDiCioccio/flutter-gh-pages@v7
```
To build a project in a folder other that the root, use the `workingDir` property

```yml
      ...
      - uses: WilliamKarolDiCioccio/flutter-gh-pages@v7
        with:
          workingDir: example
```

By default, the action will use the auto setting for web renderers, to change that you can use the `webRenderer` property (this CLI option has been deprecated in flutter 3.29.0 and it's automatically ignored based on the installed SDK version)

More on web renderers here: https://flutter.dev/docs/development/tools/web-renderers

```yml
      ...
      - uses: WilliamKarolDiCioccio/flutter-gh-pages@v8
        with:
          webRenderer: canvaskit
```

You can also specify if you want to build to WebAssembly with the `compileToWasm` property. If specified the `webRenderer` property will be ignored.

More on WebAssembly here: https://docs.flutter.dev/platform-integration/web/wasm

```yml
      ...
      - uses: bluefireteam/flutter-gh-pages@v7
        with:
          compileToWasm: true
```

By default, the action will send the files to the `gh-pages` branch, which is the default used by Github Pages.
If you need to change that, the `targetBranch` property can be used

```yml
      ...
      - uses: WilliamKarolDiCioccio/flutter-gh-pages@v7
        with:
          targetBranch: my-gh-pages-branch
```

By default, the generated page only works on `User or Organization sites` ex:`user.github.io/`. 
You can change that by specifying the `baseHref` argument, so the site will work on a `Project Site`, ex:`user.github.io/repoName`.

The parameter `baseHref` must start and end with a forward slash `"/"`.
(For projects created on flutter version earlier than `2.5.0`, please manually edit the file `web/index.html`, changing the line `<base href="/">` to `<base href="$FLUTTER_BASE_HREF">`)

```yml
      ...
      - uses: WilliamKarolDiCioccio/flutter-gh-pages@v7
        with:
          baseHref: /my-repo/
```

To pass arguments to the builder with `--dart-define` the `customArgs` property can be used

```yml
      ...
      - uses: WilliamKarolDiCioccio/flutter-gh-pages@v7
        with:
          customArgs: --dart-define="simple=example"
```


And consumed in the code via (**const** is mandatory!):
```dart
void main() async {
  String arg = const String.fromEnvironment('simple'); // arg = "example"
  ...
}
```


To use github pages with a custom domain, add a file named `CNAME` to the
`<project>/web` folder whose contents is your domain, like:
> subdomain.domain.com
