

# Python asyncio und AbstractEventLoop

`asyncio` ist ein Modul in Python, das für die asynchrone Programmierung verwendet wird. Es ermöglicht die gleichzeitige Ausführung von Aufgaben, ohne dass mehrere Threads oder Prozesse benötigt werden. Das Modul basiert auf dem Konzept des Ereignisschleifenmodells (Event Loop), das vor allem in Netzwerk- und I/O-intensiven Anwendungen verwendet wird.

## Grundlegende Konzepte von asyncio:

1. **Coroutine**:
   - Eine Coroutine ist eine Funktion, die durch das Schlüsselwort `async` definiert wird und mit dem Schlüsselwort `await` aufgerufen werden kann.
   - Beispiele:
     ```python
     async def my_coroutine():
         await some_async_function()
     ```

2. **Event Loop**:
   - Der Event Loop (Ereignisschleife) ist das zentrale Element in `asyncio`. Er sorgt dafür, dass Coroutines ausgeführt und I/O-Operationen behandelt werden.
   - Der Event Loop verwaltet den Ablauf der asynchronen Aufgaben und wechselt zwischen ihnen, sobald sie auf I/O warten müssen.

3. **Tasks**:
   - Eine Task ist eine Coroutine, die vom Event Loop verwaltet wird. Tasks ermöglichen es, mehrere Coroutines gleichzeitig auszuführen.
   - Erstellen einer Task:
     ```python
     loop = asyncio.get_event_loop()
     task = loop.create_task(my_coroutine())
     ```

4. **Future**:
   - Ein Future ist ein Objekt, das ein zukünftiges Ergebnis einer asynchronen Operation darstellt. Es wird verwendet, um das Ergebnis von Coroutines oder anderen asynchronen Operationen zu speichern.

## `AbstractEventLoop`:

`AbstractEventLoop` ist eine abstrakte Basisklasse, die die API für einen Event Loop definiert. Sie wird in `asyncio` verwendet, um spezifische Implementierungen von Ereignisschleifen zu kapseln. Obwohl man `AbstractEventLoop` normalerweise nicht direkt verwendet, stellt sie die grundlegende API zur Verfügung, die alle konkreten Event Loop-Implementierungen bereitstellen müssen.

Hier sind einige der wichtigsten Methoden und Eigenschaften von `AbstractEventLoop`:

- **`run_forever()`**:
  - Startet den Event Loop und lässt ihn unendlich laufen, bis `stop()` aufgerufen wird.
  - Beispiel:
    ```python
    loop = asyncio.get_event_loop()
    loop.run_forever()
    ```

- **`run_until_complete(future)`**:
  - Startet den Event Loop und führt ihn aus, bis das übergebene `Future` abgeschlossen ist.
  - Beispiel:
    ```python
    loop = asyncio.get_event_loop()
    loop.run_until_complete(my_coroutine())
    ```

- **`call_soon(callback, *args)`**:
  - Plant die Ausführung eines Callbacks (einer Funktion) so bald wie möglich.
  - Beispiel:
    ```python
    loop.call_soon(print, "Hello, World!")
    ```

- **`call_later(delay, callback, *args)`**:
  - Plant die Ausführung eines Callbacks nach einer bestimmten Verzögerung (in Sekunden).
  - Beispiel:
    ```python
    loop.call_later(5, print, "This will print after 5 seconds")
    ```

- **`call_at(when, callback, *args)`**:
  - Plant die Ausführung eines Callbacks zu einem bestimmten Zeitpunkt (Unix-Zeit).
  - Beispiel:
    ```python
    import time
    loop.call_at(time.time() + 10, print, "This will print after 10 seconds")
    ```

- **`create_task(coro)`**:
  - Startet eine Coroutine als Task und gibt das Task-Objekt zurück.
  - Beispiel:
    ```python
    task = loop.create_task(my_coroutine())
    ```

- **`stop()`**:
  - Stoppt den Event Loop.
  - Beispiel:
    ```python
    loop.stop()
    ```

- **`close()`**:
  - Schließt den Event Loop und gibt Ressourcen frei.
  - Beispiel:
    ```python
    loop.close()
    ```

## Anwendungsbeispiele:

Ein einfaches Beispiel, das die Verwendung von `asyncio` zeigt:

```python
import asyncio

async def say_hello():
    await asyncio.sleep(1)
    print("Hello, World!")

async def main():
    task1 = asyncio.create_task(say_hello())
    task2 = asyncio.create_task(say_hello())

    await task1
    await task2

loop = asyncio.get_event_loop()
loop.run_until_complete(main())








