## Prototype

* Prototype е създаващ шаблон за дизайн, който се използва в обектно-ориентираното програмиране. 
* Създава обекти с помощта на обект-прототип.
* Новите обекти се създават чрез клониране на прототипа, вместо с използване на конструктор.

![alt text](uml/Prototype.png)


~~~c#
// Шаблон за прототип
using System;
using System.Collections.Generic;
namespace PrototypePattern
{
	/// <summary>
	/// </summary>
	class MainApp
	{
		/// <summary>
		/// Главния стартиращ метод
		/// </summary>
		static void Main ()
		{
			ColorManager colormanager = new ColorManager ();
			// Инициализирай със стандартните цветове
			colormanager["red"] = new Color ( 255, 0, 0 );
			colormanager["green"] = new Color ( 0, 255, 0 );
			colormanager["blue"] = new Color ( 0, 0, 255 );
			// Потребителят избира цветове според предпочитаниятаси
			colormanager["angry"] = new Color ( 255, 54, 0 );
			colormanager["peace"] = new Color ( 128, 211, 128 );
			colormanager["flame"] = new Color ( 211, 34, 20 );
			// Потребителя клонира избраните цветове
			Color color1 = colormanager["red"].Clone () as Color;
			Color color2 = colormanager["peace"].Clone () as Color;
			Color color3 = colormanager["flame"].Clone () as Color;
			// Изчакай за удар от потребителя
			Console.ReadKey ();
		}
	}
	/// <summary>
	/// Абстрактния клас от тип 'Prototype'
	/// </summary>
	abstract class ColorPrototype
	{
		public abstract ColorPrototype Clone ();
	}
	/// <summary>
	/// The 'ConcretePrototype' class
	/// </summary>
	class Color : ColorPrototype
	{
		private int _red;
		private int _green;
		private int _blue;
		// Constructor
		public Color ( int red, int green, int blue )
		{
			this._red = red;
			this._green = green;
			this._blue = blue;
		}
		// Create a shallow copy
		public override ColorPrototype Clone ()
		{
			Console.WriteLine (
				"Cloning color RGB: {0,3},{1,3},{2,3}",
				_red, _green, _blue );
			return this.MemberwiseClone () as ColorPrototype;
		}
	}


	/// <summary>
	/// Prototype manager
	/// </summary>
	class ColorManager
	{
		private Dictionary<string, ColorPrototype> _colors =
			new Dictionary<string, ColorPrototype> ();
		// Indexer
		public ColorPrototype this[string key]
		{
			get { return _colors[key]; }
			set { _colors.Add ( key, value ); }
		}
	}
}
~~~