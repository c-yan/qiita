# 別アセンブリのパブリックではない抽象クラスのパブリックではないオーバーロードしているメソッドを呼ぶ

なんでこんなにややこしいものを、ややこしいまま呼ばないといけないかって、手を入れずに単体テストを追加したいからですが……. 書いたやつ出てこい! (時間が無い中、新人が書いたスパゲティコードを最低限だけ直した8年前の私なんですが)

リフレクションするコードをググると `Assembly.GetExecutingAssembly` を使う例が多いですが、当然別アセンブリなので使えません. とりあえず `Assembly.LoadFrom` で動いたのでよしにしましたが、なんかダサいですね、ファイル名を渡してるし.

抽象クラスはインスタンス化できないので、派生クラスを `new` するわけですが、派生クラスにはメソッドが生えていないので、結果として抽象・派生両方の型情報を引っ張る必要があります.

`Type.GetMethod` するとき、対象が `static` メソッドでなかったり、`public` メソッドでなかったりするときは `BindingFlags` の指定が必要なようです. また、オーバーロードしている場合は、一意になるように引数の型情報を指定する必要があります. また、当然ですが、生えている抽象型の方で `GetMethod` します.

`MethodInfo.Invoke` するときは、派生型のインスタンスを渡します.

以上をまとめると以下のようなコードになります.

```Class1.cs
namespace ClassLibrary1
{
    abstract class Class1
    {
        protected string Method1(string s1)
        {
            if (s1.StartsWith("A") || s1.StartsWith("B"))
            {
                return "ABC";
            }
            return null;
        }

        protected string Method1(string s1, string s2)
        {
            if (!string.IsNullOrEmpty(s2) && s2.Length == 3)
            {
                return s2;
            }
            return Method1(s1);
        }
    }

    class Class2: Class1
    {
    }
}
```

```Program.cs
using System;
using System.Reflection;

namespace ConsoleApp1
{
    class Program
    {
        static void Main(string[] args)
        {
            var a = Assembly.LoadFrom("ClassLibrary1.dll");
            var t0 = a.GetType("ClassLibrary1.Class2");
            var o = Activator.CreateInstance(t0);
            var t1 = a.GetType("ClassLibrary1.Class1");
            var mi = t1.GetMethod("Method1", BindingFlags.NonPublic | BindingFlags.Instance, null, new Type[] { typeof(string) }, null);
            Console.WriteLine(mi.Invoke(o, new object[] { "A" }));
            Console.Read();
        }
    }
}
```
