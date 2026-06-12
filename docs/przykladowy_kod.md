# Przykładowy kod

Poniższy, uproszczony przykład przedstawia najważniejsze elementy aplikacji: wyświetlanie planszy, sterowanie pionkiem, blokowanie przeszkód oraz wykrywanie dotarcia do mety.

Sterowanie odbywa się za pomocą klawiszy:

- `W`, `S`, `A`, `D` - ruch w pionie i poziomie,
- `Q`, `E`, `Z`, `C` - ruch po skosie,
- `X` - zakończenie programu.

```c
#include <stdio.h>

#define WIERSZE 7
#define KOLUMNY 10

const char plansza[WIERSZE][KOLUMNY + 1] = {
    "##########",
    "#        #",
    "#  ###   #",
    "#    #   #",
    "# ##     #",
    "#       M#",
    "##########"
};

void wyswietl_plansze(int pionek_x, int pionek_y)
{
    int y;
    int x;

    for (y = 0; y < WIERSZE; y++) {
        for (x = 0; x < KOLUMNY; x++) {
            if (x == pionek_x && y == pionek_y) {
                putchar('P');
            } else {
                putchar(plansza[y][x]);
            }
        }
        putchar('\n');
    }
}

int main(void)
{
    int pionek_x = 1;
    int pionek_y = 1;
    char ruch;

    while (plansza[pionek_y][pionek_x] != 'M') {
        int zmiana_x = 0;
        int zmiana_y = 0;
        int nowy_x;
        int nowy_y;

        wyswietl_plansze(pionek_x, pionek_y);
        printf("Podaj ruch (W/S/A/D/Q/E/Z/C lub X): ");
        if (scanf(" %c", &ruch) != 1) {
            return 0;
        }

        switch (ruch) {
            case 'w': case 'W': zmiana_y = -1; break;
            case 's': case 'S': zmiana_y = 1;  break;
            case 'a': case 'A': zmiana_x = -1; break;
            case 'd': case 'D': zmiana_x = 1;  break;
            case 'q': case 'Q': zmiana_x = -1; zmiana_y = -1; break;
            case 'e': case 'E': zmiana_x = 1;  zmiana_y = -1; break;
            case 'z': case 'Z': zmiana_x = -1; zmiana_y = 1;  break;
            case 'c': case 'C': zmiana_x = 1;  zmiana_y = 1;  break;
            case 'x': case 'X': return 0;
            default:
                printf("Nieznane polecenie.\n\n");
                continue;
        }

        nowy_x = pionek_x + zmiana_x;
        nowy_y = pionek_y + zmiana_y;

        if (plansza[nowy_y][nowy_x] == '#') {
            printf("To pole jest niedostepne.\n\n");
        } else {
            pionek_x = nowy_x;
            pionek_y = nowy_y;
        }
    }

    wyswietl_plansze(pionek_x, pionek_y);
    printf("Gratulacje! Pionek dotarl do mety.\n");

    return 0;
}
```

Znak `#` oznacza przeszkodę, `P` aktualne położenie pionka, a `M` pole końcowe.
