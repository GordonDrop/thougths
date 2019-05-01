# Npm notes

**Версия** - строка вида v2.0.0.

## Npm semver ranges

**Range** - набор компораторов для проверки.

Компоратор составляется из оператора и версии:

- < Less than
- <= Less than or equal to
- \> Greater than
- \>= Greater than or equal to
- = Equal. If specified or not. Optional.

Комбинация компораторов через пробел, как **пересечение**: >=1.2.7 <1.3.0.

### Advanced ranges types

- Hyphen Ranges X.Y.Z - A.B.C§
 Ex: 1.2.3 - 2.3.4, 1.2 - 2.3.4(после 1.2.0), 1.2.3 - 2(до 3.0.0)
 
- X-Ranges, Ex: * - any version, 1.x (1.0.0-2.0.0), '' same as *

- tilde ranges - allows patch-level changes if a minor version is specified on the comparator. Allows minor-level changes if not.

Ex ~1.2 := >=1.2.0 <1.(2+1).0 := >=1.2.0 <1.3.0 (same as 1.2.x)

- caret ranges  ^1.2.3 ^0.2.5 ^0.0.4

Allows changes that do not modify the left-most non-zero digit in the [major, minor, patch] tuple. In other words, this allows patch and minor updates for versions 1.0.0 and above, patch updates for versions 0.X >=0.1.0, and no updates for versions 0.0.X.

Ex: 
  - ^1.2.3 := >=1.2.3 <2.0.0
  - ^0.2.3 := >=0.2.3 <0.3.0
  - ^0.0.3 := >=0.0.3 <0.0.4
  - ^1.2.x := >=1.2.0 <2.0.0
  - ^0.0.x := >=0.0.0 <0.1.0
  - ^0.0 := >=0.0.0 <0.1.0
  
## Package locks

npm не всегда может ставить каждый раз пакеты одинаково.

Запускаем npm install и генерируется package-lock файл. Все будущие установки базируются на этом файле.

Сам файл это древовидное и воспроизводимое представление node_modules:

```json
{
  "name": "A",
  "version": "0.1.0",
  ...metadata fields...
  "dependencies": {
    "B": {
      "version": "0.0.1",
      "resolved": "https://registry.npmjs.org/B/-/B-0.0.1.tgz",
      "integrity": "sha512-DeAdb33F+",
      "dependencies": {
        "C": {
          "version": "git://github.com/org/C.git#5c380ae319fc4efe9e7f2d9c78b0faa588fd99b4"
        }
      }
    }
  }
}
```

Если файл есть, то процесс установки будет таков:

1. воспроизведение структуры в lock файле, если есть resolved поле, или к обычному разрешению версии пакета

2. Проходи по дереву и установка зависимостей, которых нет в обычной манере.

### Использование lock файлов

Любая команда, которая обновляет node_modules или package.json автоматически синхронизироует lock файлы: npm install, npm rm, npm update, etc. Чтобы предотвратить обновление используйте флаг `--no-save`

Коммитьте файл :) Это польза :)

### Merge конфликты

`npm install [--package-lock-only]`

or

install `npm-merge-driver` для авто ресолва конфликтов