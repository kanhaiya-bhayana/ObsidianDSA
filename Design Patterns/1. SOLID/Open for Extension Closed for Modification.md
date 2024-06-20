
```cs
namespace DesignPatterns.Solid
{
    public enum Color
    {
        Red, Green, Blue
    }

    public enum Size
    {
        Small, Medium, Large, Yuge
    }
    public class OpenForExtensionClosedForModification
    {
    }

    public class Product
    {
        public string Name;
        public Color Color;
        public Size Size;

        public Product(string name, Color color, Size size)
        {
            if(name == null) throw new ArgumentNullException(paramName: nameof(name));
            Name = name;
            Color = color;
            Size = size;
        }
    }


    public class ProductFilter
    {
        public IEnumerable<Product> FilterBySize(IEnumerable<Product> products, Size size)
        {
            foreach (Product p in products)
            {
                if (p.Size == size)
                {
                    yield return p;
                }
            }
        }

        public IEnumerable<Product> FilterByColor(IEnumerable<Product> products, Color color)
        {
            foreach (Product p in products)
            {
                if (p.Color == color)
                {
                    yield return p;
                }
            }
        }

        public IEnumerable<Product> FilterBySizeAndColor(IEnumerable<Product> products, Size size, Color color)
        {
            foreach (Product p in products)
            {
                if (p.Size == size && p.Color == color)
                {
                    yield return p;
                }
            }
        }
    }


    public class Demo
    {
        static void Main(string[] args)
        {
            var apple = new Product("Apple", Color.Green, Size.Small);
            var tree = new Product("Tree", Color.Green, Size.Large);
            var house = new Product("House", Color.Blue, Size.Large);

            Product[] products = {apple, tree, house};

            var pf = new ProductFilter();
            Console.WriteLine("Products:");

            foreach (var p in pf.FilterByColor(products, Color.Blue))
            {
                Console.WriteLine($" - {p.Name} is {p.Color}");
            }
        }
    }
}

```


The above implementation is breaking the open closed principle, because the open closed principle states that classes should be open for extension, which means it should be possible to extend the product filter.

It should be possible to make new filters, but they should be closed for modification, which means nobody should be going back into the filter and actually editing the code, which is already there because we can assume that the filter might have already been shipped to a customer.

#### Now how can we extend things without actually going back and changing their bodies?

> The answer is of course, inheritance.



```cs
namespace DesignPatterns.Solid
{
    public enum Color
    {
        Red, Green, Blue
    }

    public enum Size
    {
        Small, Medium, Large, Yuge
    }
    public class OpenForExtensionClosedForModification
    {
    }

    public class Product
    {
        public string Name;
        public Color Color;
        public Size Size;

        public Product(string name, Color color, Size size)
        {
            if (name == null) throw new ArgumentNullException(paramName: nameof(name));
            Name = name;
            Color = color;
            Size = size;
        }
    }


    public interface ISpecification<T>
    {
        bool IsSatisfied(T t);
    }

    public interface IFilter<T>
    {
        IEnumerable<T> Filter(IEnumerable<T> items, ISpecification<T> spec);
    }

    public class ColorSpecification : ISpecification<Product>
    {
        private Color _color;

        public ColorSpecification(Color color)
        {
            _color = color;
        }

        public bool IsSatisfied(Product t)
        {
            return t.Color == _color;
        }
    }

    public class SizeSpecification : ISpecification<Product>
    {
        private Size _size;
        public SizeSpecification(Size size)
        {
            _size = size;
        }

        public bool IsSatisfied(Product t)
        {
            return t.Size == _size;
        }
    }

    public class AndSpecification<T> : ISpecification<T>
    {
        private ISpecification<T> first, second;

        public AndSpecification(ISpecification<T> first, ISpecification<T> second)
        {
            this.first = first ?? throw new ArgumentNullException(nameof(first));
            this.second = second ?? throw new ArgumentNullException(nameof(second));
        }
        public bool IsSatisfied(T t)
        {
            return first.IsSatisfied(t) && second.IsSatisfied(t);
        }
    }

    public class ProductFilter : IFilter<Product>
    {
        public IEnumerable<Product> Filter(IEnumerable<Product> items, ISpecification<Product> spec)
        {
            foreach (var i in items)
            {
                if (spec.IsSatisfied(i))
                {
                    yield return i;
                }
            }
        }
    }

    public class Demo
    {
        static void Main(string[] args)
        {
            var apple = new Product("Apple", Color.Green, Size.Small);
            var tree = new Product("Tree", Color.Green, Size.Large);
            var house = new Product("House", Color.Blue, Size.Large);

            Product[] products = {apple, tree, house};

            var bf = new ProductFilter();
            Console.WriteLine("Products:");

            foreach (var p in bf.Filter(products, new AndSpecification<Product>(new ColorSpecification(Color.Green), new SizeSpecification(Size.Large))))
            {
                Console.WriteLine($" - {p.Name} is {p.Color} and {p.Size}");
            }            
        }
    }
}

```


The Open-Closed Principle states that components of a system, including subsystems, should be open for extension but closed for modification.

This means you should be able to extend the functionality of a component, such as a filter, without altering its existing code. Instead of modifying the original filter (e.g., "BetaFilter"), you should create new classes that implement the required specifications and integrate these new classes with the existing system.

By doing this, you avoid the need to modify and redeploy the original filter. Instead, you can deploy additional modules that enhance functionality while maintaining the stability and integrity of the already deployed components.

This is the essence of the Open-Closed Principle: extending functionality through new modules rather than modifying existing, stable components.