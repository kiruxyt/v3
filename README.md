# v3

using System;
using System.IO;
namespace Data_V3
{
class Program
{
static void Main(string[] args)
{
Menu();
Console.ReadKey();
}

private static void Menu()
{
bool pass = true;
Console.WriteLine("=====1.Mostrar los registros=====");
Console.WriteLine("=====2.Encontrar un registro=====");
Console.WriteLine("======3.Agregar un registro======");
Console.WriteLine("=============4.Salir=============");

do
{
pass = true;
string option = Console.ReadLine();
option = option.ToLower();
switch (option)
{
case "1":
ShowRegisters();
break;
case "2":
FindRegister();
break;
case "3":
AddNewRegister();
break;
case "4":
Environment.Exit(1);
break;
default:
Console.WriteLine("La opcion no es valida");
pass = false;
continue;
}
} while (!pass);

}
private static void FindRegister()
{
if (File.Exists(@"C:\Users\kirux\Desktop\ejercicios\Data V3\Save.txt"))
{
Console.WriteLine("introduzca la cedula del registro que deseas buscar");
int lineFound = 0;
string cedula = ReadCedula();
string[] lines = File.ReadAllLines(@"C:\Users\kirux\Desktop\ejercicios\Data V3\Save.txt");
foreach (string line in lines)
{
if (line.Contains(cedula))
{
Console.WriteLine(line);
lineFound++;
break;
}
lineFound++;
}
Console.WriteLine(lineFound);
Console.WriteLine("===1.Editar registro===");
Console.WriteLine("===2.Borrar registro===");
Console.WriteLine("========3.Salir========");
bool pass = true;
do
{
pass = true;
string option = Console.ReadLine();
option = option.ToLower();
switch (option)
{
case "1":
EditRegister(lines, lineFound);
break;
case "2":
DeleteRegister(lines, lineFound);
break;
case "3":
Environment.Exit(1);
break;
default:
Console.WriteLine("La opcion no es valida");
pass = false;
continue;
}
} while (!pass);
}
}
private static void ShowRegisters()
{
if (File.Exists(@"C:\Users\kirux\Desktop\ejercicios\Data V3\Save.txt"))
{
string[] lines = File.ReadAllLines(@"C:\Users\kirux\Desktop\ejercicios\Data V3\Save.txt");
foreach (string line in lines)
Console.WriteLine(line);
}
Menu();
}
private static void AddNewRegister()
{
string register = "";

bool pass = true;
bool pass2 = true;
do
{
string cedula = ReadCedula();
string name = ReadName();
string lastName = ReadLastName();
int age = ReadAge();
decimal save = ReadSave();
string password = ReadPassword();

register = cedula + "," + name + "," + lastName + "," + age + "," + save + "," + password;
pass = true;
Console.WriteLine("Grabar(G), Continuar(C), Salir(S)?");
do
{
pass2 = true;
string option = Console.ReadLine();
option = option.ToLower();
switch (option)
{
case "g":
break;
case "c":
pass = false;
break;
case "s":
Environment.Exit(1);
break;
default:
Console.WriteLine("La opcion no es valida");
pass2 = false;
continue;
}
} while (!pass2);
} while (!pass);

using (System.IO.StreamWriter file =
new System.IO.StreamWriter(@"C:\Users\kirux\Desktop\ejercicios\Data V3\Save.txt", true))
{
file.WriteLine(register);
}
Console.WriteLine("Se ha guardado exitosamente.");
Menu();
}
private static void EditRegister(string[] lines, int line)
{
string register = "";
string cedula = ReadCedula();
string name = ReadName();
string lastName = ReadLastName();
int age = ReadAge();
decimal save = ReadSave();
string password = ReadPassword();

register = cedula + "," + name + "," + lastName + "," + age + "," + save + "," + password;

using (StreamWriter writer = new StreamWriter(@"C:\Users\kirux\Desktop\ejercicios\Data V3\Save.txt"))
{
for (int currentLine = 1; currentLine <= lines.Length; ++currentLine)
{
if (currentLine == line)
{
writer.WriteLine(register);
}
else
{
writer.WriteLine(lines[currentLine - 1]);
}
}
}
Console.WriteLine("Se ha editado exitosamente.");
}
private static void DeleteRegister(string[] lines, int line)
{
using (StreamWriter writer = new StreamWriter(@"C:\Users\kirux\Desktop\ejercicios\Data V3\Save.txt"))
{
for (int currentLine = 1; currentLine <= lines.Length; ++currentLine)
{
if (currentLine == line)
{
continue;
}
else
{
writer.WriteLine(lines[currentLine - 1]);
}
}
}
Console.WriteLine("Se ha borrado exitosamente.");

}
private static string ReadCedula()
{
bool pass = true;
string cedulaNumber = "";
do
{
pass = true;
Console.WriteLine("introduce tu Cedula:");

cedulaNumber = Console.ReadLine();

if (cedulaNumber.Length != 11)
{
pass = false;
Console.WriteLine("Tienen que ser 11 numeros");
continue;
}
} while (!pass);
return cedulaNumber;
}
private static string ReadName()
{
bool pass = true;
string name = "";
do
{
pass = true;
Console.WriteLine("ingresa tu nombre:");

name = Console.ReadLine();

if (name.Length == 0)
{
pass = false;
Console.WriteLine("No puede dejarlo vacio");
continue;
}
} while (!pass);
return name;

}
private static string ReadLastName()
{
bool pass = true;
string lastName = "";
do
{
pass = true;
Console.WriteLine("ingresa tu apelido:");

lastName = Console.ReadLine();

if (lastName.Length == 0)
{
pass = false;
Console.WriteLine("No puede dejarlo vacio");
continue;
}
} while (!pass);
return lastName;
}
private static int ReadAge()
{
bool pass = true;
int age = 0;
do
{
pass = true;
Console.WriteLine("ingrese su edad:");
try
{
age = Convert.ToInt16(Console.ReadLine());
}
catch
{
pass = false;
Console.WriteLine("debe ser un numero entero");
continue;
}
if (age == 0)
{
pass = false;
Console.WriteLine("No puede dejarlo vacio");
continue;
}

} while (!pass);
return age;
}
private static decimal ReadSave()
{
bool pass = true;
decimal save = 0;
do
{
pass = true;
Console.WriteLine("ingresa cuanto quieres ahorrar:");
try
{
save = Convert.ToDecimal(Console.ReadLine());
}
catch
{
pass = false;
Console.WriteLine("debe ser un numero, con un maximo de dos decimal");
continue;
}
if (save == 0)
{
pass = false;
Console.WriteLine("No puede dejarlo vacio");
continue;
}

} while (!pass);
return save;
}

private static string ReadPassword()
{
bool pass = true;
string password = "";
string confirmPassword = "";
do
{
pass = true;
Console.WriteLine("Ingrese la contraseña:");

password = Console.ReadLine();

if (password.Length == 0)
{
pass = false;
Console.WriteLine("No puede dejarlo vacio");
continue;
}
if (password.Length < 7)
{
pass = false;
Console.WriteLine("Debe ser tener mas de 6 caracteres.");
continue;
}
Console.WriteLine("Confirme la contraseña:");

confirmPassword = Console.ReadLine();
if (password != confirmPassword)
{
pass = false;
Console.WriteLine("Las contraseñas no son iguales");
continue;
}

} while (!pass);
return password;
}
}
}
