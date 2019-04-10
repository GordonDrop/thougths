\#безопасность \#криптография

# Базовые термины криптографии

## Термины

**Криптография** - наука о методах обеспечения конфиденциальности, целостности данных, аутентификации, а также невозможности отказа от авторства.

**Конфиденциальность** - невозможность прочтения информации посторонним

**Целостности(integrity)** - невозможность незаметного из-я информации

**Авторизация** - предоставление прав, а также процесс проверки прав

**Аутентификация** - проверка подлинности

## Шифрование 
Шифрование - обратимое преобразование с ключом

### Виды криптографии

1. Криптография без ключа
    - хэш функции

2. Криптография с секретным ключом
    - Secret-Key Message Authentication
    - Secret-Key Encryption
    - Authenticated Secret-Key Encryption

3. Public-Key Cryptography (2 keys)
    - Shared Secret Key Agreement
    - Digital Signatures

## 1. Криптография без ключа

**Хэширование и hash функции**
```js
String hash hash(String alg, String message);

hash("sha256", "The quick brown fox jumps over the lazy cog");
// e4c4d8f3bf76b692de791a173e05321150f7a345b46484fe427f6acc7ecc81be
```

**Идеальная криптографическая хэш функция:**
   - determenistic - один вход -> один выход
   - высокая скорость вычисления значения
   - one way transform
   - отсутствие коллизий
   - output of fixed size

## 2. Криптография с секретным ключом

### 2.1 Secret-Key Message Authentication
Ex: HMAC - hash-based message authentication code
```js
String MAC (String alg, String message, String secret);
hash_hmac("sha256", "The quick brown fox jumps over the lazy dog", "secret key");
// 4a513ac60b4f0253d95c2687fa104691c77c9ed77e884453c6a822b7b010d36f
```

Reason: usual hash functions valnurable to https://en.wikipedia.org/wiki/Length_extension_attack

### 2.2 Secret-Key Encryption
```
String ciphertext encrypt(String message, String key)
```

ciphertext - уник строка, размер рандом байтов

**Недостаток:** уязвимость. Современное шифрование секр ключа использует три параметра
message, secret key, Initialization Vector (IV, for CBC mode) or nonce (number to be used once, for CTR mode). Расшифровка успешна, только если теже секрет и nonce используются, но при этом неизвестным должен оставаться только секрет.

### 2.3 Authenticated Secret-Key Encryption

Криптография с секретным ключом уязвима к подделке, если только не скобминировать этот подход с аутентификацией.

Стратегия: сначала шифруй затем аутен-й данные с помощью MAC.

Фичи подхода:
- Конфидециальность - сообщение может быть прочтено только с корректным секретным ключом
- Аутентификация и целостность данных - вычисление MAC с коректным сектретным ключоv
- Уникальность с помощью псевдослучайных IV/nonce

## 3. Public-Key Cryptography (2 keys)

Каждый участник имеет:
- Приватный ключ, никому не сообщается
- Публичный, сообщается всем

### 3.1. Shared Secret Key Agreement

Пара ключей приватный-публична - уникальна. Имея публичный ключ, почти невозможно узнать приватный. Зная приватный ключ, можно легко вычислить публичный.

Use case: Обмен сообщения между двумя участниками:
- Каждый делиться публичным ключом
- Каждый комбинируя свой приватный и публичный ключ получает одинаковый общий секретный ключ

### 3.2. Цифровая подпись

Вычисляется из message and private key. Любой участник с публичным ключом, может проверить, что сообщение было подписано приватным ключом.