

```html
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Java Memory Management Architecture</title>

    <style>

        body {

            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;

            margin: 0;

            padding: 20px;

            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);

            color: #333;

            min-height: 100vh;

        }

        .container {

            max-width: 1200px;

            margin: 0 auto;

            background: rgba(255, 255, 255, 0.95);

            border-radius: 20px;

            padding: 30px;

            box-shadow: 0 15px 35px rgba(0,0,0,0.1);

            backdrop-filter: blur(10px);

        }

        h1 {

            text-align: center;

            color: #2c3e50;

            margin-bottom: 30px;

            font-size: 2.5em;

            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);

        }

        .memory-diagram {

            display: flex;

            gap: 30px;

            margin: 40px 0;

            min-height: 600px;

        }

        .memory-section {

            flex: 1;

            border-radius: 15px;

            padding: 25px;

            position: relative;

            box-shadow: 0 8px 25px rgba(0,0,0,0.1);

            transition: transform 0.3s ease;

        }

        .memory-section:hover {

            transform: translateY(-5px);

        }

        .stack-memory {

            background: linear-gradient(145deg, #ff9a9e 0%, #fecfef 100%);

            border: 3px solid #e74c3c;

        }

        .heap-memory {

            background: linear-gradient(145deg, #a8edea 0%, #fed6e3 100%);

            border: 3px solid #3498db;

        }

        .section-title {

            font-size: 1.8em;

            font-weight: bold;

            text-align: center;

            margin-bottom: 20px;

            padding: 10px;

            border-radius: 8px;

            color: white;

            text-shadow: 1px 1px 2px rgba(0,0,0,0.3);

        }

        .stack-title {

            background: linear-gradient(45deg, #e74c3c, #c0392b);

        }

        .heap-title {

            background: linear-gradient(45deg, #3498db, #2980b9);

        }

        .memory-item {

            background: rgba(255, 255, 255, 0.9);

            margin: 12px 0;

            padding: 15px;

            border-radius: 10px;

            border-left: 4px solid;

            box-shadow: 0 4px 10px rgba(0,0,0,0.1);

            transition: all 0.3s ease;

        }

        .memory-item:hover {

            transform: translateX(5px);

            box-shadow: 0 6px 15px rgba(0,0,0,0.15);

        }

        .primitive {

            border-left-color: #f39c12;

        }

        .reference {

            border-left-color: #9b59b6;

        }

        .object {

            border-left-color: #27ae60;

        }

        .class-data {

            border-left-color: #e67e22;

        }

        .arrow {

            position: absolute;

            color: #e74c3c;

            font-size: 24px;

            font-weight: bold;

            animation: pulse 2s infinite;

        }

        @keyframes pulse {

            0%, 100% { opacity: 1; }

            50% { opacity: 0.5; }

        }

        .code-example {

            background: #2c3e50;

            color: #ecf0f1;

            padding: 20px;

            border-radius: 10px;

            margin: 30px 0;

            font-family: 'Courier New', monospace;

            overflow-x: auto;

            box-shadow: inset 0 4px 8px rgba(0,0,0,0.3);

        }

        .explanation {

            background: linear-gradient(135deg, #74b9ff, #0984e3);

            color: white;

            padding: 25px;

            border-radius: 15px;

            margin: 30px 0;

            box-shadow: 0 8px 25px rgba(0,0,0,0.1);

        }

        .explanation h3 {

            margin-top: 0;

            font-size: 1.5em;

        }

        .legend {

            display: flex;

            justify-content: space-around;

            margin: 30px 0;

            flex-wrap: wrap;

            gap: 15px;

        }

        .legend-item {

            display: flex;

            align-items: center;

            gap: 10px;

            background: rgba(255, 255, 255, 0.9);

            padding: 10px 15px;

            border-radius: 8px;

            box-shadow: 0 4px 10px rgba(0,0,0,0.1);

        }

        .color-box {

            width: 20px;

            height: 20px;

            border-radius: 4px;

        }

        .method-area {

            background: linear-gradient(145deg, #fdcb6e, #e17055);

            border: 3px solid #d63031;

            margin-top: 20px;

        }

        .method-title {

            background: linear-gradient(45deg, #d63031, #74b9ff);

        }

    </style>

</head>

<body>

    <div class="container">

        <h1>🧠 Java Memory Management Architecture</h1>

        <div class="code-example">

<pre>

public class Student {

    private String name;        // Instance variable

    private int age;           // Instance variable

    private static int count = 0;  // Static variable

    public Student(String name, int age) {

        this.name = name;

        this.age = age;

        count++;

    }

    public void displayInfo() {

        String message = "Student Info";  // Local variable

        int tempAge = this.age;          // Local variable

        System.out.println(message + ": " + name + ", " + tempAge);

    }

}

  

public class Main {

    public static void main(String[] args) {

        int num = 42;                    // Local primitive

        Student student1 = new Student("Alice", 20);  // Reference variable

        Student student2 = new Student("Bob", 22);    // Reference variable

    }

}

</pre>

        </div>

        <div class="memory-diagram">

            <!-- Stack Memory -->

            <div class="memory-section stack-memory">

                <div class="section-title stack-title">🥞 STACK MEMORY</div>

                <div class="memory-item primitive">

                    <strong>main() method frame:</strong><br>

                    • int num = 42<br>

                    • Student student1 = 0x1001 ➡️<br>

                    • Student student2 = 0x1002 ➡️

                </div>

                <div class="memory-item primitive">

                    <strong>displayInfo() method frame:</strong><br>

                    • String message = 0x2001 ➡️<br>

                    • int tempAge = 20<br>

                    • this = 0x1001 ➡️

                </div>

                <div class="memory-item reference">

                    <strong>Constructor frame:</strong><br>

                    • String name = 0x3001 ➡️<br>

                    • int age = 20<br>

                    • this = 0x1001 ➡️

                </div>

                <div style="margin-top: 20px; padding: 15px; background: rgba(231, 76, 60, 0.1); border-radius: 8px;">

                    <strong>Stack stores:</strong><br>

                    • Method call frames<br>

                    • Local variables<br>

                    • Method parameters<br>

                    • Reference variables (addresses)<br>

                    • Primitive values

                </div>

            </div>

            <!-- Heap Memory -->

            <div class="memory-section heap-memory">

                <div class="section-title heap-title">🏔️ HEAP MEMORY</div>

                <div class="memory-item object">

                    <strong>Student Object #1 (0x1001):</strong><br>

                    • name = 0x3001 ➡️ "Alice"<br>

                    • age = 20<br>

                    • Class metadata pointer ➡️

                </div>

                <div class="memory-item object">

                    <strong>Student Object #2 (0x1002):</strong><br>

                    • name = 0x3002 ➡️ "Bob"<br>

                    • age = 22<br>

                    • Class metadata pointer ➡️

                </div>

                <div class="memory-item object">

                    <strong>String Objects:</strong><br>

                    • 0x2001: "Student Info"<br>

                    • 0x3001: "Alice"<br>

                    • 0x3002: "Bob"

                </div>

                <div style="margin-top: 20px; padding: 15px; background: rgba(52, 152, 219, 0.1); border-radius: 8px;">

                    <strong>Heap stores:</strong><br>

                    • All objects<br>

                    • Instance variables<br>

                    • Arrays<br>

                    • String literals<br>

                    • Object metadata

                </div>

                <!-- Method Area -->

                <div class="memory-section method-area">

                    <div class="section-title method-title">📚 METHOD AREA</div>

                    <div class="memory-item class-data">

                        <strong>Student.class:</strong><br>

                        • Class metadata<br>

                        • Method bytecode<br>

                        • static int count = 2<br>

                        • Constant pool

                    </div>

                    <div class="memory-item class-data">

                        <strong>String.class, Object.class:</strong><br>

                        • System class metadata<br>

                        • Method implementations

                    </div>

                </div>

            </div>

        </div>

        <div class="legend">

            <div class="legend-item">

                <div class="color-box" style="background: #f39c12;"></div>

                <span>Primitive Variables</span>

            </div>

            <div class="legend-item">

                <div class="color-box" style="background: #9b59b6;"></div>

                <span>Reference Variables</span>

            </div>

            <div class="legend-item">

                <div class="color-box" style="background: #27ae60;"></div>

                <span>Objects</span>

            </div>

            <div class="legend-item">

                <div class="color-box" style="background: #e67e22;"></div>

                <span>Class Metadata</span>

            </div>

        </div>

        <div class="explanation">

            <h3>🔍 Key Memory Management Concepts:</h3>

            <p><strong>Stack Memory:</strong> Each thread has its own stack. When a method is called, a new frame is pushed onto the stack containing local variables, method parameters, and return addresses. Primitive variables store actual values, while reference variables store memory addresses pointing to objects in the heap.</p>

            <p><strong>Heap Memory:</strong> Shared among all threads, the heap stores all objects and their instance variables. Objects are allocated here during runtime using the 'new' keyword. The heap is divided into Young Generation (Eden, Survivor spaces) and Old Generation for garbage collection optimization.</p>

            <p><strong>Method Area:</strong> Stores class-level data including class metadata, method bytecode, static variables, and the constant pool. This area is shared among all threads and loaded when classes are first referenced.</p>

            <p><strong>Memory Flow:</strong> When you create an object, the reference variable goes on the stack, but the actual object is allocated in the heap. The reference variable contains the memory address where the object is stored in the heap.</p>

        </div>

    </div>

</body>

</html>
```