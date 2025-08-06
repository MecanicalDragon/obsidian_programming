![[Collections.svg]]

## Misc

**LinkedList** has 2 advantages over the **ArrayList**:
- ease to add and remove its end elements, that allows it to work like a queue.
- smooth size increase, that allows to save memory if you operate with enormous amounts of data.
In all other cases **ArrayList** has precedence, it is faster because it’s enhanced with intrinsic functions (well-optimized machine dependent instructions and native code alternative) and well-suitable for processor’s [cache lines and prefetches](https://blogs.oracle.com/javamagazine/post/java-and-the-modern-cpu-part-1-memory-and-the-cache-hierarchy)

---
**How to copy a collection**:
- **Immutable proxy**: `Collections.unmodifiableList(list)` – works with the same references; original list updates will be seen in the new proxy; proxy itself won’t allow updating it but will throw an exception.
- **Mutable copy**: `list.stream().collect(toList())` – creates a list with new references that point to original list elements. References updates (list elements replaces) of both collections won’t affect each other.
- **Immutable copy**: `List.copyOf(list)` – creates a list with new references that point to original list elements either.

---
**Fail-fast** – «быстрый» итератор. Когда после его создания коллекция как-либо изменилась, он падает с ошибкой без лишних разбирательств. Так работает итератор *ArrayList* - выбрасывает *ConcurrentModificationException*. Не стоит основывать логику программы на Fail-fast отказах, а использовать их только как признак ошибки реализации.

**Fail-safe** – «умный» итератор. Обычно плата за отказоустойчивость – возможная неконсистентность данных («слабая консистентность»). Итератор *ConcurrentHashMap* работает с копией данных, он не выбросит исключение при изменении коллекции, но может не увидеть часть изменений. Поведение других Fail-safe итераторов может отличаться.

---
