# C# で名前空間接頭辞入りの XML を作る

この XML を作る.

```xml
<Root xmlns="http://example.com/foo">
  <Item1></Item1>
  <Item2 xmlns:i="http://example.com/bar">
    <Item3 />
    <i:Item4 />
  </Item2>
</Root>
```

その場合のコードは以下のようになる.

```csharp
var ns1 = XNamespace.Get("http://example.com/foo");
var ns2 = XNamespace.Get("http://example.com/bar");
var xml =
    new XElement(ns1 + "Root",
        new XElement(ns1 + "Item1", ""),
        new XElement(ns1 + "Item2", new XAttribute(XNamespace.Xmlns + "i", ns2),
            new XElement(ns1 + "Item3"),
            new XElement(ns2 + "Item4")));
Console.WriteLine(xml.ToString());
```

大きな XML を吐こうと思うと便利なんだろうけど、小さい XML を吐く分にはまだるっこしい感.
