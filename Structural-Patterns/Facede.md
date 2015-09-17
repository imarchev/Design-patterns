# Фасада

 Основното предназначение дава ни опростен интерфейс, опростява сложен за използване код.
 С Фасада можем да направим дадена библиотека по лесна за използване, разбиране и тестване, тъй като фасадата има удобни методи за общите задачи, също така можем да wrap-нем лошо написан код.


![alt text](uml/Facade.png)

~~~c#
namespace IVSR.DesignPattern.Facade.Sample
{
    // The 'Subsystem ClassA' class
    class CarModel
    {
        public void SetModel()
        {
            Console.WriteLine(" CarModel - SetModel");
        }
    }

    /// <summary>
    /// The 'Subsystem ClassB' class
    /// </summary>
    class CarEngine
    {
        public void SetEngine()
        {
            Console.WriteLine(" CarEngine - SetEngine");
        }
    }

    // The 'Subsystem ClassC' class
    class CarBody
    {
        public void SetBody()
        {
            Console.WriteLine(" CarBody - SetBody");
        }
    }

    // The 'Subsystem ClassD' class
    class CarAccessories
    {
        public void SetAccessories()
        {
            Console.WriteLine(" CarAccessories - SetAccessories");
        }
    }

    // The 'Facade' class
    public class CarFacade
    {
        private CarModel model;
        private CarEngine engine;
        private CarBody body;
        private CarAccessories accessories;

        public CarFacade()
        {
            model = new CarModel();
            engine = new CarEngine();
            body = new CarBody();
            accessories = new CarAccessories();
        }

        public void CreateCompleteCar()
        {
            Console.WriteLine("******** Creating a Car **********\n");
            model.SetModel();
            engine.SetEngine();
            body.SetBody();
            accessories.SetAccessories();

            Console.WriteLine("\n******** Car creation is completed**********");
        }
    }

    // Facade Pattern Demo
    class Program
    {
        static void Main(string[] args)
        {
            CarFacade facade = new CarFacade();

            facade.CreateCompleteCar();

            Console.ReadKey();

        }
    }
}
~~~
