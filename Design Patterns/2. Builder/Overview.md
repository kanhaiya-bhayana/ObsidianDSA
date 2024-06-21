##### When construction gets a little bit too complicated

- Some objects are simple and can be created in a single constructor call.
- Other objects require a lot of ceremony to create
- Having an object with 10 constructor arguments is not productive
- Instead , opt for piecewise construction
- Builder provides an API for constructing an object step-by-step

Builder | When piecewise object construction is complicated, provide an API for doing it succinctly.
