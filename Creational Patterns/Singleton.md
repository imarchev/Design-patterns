## Singleton

* Сек (Singleton) е създаващ шаблон за дизайн, който се използва в обектно-ориентираното програмиране.
 Този шаблон се използва обикновено в моделирането на обекти, които трябва да бъдат глобално достъпни за обектите на приложението (например обекта съдържащ структурите с настройките на програмата) или обекти, които се нуждаят от максимално късна инициализация за пестенето на ресурси от паметта.
* В софтуерното инженерство Сек шаблонът е дизайн шаблон, който представя ограничението на клас до един обект. Това е полезно, когато точно един обект се нуждае да координира действия през системата.
Концепцията понякога се отнася за системи, които работят по-ефективно, когато съществува само един обект или когато е ограничено представянето на определен брой обекти.
Терминът идва  от математическата концепция за Сек.
* Има критики откъм използването на Сек, като някои го смятат за анти-модел.
 Съди се, че е преизползван  и въвежда  ненужни ограничения в ситуации, където например  класа не е наистина необходим и въвежда глобално условни в апликацията.



~~~c#
// Пример за Сек
using System;

namespace SingletonDesignPattern
{
  /// <summary>
  /// Главен клас за стартиране на примерното приложение
  /// </summary>
  class MainApp
  {
    /// <summary>
    /// Главен метод за стартиране
    /// </summary>
    static void Main()
    {

      // Конструкторът е защитен -- операторът new не може да бъде извикан
      Singleton s1 = Singleton.Instance();
      Singleton s2 = Singleton.Instance();

      // Проверка за идентичност на инстанцията
      if (s1 == s2)
      {
        Console.WriteLine("Objects are the same instance"); //и двата обекта са една и съща инстанция на този клас
      }
      // Изчакай удар по конзолата от Котребителя
      Console.ReadKey();
    }
  }

  /// <summary>
  /// The 'Singleton' class – класът Сек
  /// </summary>
  class Singleton
  {
    private static Singleton _instance;  //променлива за единствената инстанция на този клас
    // Конструторът е защитен и не може да бъде извикан
    protected Singleton()
    {

    }

    // Единтвеният начин за инстанцииране е от тук
    public static Singleton Instance()
    {
      // Използва късна инициализация (lazy initialization)
      // N.B.: Да не се използва в многонишкови приложения
      if (_instance == null)
      {
        _instance = new Singleton();
      }
      return _instance;
    }
  }

}
~~~
