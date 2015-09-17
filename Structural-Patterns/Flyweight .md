#Flyweight 

* Flyweight е дизайн патърн, който се използва да намали използването на ресурси, когато раборим  с голям брой обекти.
* Когато създаваме хиляди идентични обекти, Flyweight може да намали изпозваната памет.
* Flyweight е обект който намалява паметта като споделя колкото се може повече информация с останалите подобни обекти.

![alt text](uml/Flyweight.png)

~~~c#
public class Soldier:Unit
{
    public static int NumberOfInstances;

    public Soldier()
    {
        NumberOfInstances++;
    }

    public override void FireAt(Target target)
    {
        Console.WriteLine("Shooting at unit {0} with power of {1}."
            , target.ID, FirePower);
    }
}

public class Tank:Unit
{
    public static int NumberOfInstances;

    public Tank()
    {
        NumberOfInstances++;
    }

    public override void FireAt(Target target)
    {
        Console.WriteLine("Firing at {0} with power of {1}.", target.ID, FirePower);
    }
}

public abstract class Unit
{
    public string Name { get; internal set; }
    public int Armour { get; internal set; }
    public int Speed { get; internal set; }
    public int RotationRate { get; internal set; }
    public int FireRate { get; internal set; }
    public int Range { get; internal set; }
    public int FirePower { get; internal set; }
    public abstract void FireAt(Target target);
}

public class UnitFactory
{
    private readonly Dictionary<string, Unit> _units = new Dictionary<string, Unit>();

    public Unit GetUnit(string type)
    {
        if (_units.ContainsKey(type))
        {
            return _units[type];
        }
        Unit unit;

        switch (type)
        {
            case "Infantry":
                unit = new Soldier
                           {
                               Name = "Standard Infantry",
                               Armour = 5,
                               Speed = 4,
                               RotationRate = 180,
                               FireRate = 5,
                               Range = 100,
                               FirePower = 5
                           };
                break;

            case "Marine":
                unit = new Soldier
                           {
                               Name = "Marine",
                               Armour = 25,
                               Speed = 4,
                               RotationRate = 180,
                               FireRate = 3,
                               Range = 200,
                               FirePower = 10
                           };
                break;

            case "Tank":
                unit = new Tank
                           {
                               Name = "Tank",
                               Armour = 1000,
                               Speed = 25,
                               RotationRate = 5,
                               FireRate = 30,
                               Range = 1000,
                               FirePower = 250
                           };
                break;

            default:
                throw new ArgumentException();
        }

        _units.Add(type, unit);
        return unit;
    }
}

public class Target
{
    public Unit UnitData;
    public Guid ID;
}

class Program
{
    static void Main(string[] args)
    {
        UnitFactory factory = new UnitFactory();

        Target tank1 = new Target();
        tank1.ID = Guid.NewGuid();
        tank1.UnitData = factory.GetUnit("Tank");

        Target tank2 = new Target();
        tank2.ID = Guid.NewGuid();
        tank2.UnitData = factory.GetUnit("Tank");

        bool result = tank1.UnitData == tank2.UnitData;     // result = true
        int firepower = tank1.UnitData.FirePower;

        Console.WriteLine("Tank Instances: "+ Tank.NumberOfInstances);

        Target soldier1 = new Target();
        soldier1.ID = Guid.NewGuid();
        soldier1.UnitData = factory.GetUnit("Marine");

        var soldier2 = new Target();
        soldier2.UnitData = factory.GetUnit("Infantry");
        soldier2.ID = Guid.NewGuid();

        var soldier3 = new Target();
        soldier3.UnitData = factory.GetUnit("Infantry");
        soldier3.ID = Guid.NewGuid();

        Console.WriteLine("Soldier Instances: " + Soldier.NumberOfInstances);
    }
}
~~~