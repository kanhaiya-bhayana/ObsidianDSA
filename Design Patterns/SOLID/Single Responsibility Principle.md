
- Single responsibility principle is that a typical class is responsible for one thing and has one reason to change.

```cs

namespace DesignPatterns
{
    public class Journal
    {
        private readonly List<string> entries = new List<string>();
        private static int count = 0;

        public int AddEntry(string text)
        {
            entries.Add($"{++count}: {text}");
            return count;
        }

        public void RemoveEntry(int index)
        {
            entries.RemoveAt(index);
        }

        public override string ToString()
        {
            return string.Join(Environment.NewLine, entries);
        }

        public void save(string filename)
        {
            File.WriteAllText(filename, ToString());
        }

        public static Journal Load(string filename)
        {
            return new Journal();
        }

        public void Load(Uri uri)
        {
        }

    }

    public class Demo
    {
        static void Main(string[] args)
        {
            var j = new Journal();
            j.AddEntry("I cried today");
            j.AddEntry("I ate a bug");
            Console.WriteLine(j);
        }
    }
}
```

- Now the problem here and the reason why we are violating the single responsibility principle is that we're adding too much responsibility to the journal class. 
- You can see that the Journal is now responsible not only for keeping the entries, but also managing all of this persistence.



#### Solution

```cs

using System.Diagnostics;

namespace DesignPatterns
{
    public class Journal
    {
        private readonly List<string> entries = new List<string>();
        private static int count = 0;

        public int AddEntry(string text)
        {
            entries.Add($"{++count}: {text}");
            return count;
        }

        public void RemoveEntry(int index)
        {
            entries.RemoveAt(index);
        }

        public override string ToString()
        {
            return string.Join(Environment.NewLine, entries);
        }        
    }

    public class PErsistence
    {
        public void SaveToFile(Journal j, string filename, bool overwrite = false)
        {
            if (overwrite || !File.Exists(filename))
            {
                File.WriteAllText(filename, j.ToString());
            }
        }
    }

    public class Demo
    {
        static void Main(string[] args)
        {
            var j = new Journal();
            j.AddEntry("I cried today");
            j.AddEntry("I ate a bug");
            Console.WriteLine(j);

            var p = new PErsistence();
            var filename = @"C:\Users\Kanhaiya.Bhayana\source\repos\Cloud\DesignPatterns\DesignPatterns.Solid\temp\journal.txt";
            p.SaveToFile(j, filename, overwrite: true);
        }
    }
}
```

