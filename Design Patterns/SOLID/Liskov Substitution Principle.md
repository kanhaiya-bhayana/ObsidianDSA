- The Liskov Substitution Principle (LSP) is one of the five SOLID principles of object-oriented programming. 
- It states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In other words, a subclass should be able to be used interchangeably with its parent class without causing unexpected behavior.


Here’s an example in C# that demonstrates the Liskov Substitution Principle:

```cs
class Shape
{
    public virtual void Draw()
    {
        Console.WriteLine("Drawing a shape");
    }
}

class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle");
    }
}

class Square : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a square");
    }
}

class Demo
{
    static void DrawShape(Shape shape)
    {
        shape.Draw();
    }

    static void Main(string[] args)
    {
        Shape shape1 = new Circle();
        Shape shape2 = new Square();

        DrawShape(shape1); // Outputs: Drawing a circle
        DrawShape(shape2); // Outputs: Drawing a square
    }
}
```