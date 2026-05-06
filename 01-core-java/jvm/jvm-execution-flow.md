*** What happens when we run a java code:

-> When we run a java program, the execution happens in multiple steps:
1) Compilation Phase:
    - The .java file is compiled by the Java compiler javac.
    - It converts the source code to bytecode (.class file)
    - This bytecode is platform independent.
2) Class Loading Phase:
    - The ClassLoader (part of the JVM) loads the .class file into memory.
    - It loads the data into the Method Area.
3) ByteCode Verification
    - JVM verifies the bytecode to ensure:
        a) It follows correct format
        b) It is secure and does not violate any memory access rules.
4) Execution Phase:
    - The Execution Engine executes the bytecode using:
        a) Interpreter: Executes code line by line.
        b) JIT Compiler: converts the frequently executed code into native machine code for better performance.
5) Memory Usage:
    - JVM uses different memory areas:
        a) Heap: stores Objects.
        b) Stack: stores method calls and local variables.
        c) Method Area: stores class metadata.

-> JVM is a part of JRE and JRE is a part of the JDK.
-> Since Bytecode runs on JVM, Java achieves platform independnece.

