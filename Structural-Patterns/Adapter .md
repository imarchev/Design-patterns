#Adapter

* Adapter патерна се използва за да могат два несъвместими типа да комуникират.
* Когато един класс разчита на определен интерфейс, който не се изпълнява от друг клас, адаптера действа като преводач между двата класа.
* Adapter е се използва когато се променят изискванията и трябва да се приложат нови функционалности на класовете.

![alt text](uml/Adapter.png)

~~~c#
public interface ISerializerAdapter
{
    string Serialize<T>(object objectToSerialize);
}

public class JSONSerializerAdapter:ISerializerAdapter
{
    public string Serialize<T>(object objToSerialize)
    {
        var serializer = new JavaScriptSerializer();
        return serializer.Serialize(objToSerialize);
    }
}

public class XMLSerializerAdapter:ISerializerAdapter
{
    public string Serialize<T>(object objToSerialize)
    {
        using(var writer = new StringWriter())
        {
            var serializer = new XmlSerializer(typeof(T));
            serializer.Serialize(writer, objToSerialize);
            return writer.ToString();
        }
    }
}

public class PersonInfo
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public DateTime BirthDate { get; set; }
    public double Height { get; set; }
    public double Weight { get; set; }
}

class Program
{
    static void Main()
    {
        var serializer = new DataSerializer(new XMLSerializerAdapter());
        Console.WriteLine(serializer.Render());

        serializer = new DataSerializer(new JSONSerializerAdapter());
        Console.WriteLine(serializer.Render());
    }
}

public class DataSerializer
{
    private readonly ISerializerAdapter _serializer;

    public DataSerializer(ISerializerAdapter serializer)
    {
        _serializer = serializer;
    }

    public string Render()
    {
        var sb = new StringBuilder();

        var list = new List<PersonInfo>
                           {
                               new PersonInfo
                                   {
                                       FirstName = "Robert",
                                       LastName = "Kanasz",
                                       BirthDate = new DateTime(1985,8,19),
                                       Height = 168,Weight = 71
                                   },
                               new PersonInfo
                                   {
                                       FirstName = "John",
                                       LastName = "Doe",
                                       BirthDate = new DateTime(1981,9,25),
                                       Height = 189,
                                       Weight = 80
                                   },
                               new PersonInfo
                                   {
                                       FirstName = "Jane",
                                       LastName = "Doe",
                                       BirthDate = new DateTime(1989,12,1),
                                       Height = 164,
                                       Weight = 45
                                   }
                           };

        foreach (var personInfo in list)
        {
            sb.AppendLine(_serializer.Serialize<PersonInfo>(personInfo));
        }

        return sb.ToString();
    }

}
~~~