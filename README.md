# Prosty Platformer MVP

## Wstęp @unplugged

Witaj! W tym tutorialu zbudujemy podstawową wersję (MVP) gry platformowej. Twój gracz będzie mógł biegać, skakać pod wpływem grawitacji i zbierać punkty!

```template
let mySprite: Sprite = null
let coin: Sprite = null
tiles.setTilemap(tilemap`level1`)
```

## Krok 1

Zacznijmy od wczytania poziomu. Przeciągnij gotowy blok `||scene:set tilemap to level1||` z sekcji `||scene:Scene||` i umieść go wewnątrz bloku `||loops:on start||`.

Ten blok ma już automatycznie załadowany cały leśny świat, więc nie musisz niczego samodzielnie rysować!

```blocks
tiles.setTilemap(tilemap`level1`)
```

## Krok 2

Teraz dodajmy naszego bohatera. Znajdź blok `||variables(sprites):set mySprite to||` w sekcji `||sprites:Sprites||`. Przeciągnij go do bloku `||loops:on start||` pod mapą kafelków.

Kliknij szary kwadrat i narysuj swoją postać. Upewnij się, że jej rodzaj (kind) to `Player`.

```blocks
tiles.setTilemap(tilemap`level1`)
mySprite = sprites.create(img`.`, SpriteKind.Player)
```

## Krok 3

Chcemy, aby gracz poruszał się w lewo i w prawo. Wyciągnij blok `||controller:move mySprite with buttons||` z sekcji `||controller:Controller||` i umieść go na dole bloku `||loops:on start||`.

Kliknij mały plusik `(+)` na tym bloku i zmień wartość `vy` (prędkość pionową) na `0`. Dzięki temu gracz będzie chodził tylko na boki, a pionem zajmie się grawitacja!

```blocks
tiles.setTilemap(tilemap`level1`)
mySprite = sprites.create(img`.`, SpriteKind.Player)
controller.moveSprite(mySprite, 100, 0)
```

## Krok 4

Bez grawitacji nasza postać będzie latać! Znajdź blok `||sprites:set mySprite acceleration x to 0||` w sekcji `||sprites:Sprites||` (w podkategorii *Physics*). Przeciągnij go na koniec bloku `||loops:on start||`.

Zmień kliknięciem `acceleration x` na `ay (acceleration y)` i wpisz wartość `300`. To sprawi, że postać zacznie spadać w dół.

```blocks
tiles.setTilemap(tilemap`level1`)
mySprite = sprites.create(img`.`, SpriteKind.Player)
controller.moveSprite(mySprite, 100, 0)
mySprite.ay = 300
```

## Krok 5

Czas na skok! Wyciągnij blok wydarzenia `||controller:on A button pressed||` z sekcji `||controller:Controller||` i umieść go osobno na ekranie.

Wewnątrz niego umieść blok `||sprites:set mySprite velocity x to 0||` z sekcji `||sprites:Sprites||`. Zmień `velocity x` na `vy (velocity y)` i wpisz wartość `-150`. Minus oznacza ruch w górę, czyli skok!

```blocks
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    mySprite.vy = -150
})
```

## Krok 6

Ponieważ nasz gotowy poziom jest długi, gracz szybko uciekłby z ekranu. Znajdź blok `||scene:camera follow sprite mySprite||` w sekcji `||scene:Scene||` i umieść go na samym końcu bloku `||loops:on start||`.

```blocks
tiles.setTilemap(tilemap`level1`)
mySprite = sprites.create(img`.`, SpriteKind.Player)
controller.moveSprite(mySprite, 100, 0)
mySprite.ay = 300
scene.cameraFollowSprite(mySprite)
```

## Krok 7

Dodajmy cel gry. Przeciągnij kolejny blok `||variables(sprites):set mySprite2 to||` na koniec `||loops:on start||`. Zmień nazwę zmiennej na `coin` (moneta) i zmień jej rodzaj (kind) z `Player` na `Food`. Narysuj monetę.

Użyj bloku `||scene:place mySprite on top of random||` z sekcji `||scene:Scene||`, aby umieścić monetę losowo na kafelku podłogi (`myTiles.tile1`).

```blocks
tiles.setTilemap(tilemap`level1`)
mySprite = sprites.create(img`.`, SpriteKind.Player)
controller.moveSprite(mySprite, 100, 0)
mySprite.ay = 300
scene.cameraFollowSprite(mySprite)
coin = sprites.create(img`.`, SpriteKind.Food)
tiles.placeOnRandomTile(coin, myTiles.tile1)
```

## Krok 8

Na koniec musimy obsłużyć moment zebrania monety. Wyciągnij duży blok wydarzenia `||sprites:on sprite of kind Player overlaps otherSprite of kind Player||` z sekcji `||sprites:Sprites||`. Zmień drugi rodzaj z `Player` na `Food`.

Wewnątrz tego bloku dodaj:

1. `||info:change score by 1||` z sekcji `||info:Info||`.
2. `||sprites:destroy otherSprite||` z sekcji `||sprites:Sprites||`.

```blocks
sprites.onOverlap(SpriteKind.Player, SpriteKind.Food, function (sprite, otherSprite) {
    info.changeScoreBy(1)
    otherSprite.destroy()
})
```

## Koniec!

Gotowe! Stworzyłeś podstawową wersję platformówki. Przetestuj grę na symulatorze, biegaj, skacz i zbierz monetę!
