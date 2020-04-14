# Hash table

Other names: `hash`, `hash map`, `map`, `unordered map`, `dictionary`.

![Img](assets/hash-table.png)

A hash table organizes data so you can quickly look up values for a given key. For example, **Associative array**. An array only support integer for key, whereas hash table can use key of any type as long as they're hashable.

## Strengths

- **Fast lookups**. Lookups take O(1) time _on average_.
- **Flexible keys**. Most data types can be used for keys, as long as they're hashable.

## Weaknesses

- **Slow worst-case lookups**. Lookups can take O(n) time _in the worst case_.
- **Unordered**. Keys aren't stored in a special order. If you're looking for the smallest key you'll need to look through every key to find it.

## Hash collisions

When you are implementing a hash table you have to assume that hash collisions are unavoidable. The two main approaches to solving these collisions are **Chaining** and **Open Addressing**.

[Hashing technique](https://www.youtube.com/watch?v=mFY0J5W8Udk)

## Complexity

| Algo   | Average | Worst |
|--------|---------|-------|
| Search | O(1)    | O(n)  |
| Insert | O(1)    | O(n)  |
| Delete | O(1)    | O(n)  |

Worst case is O(n) for all operations because if too many elements were hashed into the same key then looking inside this key may take O(n) time.
