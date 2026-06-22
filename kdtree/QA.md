# Q&A

<details>
  <summary>1. Why squared Euclidean distance instead of Euclidean distance?</summary>

  > Because it gives the same closest-point result, but it is faster to compute.
  >
  > Euclidean distance is just the square root of squared distance. Since square root does not change which value is bigger or smaller, the nearest point stays the same. We only use normal Euclidean distance when we need the exact distance value.
</details>

<details>
  <summary>2. Why do we have both <code>case class Point</code> and <code>object Point</code>?</summary>

  > `case class Point` defines what a point instance is and what data and methods each point has.
  >
  > `object Point` is the companion object. It holds shared helpers related to `Point`, such as factory methods like `apply(...)`.
</details>

<details>
  <summary>3. Why is <code>apply(xs: Double*)</code> inside <code>object Point</code> and not inside the case class?</summary>

  > Because `apply` is used to create a new `Point` from the type itself, like `Point(1.0, 2.0)`.
  >
  > If it were inside the case class, it would be an instance method and would require an existing `Point` first. The companion object is the right place for constructors and factory helpers.
</details>

<details>
  <summary>4. What is <code>Seq[Point]</code> in Scala?</summary>

  > `Seq[Point]` means a sequence of `Point` objects.
  >
  > For a Java developer, the closest idea is `List&lt;Point&gt;`. It is an ordered collection, but it does not force one specific implementation.
</details>

<details>
  <summary>5. Does <code>Seq</code> sort the elements by itself?</summary>

  > No. `Seq` only means the elements have an order, like first, second, third.
  >
  > It does not sort automatically. Sorting only happens when the code explicitly asks for it, for example with `sortBy(...)`.
</details>

<details>
  <summary>6. What does the <code>_</code> mean in <code>points.sortBy(_(axis))</code>?</summary>

  > The `_` is shorthand for the current element.
  >
  > Since `points` is a `Seq[Point]`, `_` means "one point". So `_(axis)` means "take this point and get its coordinate at `axis`".
</details>

<details>
  <summary>7. What is a clearer version of <code>points.sortBy(_(axis))</code>?</summary>

  > A clearer version is `points.sortBy(point =&gt; point(axis))`.
  >
  > It means: for each point, use the coordinate at `axis` as the sorting key.
</details>

<details>
  <summary>8. Why do we use <code>enum KDTree</code>?</summary>

  > Because a KD-tree has only two possible shapes: it is either empty or it is a node with data and children.
  >
  > `enum` is a clean way in Scala 3 to model a type that has a fixed set of possible cases.
</details>

<details>
  <summary>9. What does <code>enum KDTree</code> mean in this project?</summary>

  > It means a `KDTree` can only be one of these:
  >
  > - `KDTree.Empty`
  > - `KDTree.Node(point, axis, left, right)`
  >
  > This makes the tree structure explicit and recursive.
</details>

<details>
  <summary>10. Why is <code>enum</code> useful with pattern matching?</summary>

  > Because Scala knows all possible cases of the enum.
  >
  > When we write a `match`, Scala can check that we handled every possible tree shape, such as `Empty` and `Node(...)`.
</details>

<details>
  <summary>11. How is <code>enum KDTree</code> similar to older Scala code?</summary>

  > It is similar to writing a sealed hierarchy like `sealed trait KDTree`, `case object Empty`, and `case class Node(...)`.
  >
  > Scala 3 `enum` is just a cleaner and more modern way to express the same idea.
</details>

<details>
  <summary>12. What is pattern matching in Scala?</summary>

  > Pattern matching is a way to check a value's shape and extract its data in one step.
  >
  > For a Java developer, it is like combining `switch`, `instanceof`, and field extraction into a single construct.
</details>

<details>
  <summary>13. Why can Scala use the same name for <code>enum KDTree</code> and <code>object KDTree</code>?</summary>

  > Because they play different roles.
  >
  > `enum KDTree` defines the type and its possible cases. `object KDTree` is the companion object, which holds shared helper methods. For a Java developer, the companion object is similar to the place where you would usually put `static` methods.
</details>

<details>
  <summary>14. What is <code>sorted</code> in <code>point = sorted(mid)</code>?</summary>

  > `sorted` is just a local variable created earlier with `val sorted = points.sortBy(...)`.
  >
  > It contains the points after sorting, and `sorted(mid)` picks the element at the middle index.
</details>

<details>
  <summary>15. In Scala, can we access a sequence element with <code>list(index)</code>?</summary>

  > Yes. In Scala, `list(index)` is the usual way to access an element from many collections.
  >
  > For a Java developer, it is similar to `list.get(index)`.
</details>

<details>
  <summary>16. Why does <code>list(index)</code> work in Scala?</summary>

  > Because `list(index)` is syntax sugar for `list.apply(index)`.
  >
  > If a type defines an `apply(...)` method, Scala lets you call it like a function.
</details>

<details>
  <summary>17. Does <code>point(axis)</code> work only because we defined <code>apply</code> in <code>Point</code>?</summary>

  > Yes. `point(axis)` works because `Point` defines `def apply(i: Int): Double`.
  >
  > Without that method, we would need a more explicit form such as `point.coords(axis)`.
</details>

<details>
  <summary>18. Does <code>sorted(mid)</code> also depend on our custom <code>Point.apply</code>?</summary>

  > No. `sorted(mid)` works because Scala sequences already provide their own `apply(index)` method.
  >
  > So `sorted(mid)` comes from the Scala collections library, while `point(axis)` comes from the custom method we wrote in `Point`.
</details>

<details>
  <summary>19. What do <code>sorted.take(mid)</code> and <code>sorted.drop(mid + 1)</code> mean?</summary>

  > `sorted.take(mid)` returns all elements before the middle index.
  >
  > `sorted.drop(mid + 1)` skips the middle element and returns all elements after it. In this KD-tree build, the middle element becomes the current node, the `take(...)` part becomes the left subtree, and the `drop(...)` part becomes the right subtree.
</details>

<details>
  <summary>20. What does <code>case node @ KDTree.Node(p, axis, left, right)</code> mean?</summary>

  > It matches a `KDTree.Node`, extracts its fields into `p`, `axis`, `left`, and `right`, and also keeps the whole matched node in the variable `node`.
  >
  > The `@` means "bind the whole matched value to a name while also unpacking it". For a Java developer, it is similar to keeping the original object reference and also reading its fields into local variables.
  >
  > In this case, the code could use `tree`, but `node` is clearer because it makes it explicit that this branch already matched a `Node`.
</details>

<details>
  <summary>21. What does <code>node.copy(...)</code> mean?</summary>

  > `copy` is a method that Scala automatically gives to a case class.
  >
  > It creates a new object based on the existing one, changing only the fields you specify. In this code, `node.copy(left = ...)` means "create a new node with the same data, but with an updated left child". This is a common immutable style in Scala.
</details>

<details>
  <summary>22. Can Scala define one method inside another method?</summary>

  > Yes. Scala allows local methods, which means a method can be declared inside another method.
  >
  > In this code, `contains(...)` defines `loop(...)` as a helper method used only inside `contains`. For a Java developer, this is similar to a private helper method, but kept locally inside the method that uses it.
</details>

<details>
  <summary>23. How can Scala return a value without using the <code>return</code> word?</summary>

  > In Scala, the last expression in a method or block is returned automatically.
  >
  > For a Java developer, this feels unusual at first, but the idea is simple: instead of writing `return value;`, Scala usually just places the value as the final expression.
</details>

<details>
  <summary>24. What is <code>Option</code> in Scala?</summary>

  > `Option[T]` means a value may or may not exist.
  >
  > It has two cases: `Some(value)` when a value exists, and `None` when it does not. For a Java developer, it is similar to `Optional&lt;T&gt;`, but it is used much more often in normal Scala code.
</details>

<details>
  <summary>25. Do I always have to use <code>Some</code> and <code>None</code> when using <code>Option</code>?</summary>

  > Conceptually yes, because every `Option` is either `Some(...)` or `None`.
  >
  > In practice, you do not always write them manually. Sometimes you create them directly, and sometimes you just consume an `Option` returned by another method using pattern matching or helper methods like `map` and `getOrElse`.
</details>

<details>
  <summary>26. What is a pair in Scala, and how does <code>val (a, b)</code> work?</summary>

  > A pair in Scala is a tuple with two values, like `(left, right)` or `(1, "hello")`.
  >
  > `val (a, b) = pair` is tuple destructuring. It means Scala takes the first value of the pair and stores it in `a`, and the second value and stores it in `b`.
</details>

<details>
  <summary>27. What does <code>flatten</code> do when we have a sequence of <code>Option</code> values?</summary>

  > `flatten` removes the `None` values and unwraps the `Some(...)` values.
  >
  > For example, `Seq(Some(a), None, Some(b)).flatten` becomes `Seq(a, b)`.
</details>

<details>
  <summary>28. What does <code>minBy(...)</code> do in Scala?</summary>

  > `minBy(...)` returns the element that has the smallest value according to the rule you provide.
  >
  > For example, `points.minBy(p =&gt; p(searchAxis))` means "return the point whose coordinate at `searchAxis` is the smallest".
</details>

<details>
  <summary>29. Why does <code>rangeSearch</code> return <code>List[Point]</code> instead of <code>Seq[Point]</code>?</summary>

  > `List` is a specific immutable collection type, while `Seq` is a more general sequence type.
  >
  > In this method, `List` fits naturally because the recursive code builds list results using values like `Nil` and concatenation. `Seq[Point]` would also work, but `List[Point]` makes the concrete return type explicit.
</details>

<details>
  <summary>30. What does <code>until</code> mean in Scala?</summary>

  > `until` creates a range of numbers, usually starting at one value and stopping before the end value.
  >
  > For example, `0 until 5` means `0, 1, 2, 3, 4`. For a Java developer, it is similar to the index values used in `for (int i = 0; i &lt; 5; i++)`, but `until` itself is not the loop, it only creates the range.
</details>

<details>
  <summary>31. What does <code>forall(...)</code> do in Scala?</summary>

  > `forall(...)` checks whether all elements in a collection satisfy a condition.
  >
  > It returns `true` only if the condition is true for every element. For a Java developer, it is similar to looping through all elements and returning `false` as soon as one element fails the condition.
</details>

<details>
  <summary>32. What does <code>++</code> mean in Scala?</summary>

  > `++` combines two collections into one.
  >
  > For example, `a ++ b` means "take all elements from `a`, then all elements from `b`". In this project, it is used to combine list results from different parts of the tree.
</details>

<details>
  <summary>33. What is <code>Nil</code> in Scala?</summary>

  > `Nil` is the empty `List` in Scala.
  >
  > It is similar to `List()` and is commonly used as the base case in recursive code that returns a list.
</details>

<details>
  <summary>34. Why is <code>build(...)</code> more balanced than repeated <code>insert(...)</code> calls?</summary>

  > `build(...)` sorts the points and repeatedly chooses the middle element, so it creates a tree that starts balanced or close to balanced.
  >
  > `insert(...)` adds one point at a time following the current tree shape, and it does not rebalance afterward. Because of that, repeated inserts can make the tree become unbalanced.
</details>

<details>
  <summary>35. What does the <code>private</code> mean in <code>class LocationFinder private (...)</code>?</summary>

  > It makes the constructor private, not the class itself.
  >
  > That means outside code cannot call `new LocationFinder(...)` directly, and must create instances through companion object methods like `LocationFinder.empty` or `LocationFinder.fromPOIs(...)`. For a Java developer, this is similar to a private constructor plus static factory methods.
</details>

<details>
  <summary>36. What does <code>byPoint.updated(key, value)</code> do in Scala?</summary>

  > `updated(key, value)` returns a new map with that key/value inserted or replaced.
  >
  > It does not mutate the original map. For a Java developer, it is similar to creating a new map from the old one and then calling `put(...)` on the new map.
</details>

<details>
  <summary>37. What does <code>map - key</code> mean in Scala?</summary>

  > `map - key` returns a new map without that key.
  >
  > It does not mutate the original map. For a Java developer, it is similar to creating a new map from the old one and then calling `remove(key)` on the new map.
</details>

<details>
  <summary>38. Why does <code>.map(byPoint)</code> work without using <code>_</code>?</summary>

  > Because `Option.map(...)` expects a function, and `byPoint` can already be used as a function from `Point` to `POI`.
  >
  > So `.map(byPoint)` is a shorter form of `.map(point =&gt; byPoint(point))`. Using `_` here would be possible, but unnecessary.
</details>

<details>
  <summary>39. What does <code>::</code> mean in Scala lists?</summary>

  > `x :: list` means "add `x` to the front of the list".
  >
  > It is a very common way to build lists efficiently in recursive code.
</details>

<details>
  <summary>40. Why do we call <code>acc.reverse</code> at the end of a recursive list accumulator?</summary>

  > Because values were added to the front of the list with <code>::</code>, so the accumulator is built in reverse order.
  >
  > Calling <code>reverse</code> at the end restores the expected final order.
</details>

<details>
  <summary>41. What does <code>a -&gt; b</code> mean in Scala?</summary>

  > `a -&gt; b` creates a pair, usually used as a key/value entry.
  >
  > For example, `point -&gt; poi` means `(point, poi)`, which is useful when building maps.
</details>

<details>
  <summary>42. What does <code>toMap</code> do in Scala?</summary>

  > `toMap` converts a collection of pairs into a map.
  >
  > For example, `Seq(point1 -&gt; poi1, point2 -&gt; poi2).toMap` becomes a `Map[Point, POI]`.
</details>

<details>
  <summary>43. When should I use <code>class</code>, <code>case class</code>, and <code>object</code> in Scala?</summary>

  > Use `class` for a normal type with instances, `case class` for a data-focused immutable type, and `object` for a singleton with shared helpers or factory methods.
  >
  > For a Java developer, a `class` is like a normal Java class, a `case class` is similar to a data-oriented record, and an `object` is similar to a singleton that often plays the role of static members.
</details>

<details>
  <summary>44. Can I have an <code>object</code> without a matching <code>class</code> or <code>case class</code>?</summary>

  > Yes. An `object` can exist by itself and does not need a matching type with the same name.
  >
  > In that case, it is simply a singleton value with shared methods or constants.
</details>

<details>
  <summary>45. What is the difference between <code>val</code>, <code>var</code>, <code>def</code>, and <code>lazy val</code> in Scala?</summary>

  > Use `val` for an immutable value, `var` for a mutable variable, `def` for a method/computation, and `lazy val` for a value computed only on first use.
  >
  > As a rule of thumb, use `val` by default, `def` when it is really behavior, `lazy val` for delayed initialization, and `var` only when reassignment is truly needed.
</details>
