+++
author = "Cristian Porzio"
title = "Coding Curiosity"
date = "2023-11-14"
description = "Il Countdown Problem"
tags = [
    "blog",
    "coding",
]
categories = [
    "Post",
    "Coding",
]
series = ["Annunci"]
image = "lamp.jpg"
+++

TLDR: Hai un array [1, 3, 5, 7, 10, 25, 50]. Devi trovare una combinazione che faccia 725=(25-10)*(50+1).
<!--more-->

## Premessa

Ho in mente di creare questa nuova rubrica principalmente orientata alla programmazione, non eccessivamente techy né particolarmente orientata alle tecnologie. Dovrebbe essere più una trattazione di argomenti semplici ed alla portata di chiunque.

## Cosa ho portato oggi

Ci sono migliaia di ‘programmini’ divertenti che sono divertenti da implementare, uno di questi proviene dal gameshow inglese [Countdown](https://en.wikipedia.org/wiki/Countdown_(game_show)) del 1982 che per 20 anni è rimasto uno dei 4 programmi più visti. Gli step del gioco erano:

- Chi diveniva il vincitore diventava comandante per il prossimo episodio.
- Il comandante sceglieva la disposizione delle 6 delle 24 tessere numeriche così composte:
    - 20 numeri piccoli (da 1 a 10) e 4 numeri grandi a caso.
    - Il concorrente poteva decidere quanti numeri grandi utilizzare (da 0 a 4).
- Venivano sorteggiati i numeri e dati ai concorrenti.
- Una macchina generava un numero casuale.
- I concorrenti dovevano trovare una sequenza di calcoli che si avvicini il più possibile al numero generato **entro 30 secondi**.

- Un numero non può essere usato per più di un calcolo.
- La divisione può essere eseguita solo se il risultato non da resto.
- Il risultato ha solo numeri interi positivi in qualsiasi fase del calcolo.
- Se il risultato risulta essere uguale a quello di un altro concorrente, deve mostrare all’altro il proprio lavoro.
- I concorrenti devono dichiarare i risultati prima del tempo.

## E che c'entra la programmazione?
```java
// Using OptionalInt instead of List<Integer>
   static OptionalInt eval(Expr expr) {
      return switch (expr) {
         // eval (Val n)     = [n | n > 0]
         case Val(var n) -> n > 0 ? OptionalInt.of(n) : OptionalInt.empty();

         // eval (App o l r) = [apply o x y | x <- eval l,
         //                                   y <- eval r,
         //                                   valid o x y]
         case App(var op, var l, var r) -> {
            var x = eval(l);
            var y = eval(r);
            yield (x.isPresent() && y.isPresent() &&
                   isValid(op, x.getAsInt(), y.getAsInt())) ?
               OptionalInt.of(apply(op, x.getAsInt(), y.getAsInt())) :
               OptionalInt.empty();
         }
      };
   }
```
Nel codice che abbiamo esaminato, la funzione `eval` è responsabile di valutare un'espressione `Expr`. Un'espressione `Expr` può essere un valore (`Val`) o un'applicazione (`App`) di un operatore a due espressioni.

Per un valore, `eval` controlla se il valore è maggiore di zero. Se lo è, restituisce un `OptionalInt` che contiene quel valore. Altrimenti, restituisce un `OptionalInt` vuoto.

Per un'applicazione, `eval` chiama ricorsivamente se stessa sui due argomenti dell'applicazione. Se entrambi i risultati sono presenti e la combinazione dell'operatore e dei due risultati è valida (verificata dalla funzione `isValid`, che non è mostrata nel codice che abbiamo esaminato), allora `eval` chiama la funzione `apply` sui due risultati e restituisce un `OptionalInt` che contiene il risultato. Altrimenti, restituisce un `OptionalInt` vuoto.

Quindi, in sostanza, la funzione `eval` cerca di esprimere il numero target come una combinazione di valori e operazioni matematiche basate sui numeri e gli operatori dati. Se riesce a trovare una tale espressione, la restituisce come un `OptionalInt`. Altrimenti, restituisce un `OptionalInt` vuoto.

Tutto chiaro? Ni.
In realtà un problemino del genere rientra nell'ambito della Programmazione Dinamica e richiede delle conoscenze non banali. Facciamo un altro codice che sia più semplice

```java
public class CountdownSolver {

    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);
        int target = 42;

        List<String> solutions = countdownSolver(numbers, target);

        if (solutions.isEmpty()) {
            System.out.println("No solution found.");
        } else {
            System.out.println("Solutions:");
            for (String solution : solutions) {
                System.out.println(solution);
            }
        }
    }

    public static List<String> countdownSolver(List<Integer> numbers, int target) {
        List<String> solutions = new ArrayList<>();
        countdownHelper(numbers, target, "", solutions);
        return solutions;
    }

    private static void countdownHelper(List<Integer> numbers, int target, String expression, List<String> solutions) {
        if (target == 0) {
            solutions.add(expression);
            return;
        }

        for (int i = 0; i < numbers.size(); i++) {
            int currentNumber = numbers.get(i);

            // Generate new list without the current number
            List<Integer> remainingNumbers = new ArrayList<>(numbers);
            remainingNumbers.remove(i);

            // Try addition
            countdownHelper(remainingNumbers, target - currentNumber, expression + "+" + currentNumber, solutions);

            // Try subtraction
            countdownHelper(remainingNumbers, target + currentNumber, expression + "-" + currentNumber, solutions);

            // Try multiplication
            countdownHelper(remainingNumbers, target / currentNumber, expression + "*" + currentNumber, solutions);

            // Try division
            if (target % currentNumber == 0) {
                countdownHelper(remainingNumbers, target * currentNumber, expression + "/" + currentNumber, solutions);
            }
        }
    }
}
```
- countdownHelper è un metodo ricorsivo che esplora tutte le possibili combinazioni di numeri ed operatori per raggiungere quel target.
    - caso base: se raggiunge il target (arriva a 0) aggiungi questa espressione alla lista delle soluzioni e ritorna.
    - passo iterativo: effettuiamo un loop con ogni numero presente nella lista e per ogni numero esploriamo i 4 operatori ricorsivamente.

Ops, abbiamo dimenticato una cosa! Cosa accade se i numeri dati non riescono a raggiungere il nostro target? Niente, dice che non è possibile perché abbiamo esplorato tutte le possibili soluzioni ricorsivamente. **E questo è terribile, oltre ad essere un approccio BruteForce**. Facciamo un codice che restituisca il risultato che si avvicina maggiormente (quindi che dia valore massimo rispetto agli altri e compreso rispetto a target)
```java
public class CountdownSolver {

    public static void main(String[] args) {
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);
        int target = 42;

        Result bestResult = countdownSolver(numbers, target);

        if (bestResult.isFound()) {
            System.out.println("Best Solution: " + bestResult.getExpression());
        } else {
            System.out.println("No solution found.");
        }
    }

    public static Result countdownSolver(List<Integer> numbers, int target) {
        Result bestResult = new Result(Integer.MAX_VALUE, "");
        countdownHelper(numbers, target, "", bestResult);
        return bestResult;
    }

    private static void countdownHelper(List<Integer> numbers, int target, String expression, Result bestResult) {
        if (Math.abs(target) < Math.abs(bestResult.getValue())) {
            bestResult.updateResult(target, expression);
        }

        if (target == 0) {
            bestResult.updateFound(expression);
            return;
        }

        for (int i = 0; i < numbers.size(); i++) {
            int currentNumber = numbers.get(i);

            List<Integer> remainingNumbers = new ArrayList<>(numbers);
            remainingNumbers.remove(i);

            countdownHelper(remainingNumbers, target - currentNumber, expression + "+" + currentNumber, bestResult);
            countdownHelper(remainingNumbers, target + currentNumber, expression + "-" + currentNumber, bestResult);

            if (currentNumber != 0 && target % currentNumber == 0) {
                countdownHelper(remainingNumbers, target / currentNumber, expression + "*" + currentNumber, bestResult);
            }

            if (currentNumber != 0 && target % currentNumber == 0) {
                countdownHelper(remainingNumbers, target * currentNumber, expression + "/" + currentNumber, bestResult);
            }
        }
    }

    private static class Result {
        private int value;
        private String expression;
        private boolean found;

        public Result(int value, String expression) {
            this.value = value;
            this.expression = expression;
            this.found = false;
        }

        public int getValue() {
            return value;
        }

        public String getExpression() {
            return expression;
        }

        public boolean isFound() {
            return found;
        }

        public void updateFound(String newExpression) {
            this.expression = newExpression;
            this.found = true;
        }

        public void updateResult(int newValue, String newExpression) {
            this.value = newValue;
            this.expression = newExpression;
        }
    }
}
```

Abbiamo utilizzato una strategia molto simile a quella precedente ma abbiamo creato una nuova classe `Result` che incapsula il risultato e l'espressione attuale. La funzione `countdownSolver`invece aggiorna `bestResult` contenendo l'espressione che è più vicina allo 0.

## Conclusioni
Il Countdown Problem ci offre non solo un intrigante rompicapo matematico ma anche una finestra su come combinare creatività e logica per trovare soluzioni. Abbiamo esplorato diverse strategie e operatori per avvicinarci al nostro obiettivo, ma come ogni buon problema, il Countdown ci sfida a pensare fuori dagli schemi e a cercare nuovi modi di affrontare le sfide. Che tu sia un appassionato di matematica o solo un curioso, ti invito a sperimentare questo problema e a condividere le tue scoperte. La matematica è un viaggio senza fine di esplorazione e apprendimento, e il Countdown Problem è solo uno dei tanti nodi intriganti lungo il percorso. Buon conto alla rovescia!
