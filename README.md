r# dasCHash
C# -> daScript converter

Main idea here is that the converter does all the tedious syntax conversion, and then code is copy-pasted on as-needed basis into the ported project.
It should mostly compile, with minor fixes required to address namespaces, using directives, as well as some other language specific and project specific constructs.

Go from C# code

```
using static System.Console;
using System;
using System.Collections.Generic;

namespace HelloWorld
{
    // enum )
    struct TestStruct
    {
        public int x;
    };

    class Foo
    {
        static int count = 13;

        public int a = 13;
        private int b = 13;

        public Foo() { }
        public Foo(int A) { a = A; }

        public void bar()
        {
            a = 123;
        }

        public void bar(in int t)
        {
            a = t;
        }

        // static method
        static void farfar()
        {
            Console.WriteLine("static method");
        }

        static public int next()
        {
            return count++;
        }
    }

    // struct - value type
    struct SFoo
    {
        public int a;
    }

    class HelloProgram
    {
        int count = 13;

        static void _main(string[] args)
        {
            WriteLine("Hello, World!");
            Console.WriteLine("Hello, World!");
            System.Console.WriteLine("Hello, World!");

            Write("Hello, World!\\n");
            Console.Write("Hello, World!\\n");
            System.Console.Write("Hello, World!\\n");

            Write("1", "2", "3");
        }

        static void ifThenElse(bool cond)
        {
            if (cond)
                Console.WriteLine("if no-block");
            if (cond)
            {
                Console.WriteLine("if block");
            }
            if (cond)
                Console.WriteLine("if no block");
            else
                Console.WriteLine("else no block");
            if (cond)
            {
                Console.WriteLine("if block");
            }
            else
                Console.WriteLine("else no block");
            if (cond)
                Console.WriteLine("if no block");
            else
            {
                Console.WriteLine("else block");
            }
            if (cond)
            {
                Console.WriteLine("if block");
            }
            else
            {
                Console.WriteLine("else block");
            }
            if (cond)
                if (cond)
                    Console.WriteLine("if cond if cond");
            if (cond)
                Console.WriteLine("cond1");
            else if (cond)
                Console.WriteLine("cond2");
            else
                Console.WriteLine("cond-else");
        }

        static void varDecl()
        {
            float t;
            int x = 1;
            int y = 2, z = 3;
        }

        static void allOperators(int a, float b)
        {
            // assignment
            a = a;
            // op1
            int a1 = +a;
            int a2 = -a;
            int a3 = ~a;
            bool a4 = !(a == 0);
            // op2
            float c = a + b;
            float d = a - b;
            float e = a * b;
            float f = a / b;
            float g = a % b;
            // op2 bool
            bool t1 = a == a;
            bool t2 = a != a;
            bool t3 = a <= a;
            bool t4 = a >= a;
            bool t5 = a > a;
            bool t6 = a < a;
            // more bool
            bool tt1 = t1 && t2;
            bool tt2 = t2 || t3;
            bool tt3 = t3 ^ t4;
            // binary
            int b1 = a & a;
            int b2 = a | a;
            int b3 = a ^ a;
            int b4 = a << 1;
            int b5 = a >> 1;
            // assignmetn
            a <<= 1;
            a >>= 1;
            a |= a;
            a &= a;
            a ^= a;
            Console.WriteLine(c);
        }

        static void allCasts()
        {
            sbyte i8 = 0;
            byte u8 = 0;
            Int16 i16 = 0;
            UInt16 u16 = 0;
            Int32 i32 = 0;
            UInt32 u32 = 0;
            int i = 0;
            uint u = 0;
            Int64 i64 = 0;
            UInt64 u64 = 0;
            float f = 0;
            double d = 0;

            var i8i8 = i8 + i8;     // int
            var i8u8 = i8 + u8;     // int
            var i16i16 = i16 + i16; // int
            var i16u16 = i16 + u16; // int
            var ii = i + i;         // int
            var uu = u + u;         // uint
            var ui = i + u;         // long
            var i32i32 = i32 + i32; // int
            var i32u32 = i32 + u32; // long
            var i64i64 = i64 + i64; // long
            var u64u64 = u64 + u64; // ulong
            // var i64u64 = i64 + u64;  // ambiguity, does not compile
            var fd = f + d;   // goes to double

            // casts
            double dd = 10.0d;
            float ff = 10.0f;
            double df = 10.0f;
            int ii8 = i8;
            int iu8 = u8;
            Int64 i64i = i;
            float fi = i;
        }

        static void whileLoop()
        {
            var a = 1;
            while (a < 5)
                a++;
            while (a != 10)
            {
                ++a;
                --a;
                a--;
            }
            while (a == a)
            {
                if (a != a)
                    break;
                if (a == a)
                    continue;
            }
        }

        static void forLoop()
        {
            for (var i = 0; i < 10; ++i)
                Console.WriteLine(i);
            for (int i = 0, j = 12; i + j < 100; i++, j--)
                Console.WriteLine($"i={i} j={j}");
            for (int i = 0; i < 10; ++i)
            {
                if (i == 5)
                    break;
                Console.WriteLine(i);
            }

            for (var i = 1; i != 3; i += 2) // not it
                Console.WriteLine(i);
            for (var i = 1; i != 3; i = i + 2)    // not it
                Console.WriteLine(i);

            for (var i = 1; i != 3; ++i)    // for i in range(1,3)
                Console.WriteLine(i);
            for (var i = 1; 3 != i; ++i)    // for i in range(1,3)
                Console.WriteLine(i);
            for (var i = 1; i < 3; i++)    // for i in range(1,3)
                Console.WriteLine(i);
            for (var i = 1; 3 > i; i++)    // for i in range(1,3)
                Console.WriteLine(i);
            for (var i = 1; i != 3; i += 1)    // for i in range(1,3)
                Console.WriteLine(i);
            for (var i = 1; i != 3; i = i + 1)    // for i in range(1,3)
                Console.WriteLine(i);
            for (var i = 1; i != 3; i = 1 + i)    // for i in range(1,3)
                Console.WriteLine(i);
        }

        static void doWhileLoop()
        {
            var a = 1;
            do
                ++a;
            while (a != 10);
            do
            {
                --a;
                if (a == 5)
                    break;
            }
            while (a != 0);
        }

        static void arrayTypes()
        {
            int[] b = new int[10];
            var s = 10;
            int[] bs = new int[s];
            int[][] bb = new int[20][];
            int[,] c = new int[10, 20];
            int[] d = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
            int[] dd = new int[] { 0, 1, 2, 4, 5 };
        }

        static void forEachLoop()
        {
            int[] b = { 1, 2, 3, 4 };
            foreach (var t in b)
                WriteLine(t);
        }

        static void switchCase()
        {
            var i = 13;
            switch (i)
            {
                case 0:
                    break;
                case 1:
                case 2:
                    Console.WriteLine("12");
                    break;
                default:
                    Console.WriteLine("anything");
                    break;
            }
            Console.WriteLine("and thats that");
            switch (i)
            {
                case 1: i++; break;
                case 2: i--; break;
            }
            // and the crazy case
            switch (i)
            {
                case 1: if (i == 0) break; else { i++; break; }
                case 2: i--; break;
            }
            // default in the middle
            switch (i)
            {
                case 1: i++; break;
                default: break;
                case 2: i--; break;
            }
        }

        private static Dictionary<int, string> POTToText = new Dictionary<int, string>
        {
            {14, "16K"},            {15, "32K"},            {16, "64K"},            {17, "128K"},
            {18, "256K"},           {19, "512K"},           {20, "1M"},             {21, "2M"},
            {22, "4M"},             {23, "8M"},             {24, "16M"},            {25, "32M"},
            {26, "64M"},            {27, "128M"},           {28, "256M"},           {29, "512M"},
            {30, "1B"},             {31, "2B"},             {32, "4B"},             {33, "8B"},
            {34, "16B"},            {35, "32B"},            {36, "64B"},            {37, "128B"},
            {38, "256B"},           {39, "512B"},           {40, "1024B"},
        };

        void objectInit()
        {
            System.DateTime epochStart = new System.DateTime(1970, 1, 1, 0, 0, 0, System.DateTimeKind.Utc);
            Foo t = new Foo();
            Foo tt = new Foo(1);
            t.bar();
            t.bar(1);
        }

        void structures()
        {
            var f1 = new SFoo();
            var f2 = new SFoo() { a = 1 };
            var f3 = f1; // clone!
            f1 = default(SFoo);
            f3 = f2;
            Console.WriteLine($"f1 = {f1} f2 = {f2.ToString()} f3 = {f3.a}");
            Console.WriteLine(f1.ToString());
            var f2Str = f2.ToString();
            Console.WriteLine(f2Str);
        }

        void lists()
        {
            var a = new List<float>()
            {
                1, 2, 3, 4, 5, 6
            };
            a.Add(7f);
            a.RemoveAt(0);
            a.Remove(2.1f);
            a.Insert(0, 123);
            a.RemoveRange(0, 1);
            var b = a[0];
            Console.WriteLine($"a = {a.ToString()} size: {a.Count}");
            Console.WriteLine($"b = {b}");
            a.Clear();
            var sum = 0f;
            foreach (var i in a)
            {
                sum += i;

                Console.WriteLine($"i = {i}");
            }
            Console.WriteLine($"sum = {sum} has 6 {a.Contains(6)}, 7 index {a.IndexOf(7)}");
            a.Sort();

            var c = new List<Foo>() { new Foo() { a = 1 }, new Foo() { a = 2 } };
            c.Add(new Foo() { a = 3 });
            Console.WriteLine($"c = {c}");

            var d = new List<SFoo>() { new SFoo() { a = 1 }, new SFoo() { a = 2 } };
            d.Add(new SFoo() { a = 3 });
            Console.WriteLine($"d = {d.ToString()}");
        }

        void conditional_expression()
        {
            var a = 1;
            var b = (a > 0 ? 1 : 0) * (a < 0 ? 1 : 0);
        }

        void static_methods()
        {
            var i = Foo.next();
            Console.WriteLine($"i = {i}");
        }

        static void m_in(in int a)
        {
            Console.WriteLine(a);
        }

        static void m_out(out int a)
        {
            a = 13;
        }

        static void testInOut()
        {
            var a = 0;
            m_out(out a);
            m_in(a);
        }
    }

    public class DefaultPerson
    {
        public string FirstName { get; set; }
    }

    public class DefaultPersonWithInit
    {
        public string FirstName { get; set; } = "blahblah";
    }

    public class Person
    {
        public string FirstName
        {
            get { return _firstName; }
            set { _firstName = value; }
        }
        private string _firstName;
    }

    public class PersonWithArrows
    {
        // new style operator
        public string FirstName
        {
            get => _firstName;
            set => _firstName = value;
        }
        private string _firstName;
    }

    class Goo
    {
        static void testProperty()
        {
            var p = new Person();
            // set first name
            p.FirstName = "Test";
            // concat
            p.FirstName += "Me";
            if (p.FirstName == "TestMe") { Console.WriteLine(p.FirstName); }
        }
    }

    abstract class Shape
    {
        public abstract double Area
        {
            get;
            set;
        }
    }

    class Square : Shape
    {
        public double side;

        //constructor
        public Square(double s) => side = s;

        public override double Area
        {
            get => side * side;
            set => side = System.Math.Sqrt(value);
        }
    }

    class Cube : Shape
    {
        public double side;

        //constructor
        public Cube(double s) => side = s;

        public override double Area
        {
            get => 6 * side * side;
            set => side = System.Math.Sqrt(value / 6);
        }
    }

    public class StaticPerson
    {
        public static string FirstName
        {
            get { return _firstName; }
            set { _firstName = value; }
        }
        private static string _firstName;

        static void foo()
        {
            StaticPerson.FirstName = "foo";
            StaticPerson.FirstName += "bar";
            Console.WriteLine(StaticPerson.FirstName);
        }
    }
    enum Season
    {
        Spring,
        Summer,
        Autumn,
        Winter
    }

    public enum ErrorCode : ushort
    {
        None = 0,
        Unknown = 1,
        ConnectionLost = 100,
        OutlierReading = 200
    }
}

```

to daScript equivalent.

```
// using System.Console
// using System
// using System.Collections.Generic

// namespaced HelloWorld
// enum )
struct TestStruct
        x : int


class Foo
        static count : int = 13

        a : int = 13

        private b : int = 13

        def Foo
                pass

        def Foo (var A : int)
                a = A

        def bar : void
                a = 123

        def bar`int (t : int) : void
                a = t
        // static method
        def static farfar : void
                print("static method\n")

        def static next : int
                return count++

// struct - value type
struct SFoo
        a : int


class HelloProgram
        count : int = 13

        def static _main (var args : array<string> /*string[]*/) : void
                print("Hello, World!\n")
                print("Hello, World!\n")
                print("Hello, World!\n")

                print("Hello, World!\n")
                print("Hello, World!\n")
                print("Hello, World!\n")

                print("1 2 3")

        def static ifThenElse (var cond : bool) : void
                if cond
                        print("if no-block\n")
                if cond
                        print("if block\n")
                if cond
                        print("if no block\n")
                else
                        print("else no block\n")
                if cond
                        print("if block\n")
                else
                        print("else no block\n")
                if cond
                        print("if no block\n")
                else
                        print("else block\n")
                if cond
                        print("if block\n")
                else
                        print("else block\n")
                if cond
                        if cond
                                print("if cond if cond\n")
                if cond
                        print("cond1\n")
                elif cond
                        print("cond2\n")
                else
                        print("cond-else\n")

        def static varDecl : void
                var t : float
                var x : int = 1
                var y : int = 2
                var z : int = 3

        def static allOperators (var a : int; var b : float) : void
                a = a
                // op1
                var a1 : int = (+a)
                var a2 : int = (-a)
                var a3 : int = (~a)
                var a4 : bool = (!((a == 0)))
                // op2
                var c : float = (float(a) + b)
                var d : float = (float(a) - b)
                var e : float = (float(a) * b)
                var f : float = (float(a) / b)
                var g : float = (float(a) % b)
                // op2 bool
                var t1 : bool = (a == a)
                var t2 : bool = (a != a)
                var t3 : bool = (a <= a)
                var t4 : bool = (a >= a)
                var t5 : bool = (a > a)
                var t6 : bool = (a < a)
                // more bool
                var tt1 : bool = (t1 && t2)
                var tt2 : bool = (t2 || t3)
                var tt3 : bool = (t3 ^^ t4)
                // binary
                var b1 : int = (a & a)
                var b2 : int = (a | a)
                var b3 : int = (a ^ a)
                var b4 : int = (a << 1)
                var b5 : int = (a >> 1)
                // assignmetn
                a <<= 1
                a >>= 1
                a |= a
                a &= a
                a ^= a
                print("{c}\n")

        def static allCasts : void
                var i8 : int8 = int8(0)
                var u8 : uint8 = 0u8
                var i16 : int16 = int16(0)
                var u16 : uint16 = uint16(0)
                var i32 : int = 0
                var u32 : uint = 0u
                var i : int = 0
                var u : uint = 0u
                var i64 : int64 = 0l
                var u64 : uint64 = 0ul
                var f : float = 0f
                var d : double = 0d

                var i8i8 = (int(i8) + int(i8))          // int
                var i8u8 = (int(i8) + int(u8))          // int
                var i16i16 = (int(i16) + int(i16))              // int
                var i16u16 = (int(i16) + int(u16))              // int
                var ii = (i + i)                // int
                var uu = (u + u)                // uint
                var ui = (int64(i) + int64(u))          // long
                var i32i32 = (i32 + i32)                // int
                var i32u32 = (int64(i32) + int64(u32))          // long
                var i64i64 = (i64 + i64)                // long
                var u64u64 = (u64 + u64)                // ulong
                // var i64u64 = i64 + u64;  // ambiguity, does not compile
                var fd = (double(f) + d)                // goes to double

                // casts
                var dd : double = 10.0d
                var ff : float = 10.0f
                var df : double = double(10.0f)
                var ii8 : int = int(i8)
                var iu8 : int = int(u8)
                var i64i : int64 = int64(i)
                var fi : float = float(i)

        def static whileLoop : void
                var a = 1
                while (a < 5)
                        a++
                while (a != 10)
                        ++a
                        --a
                        a--
                while (a == a)
                        if (a != a)
                                break
                        if (a == a)
                                continue

        def static forLoop : void
                for i in range(0,10)
                        print("{i}\n")
                if true // for loop
                        var i : int = 0
                        var j : int = 12
                        while ((i + j) < 100)
                                print("i={i} j={j}\n")
                                i++
                                j--
                for i in range(0,10)
                        if (i == 5)
                                break
                        print("{i}\n")

                if true // for loop
                        var i = 1
                        while (i != 3)
                                print("{i}\n")
                                i += 2
                if true // for loop
                        var i = 1
                        while (i != 3)
                                print("{i}\n")
                                i = (i + 2)

                for i in range(1,3)
                        print("{i}\n")
                for i in range(1,3)
                        print("{i}\n")
                for i in range(1,3)
                        print("{i}\n")
                for i in range(1,3)
                        print("{i}\n")
                for i in range(1,3)
                        print("{i}\n")
                for i in range(1,3)
                        print("{i}\n")
                for i in range(1,3)
                        print("{i}\n")

        def static doWhileLoop : void
                var a = 1
                var _temp_1_doWhileCond_ = true
                while _temp_1_doWhileCond_
                        ++a
                        _temp_1_doWhileCond_ = (a != 10)
                var _temp_2_doWhileCond_ = true
                while _temp_2_doWhileCond_
                        if true
                                --a
                                if (a == 5)
                                        break
                        finally
                                _temp_2_doWhileCond_ = (a != 0)

        def static arrayTypes : void
                var b : array<int> /*int[]*/ <- [{ for _temp_3_iter_ in range(10); _temp_3_iter_ }]
                var s = 10
                var bs : array<int> /*int[]*/ <- [{ for _temp_4_iter_ in range(s); _temp_4_iter_ }]
                var bb : array<array<int>> /*int[][]*/ <- newArray(20, -1)
                var c : array<array<int>> /*int[,]*/ <- newArray(10, 20)
                var d : array<int> /*int[]*/ <- [{auto 0; 1; 2; 3; 4; 5; 6; 7; 8; 9; 10}]
                var dd : array<int> /*int[]*/ <- [{int 0; 1; 2; 4; 5}]

        def static forEachLoop : void
                var b : array<int> /*int[]*/ <- [{auto 1; 2; 3; 4}]
                for t in b
                        print("{t}\n")

        def static switchCase : void
                var i = 13
                let _temp_5_switchcase_ = i
                if _temp_5_switchcase_==0
                        pass
                elif _temp_5_switchcase_==1 || _temp_5_switchcase_==2
                        print("12\n")
                else
                        print("anything\n")
                print("and thats that\n")
                let _temp_6_switchcase_ = i
                if _temp_6_switchcase_==1
                        i++
                elif _temp_6_switchcase_==2
                        i--
                // and the crazy case
                let _temp_7_switchcase_ = i
                while true
                        if _temp_7_switchcase_==1
                                if (i == 0)
                                        break
                                else
                                        i++
                                        break
                        elif _temp_7_switchcase_==2
                                i--
                                break
                        break
                // default in the middle
                let _temp_8_switchcase_ = i
                if _temp_8_switchcase_==1
                        i++
                elif _temp_8_switchcase_==2
                        i--
                else
                        pass

        static private POTToText : table<int; string>? <- new {{14 => "16K"; 15 => "32K"; 16 => "64K"; 17 => "128K"; 18 => "256K"; 19 => "512K"; 20 => "1M"; 21 => "2M"; 22 => "4M"; 23 => "8M"; 24 => "16M"; 25 => "32M"; 26 => "64M"; 27 => "128M"; 28 => "256M"; 29 => "512M"; 30 => "1B"; 31 => "2B"; 32 => "4B"; 33 => "8B"; 34 => "16B"; 35 => "32B"; 36 => "64B"; 37 => "128B"; 38 => "256B"; 39 => "512B"; 40 => "1024B"; }}

        def objectInit : void
                var epochStart : System::DateTime = [[System::DateTime(1970, 1, 1, 0, 0, 0, System.DateTimeKind.Utc)]]
                var t : Foo? <- new [[Foo()]]
                var tt : Foo? <- new [[Foo(1)]]
                t->bar()
                t->bar`int(1)

        def structures : void
                var f1 = [[SFoo()]]
                var f2 = [[SFoo() a = 1, ]]
                var f3 := f1            // clone!
                f1 := [[SFoo]]
                f3 := f2
                print("f1 = {f1} f2 = {f2} f3 = {f3.a}\n")
                print("{f1}\n")
                var f2Str = "{f2}"
                print("{f2Str}\n")

        def lists : void
                var a <- new [{float 1f; 2f; 3f; 4f; 5f; 6f; }]
                *a |> push(7f)
                *a |> erase(0)
                *a |> remove_value(2.1f)
                *a |> push(123f, 0)
                *a |> erase(0, 1)
                var b = (*a)[0]
                print("a = {*a} size: {*a |> length()}\n")
                print("b = {b}\n")
                *a |> clear()
                var sum = 0f
                for i in *a
                        sum += i

                        print("i = {i}\n")
                print("sum = {sum} has 6 {*a |> has_value(6f)}, 7 index {*a |> find_index(7f)}\n")
                *a |> sort()

                var c <- new [{Foo? new [[Foo() a = 1, ]]; new [[Foo() a = 2, ]]; }]
                *c |> push(new [[Foo() a = 3, ]])
                print("c = {c}\n")

                var d <- new [{SFoo [[SFoo() a = 1, ]]; [[SFoo() a = 2, ]]; }]
                *d |> push([[SFoo() a = 3, ]])
                print("d = {*d}\n")

        def conditional_expression : void
                var a = 1
                var b = (((a > 0) ? 1 : 0) * ((a < 0) ? 1 : 0))

        def static_methods : void
                var i = Foo`next()
                print("i = {i}\n")

        def static m_in (a : int) : void
                print("{a}\n")

        def static m_out (var a : int&) : void
                a = 13

        def static testInOut : void
                var a = 0
                HelloProgram`m_out(a)
                HelloProgram`m_in(a)


class DefaultPerson
        // property FirstName : string
        def operator . FirstName : string
                return FirstName`get()
        def FirstName`get : string
                return FirstName`Storage
        def operator . FirstName := ( value:string )
                FirstName`set(value)
        def FirstName`set ( value:string ) : void
                FirstName`Storage = value
        private FirstName`Storage : string

class DefaultPersonWithInit
        // property FirstName : string
        def operator . FirstName : string
                return FirstName`get()
        def FirstName`get : string
                return FirstName`Storage
        def operator . FirstName := ( value:string )
                FirstName`set(value)
        def FirstName`set ( value:string ) : void
                FirstName`Storage = value
        private FirstName`Storage : string = "blahblah"

class Person
        // property FirstName : string
        def operator . FirstName : string
                return FirstName`get()
        def FirstName`get : string
                return _firstName
        def operator . FirstName := ( value:string )
                FirstName`set(value)
        def FirstName`set ( value:string ) : void
                _firstName = value

        private _firstName : string


class PersonWithArrows  // new style operator
        // property FirstName : string
        def operator . FirstName : string
                return FirstName`get()
        def FirstName`get : string
                return _firstName
        def operator . FirstName := ( value:string )
                FirstName`set(value)
        def FirstName`set ( value:string ) : void
                _firstName = value

        private _firstName : string


class Goo
        def static testProperty : void
                var p <- new [[Person()]]
                // set first name
                p.FirstName := "Test"
                // concat
                p.FirstName := p.FirstName + "Me"
                if (p.FirstName == "TestMe")
                        print("{p.FirstName}\n")


class Shape
        // property Area : double
        def operator . Area : double
                return Area`get()
        def abstract Area`get : double
        def operator . Area := ( value:double )
                Area`set(value)
        def abstract Area`set ( value:double ) : void


class Square
        side : double
        //constructor
        def Square (var s : double)
                side = s

        // property Area : double
        def Area`get : double
                return (side * side)
        def Area`set ( value:double ) : void
                side = Math`Sqrt(value)


class Cube
        side : double
        //constructor
        def Cube (var s : double)
                side = s

        // property Area : double
        def Area`get : double
                return ((6d * side) * side)
        def Area`set ( value:double ) : void
                side = Math`Sqrt((value / 6d))


class StaticPerson
        // static property FirstName : string
        def static FirstName`get : string
                return _firstName
        def static FirstName`set ( value:string ) : void
                _firstName = value

        static private _firstName : string

        def static foo : void
                StaticPerson`FirstName`set("foo")
                StaticPerson`FirstName`set(StaticPerson`FirstName`get() + "bar")
                print("{StaticPerson`FirstName`get()}\n")


enum Season
        Spring
        Summer
        Autumn
        Winter

enum public ErrorCode : uint16
        None = uint16(0)
        Unknown = uint16(1)
        ConnectionLost = uint16(100)
        OutlierReading = uint16(200)
```


