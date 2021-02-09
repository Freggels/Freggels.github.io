# Informatik 09.02.2021

## Lineare Datenstrukturen

### S.81 Aufgabe 3

#### a)

Ich würde für jeden Gang als Datenstruktur einen Stack wählen, da immer nur dir Maus am nächsten zur Kreuzung wichtig ist. Diese ist dann im Stack als das `top`-Element immer erkennbar und entnehmbar.

Um die "hinteren" Mäuse eines Gangs zu bewegen, müssen zunächst die vorderen bewegt werden.

#### b)

```java
import java.util.Random;
import java.util.Stack;

public class MouseSim {
    public static final int MOUSE_AMOUNT = 20;
    public static Stack<Mouse> entrance, exit, side;

    public static void main(String[] args) {
        entrance = new Stack<Mouse>();
        exit = new Stack<Mouse>();
        side = new Stack<Mouse>();

        for (int i = 0; i < MOUSE_AMOUNT; ++i)
            entrance.push(new Mouse(i));
        printStacks();

        while (exit.size() != MOUSE_AMOUNT) {
            simulationStep();
            printStacks();
        }
    }

    private static void simulationStep() {
        // FALL: Mäuse im Eingang wollen zum Seitengang, welcher leer ist -> problemlos
        if (!entrance.empty() && side.empty()) {
            side.push(entrance.pop());
            return;
        }
        // FALL: Mäuse im Eingang wollen zum Seitengang, wo andere Mäuse sind -> Kampf
        if (!entrance.empty() && !side.empty()) {
            if (entrance.peek().strength > side.peek().strength) {
                side.push(entrance.pop());
            } else {
                exit.push(side.pop());
            }
            return;
        }
        // FALL: Eingang ist leer, im Seitengang sind noch Mäuse -> zum Ausgang
        if (!side.empty()) {
            exit.push(side.pop());
        }
    }

    private static void printStacks() {
        System.out.print("Entrance: ");
        for (Mouse m : entrance) {
            System.out.print("{" + m.id + ":" + m.strength + "}");
        }
        System.out.println();
        System.out.print("Side: ");
        for (Mouse m : side) {
            System.out.print("{" + m.id + ":" + m.strength + "}");
        }
        System.out.println();
        System.out.print("Exit: ");
        for (Mouse m : exit) {
            System.out.print("{" + m.id + ":" + m.strength + "}");
        }
        System.out.println();
    }

    private static class Mouse {
        public final int id;
        public final int strength;

        public Mouse(int id) {
            this.id = id;
            strength = new Random(System.nanoTime()).nextInt(10) + 1;
        }
    }
}
```

##### Output

```
Entrance: {0:6}{1:5}{2:5}{3:3}{4:4}{5:2}{6:7}{7:8}{8:8}{9:7}{10:7}{11:7}{12:1}{13:4}{14:5}{15:5}{16:3}{17:1}{18:1}{19:7}
Side: 
Exit: 
Entrance: {0:6}{1:5}{2:5}{3:3}{4:4}{5:2}{6:7}{7:8}{8:8}{9:7}{10:7}{11:7}{12:1}{13:4}{14:5}{15:5}{16:3}{17:1}{18:1}
Side: {19:7}
Exit: 
Entrance: {0:6}{1:5}{2:5}{3:3}{4:4}{5:2}{6:7}{7:8}{8:8}{9:7}{10:7}{11:7}{12:1}{13:4}{14:5}{15:5}{16:3}{17:1}{18:1}
Side: 
Exit: {19:7}
Entrance: {0:6}{1:5}{2:5}{3:3}{4:4}{5:2}{6:7}{7:8}{8:8}{9:7}{10:7}{11:7}{12:1}{13:4}{14:5}{15:5}{16:3}{17:1}
Side: {18:1}
Exit: {19:7}

...

Entrance: 
Side: {12:1}{5:2}
Exit: {19:7}{18:1}{15:5}{14:5}{13:4}{16:3}{17:1}{11:7}{10:7}{8:8}{7:8}{9:7}{6:7}{4:4}{2:5}{0:6}{1:5}{3:3}
Entrance: 
Side: {12:1}
Exit: {19:7}{18:1}{15:5}{14:5}{13:4}{16:3}{17:1}{11:7}{10:7}{8:8}{7:8}{9:7}{6:7}{4:4}{2:5}{0:6}{1:5}{3:3}{5:2}
Entrance: 
Side: 
Exit: {19:7}{18:1}{15:5}{14:5}{13:4}{16:3}{17:1}{11:7}{10:7}{8:8}{7:8}{9:7}{6:7}{4:4}{2:5}{0:6}{1:5}{3:3}{5:2}{12:1}
```
