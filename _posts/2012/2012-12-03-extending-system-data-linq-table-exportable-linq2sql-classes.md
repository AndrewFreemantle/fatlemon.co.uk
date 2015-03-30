---
title: 'Extending System.Data.Linq.Table&lt;T&gt; &#8211; Exportable Linq2SQL Classes'
author: Andrew Freemantle
layout: post
permalink: /2012/12/extending-system-data-linq-table-exportable-linq2sql-classes/
tags:
  - 'C#'
  - Development
  - Linq
---
Wouldn't this be handy?:

{% highlight csharp %}
using (var db = new DataContext()) {
    return db.StockItems.ToXElement();
}
{% endhighlight %}

I've found this approach useful when working on API's which essentially provide access to data that closely mirrors Linq2SQL generated classes. It makes it easy to control which properties are included, and how they are formatted.

First, we declare an interface that we can decorate our Linq2SQL classes with. Note that I'm returning an `XElement` here, but this could be JSON or something else:

`IXElementable.cs`:
{% highlight csharp %}
using System;
using System.Xml.Linq;

public interface IXElementable {
    XElement ToXElement();
}
{% endhighlight %}

Then we augment the Linq2SQL classes we wish to be exportable, using Partial Classes as we'd normally do:

e.g. `StockItem.cs`:
{% highlight csharp %}
public partial class StockItem: implements IXElementable {
  // ..
  public XElement ToXElement() {
    return new XElement("StockItem",
           new XElement("StockItemId", Id),
           new XElement("PartNo", PartNo),
           new XElement("QtyInStock", QtyInStock),
           new XElement("QtyOnOrder", StockOrders
             .Where(o => o.Status == (int)StockOrderStatuses.Outstanding)
             .Count()),
           new XElement("CreatedDate", CreatedDate.ToString("u")));
  }
  // ..
}
{% endhighlight %}

Finally, we extend the Linq `Table` class itself:

`TableExtensions.cs`:
{% highlight csharp %}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Linq;
using System.Xml.Linq;

public static class TableExtensions {

  /// <summary>
  /// Returns an XElement (XML) representation of the Table and its data
  /// <remarks>Note that data objects in the Table need to implement IXElementable&lt;/remarks>
  /// </summary>
  public static XElement ToXElement<T>(this Table<T> table) where T: class {

    var xml = new XElement(GetTableName(table));

    foreach (var item in table) {
      try {
        xml.Add(((IXElementable)item).ToXElement());

      } catch (Exception ex) {
        Log.Write(string.Format("Table: {0}.ToXElement", xml.Name), ex);
        
        // no point continuing..
        break;
      }
    }

    return xml;

  }

  private static string GetTableName<T>(Table<T> table) where T: class {
    // a bit of a hack, but there doesn't seem to be a nice way to get the Linq2SQL table name

    string tableName = table.GetType().ToString().Split('.').Last().Trim(']');

    // pluralise it..
    if (tableName.EndsWith("y"))
      tableName = tableName.TrimEnd('y') + "ies";
    else
      tableName += 's';

    return tableName;
  }
}
{% endhighlight %}

And that's it!

As you can see, this approach affords us complete control over the Linq2SQL generated properties and the format they're serialised to, and it makes it really easy to omit properties without having to re-implement them in the Partial class just so we can add the `[XmlIgnore]` attribute.