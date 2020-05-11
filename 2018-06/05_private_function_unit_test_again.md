# 非公開関数のユニットテストについて 再び (C#)

[前の記事](https://qiita.com/c-yan/items/f4acb3ab4c8090254a0e)で「PrivateObject クラス を使うとソースを書き換えずテストできますよ。」とコメントをもらったので.

静的メソッドは`PrivateType`、動的メソッドは`PrivateObject`でテストできるようです.
ただ、呼び出しがめんどくさいし、メソッド名が`nameof`できないので、リネーム機能の適用対象外になるのが辛い.
もちろんテストのためだけにアクセス修飾子を変えるのはどうよと言われるとそれはそれで悩ましいところがありますが、`internal`の方が気楽な感じがしました.


```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using TestTarget;

namespace TestTarget
{
    public class Target
    {
        private string value;

        public Target(string value)
        {
            this.value = value;
        }

        private string DynamicMethod()
        {
            return value;
        }

        private static string StaticMethod()
        {
            return "static";
        }
    }
}

namespace UnitTest
{
    [TestClass]
    public class Test
    {
        [TestMethod]
        public void StaticMethodTest()
        {
            Assert.AreEqual("static", new PrivateType(typeof(Target)).InvokeStatic("StaticMethod", null));
        }

        [TestMethod]
        public void DynamicMethodTest()
        {
            var value = "dynamic";
            Assert.AreEqual(value, new PrivateObject(typeof(Target), new object[] { value }).Invoke("DynamicMethod", null));
        }
    }
}
```
