# Цезарова шифра (C#)

## Задатак 1

Креирај конзолну апликацију у програмском језику C# за шифровање и дешифровање
порука Цезаровом шифром. Омогући да корисник апликације у поруци може да уноси
мала и велика слова енглеског алфабета, размаке, бројеве и знаке интерпункције.
Након уноса поруке, корисник треба да унесе кључ који може да има вредност од 0
до 25. Апликација треба да шифрује/дефишфрује искључиво слова. Размаци, бројеви
и знаци интерпункије не треба да се шифрују/дешифрују!

Кôд конзолне апликације **мора да садржи две класе**:

* класу `Program` са методом `Main()` и
* класу `CaesarCipher`.

Класа `CaesarCipher` треба да садржи:

* конструктор са параметром који треба да прихвати вредност кључа и осигура да
је та вредност у дозвољеном опсегу,
* приватно својство са гетером и сетером за чување вредности кључа,
* јавну методу за шифровање поруке,
* јавну методу за дешифровање поруке и
* опционо, приватну методу за обраду порука коју користе претходне две методе.

**Решење задатка**:

Класа `CaesarCipher` може да изгледа овако...

```cs
public class CaesarCipher
{
    public CaesarCipher(int kljuc)
    {
        Kljuc = kljuc % 26;
    }

    private int Kljuc { get; set; }

    public string Sifruj(string poruka)
    {
        return Obradi(poruka, Kljuc);
    }

    public string Desifruj(string poruka)
    {
        return Obradi(poruka, 26 - Kljuc);
    }

    private string Obradi(string poruka, int pomeraj)
    {
        string rezultat = string.Empty;
        foreach (char karakter in poruka)
        {
            if (char.IsLetter(karakter))
            {
                char prvo = char.IsUpper(karakter) ? 'A' : 'a';
                char slovo = (char)(((karakter + pomeraj - prvo) % 26) + prvo);
                rezultat += slovo;
            }
            else
            {
                rezultat += karakter;
            }
        }
        return rezultat;
    }
}
```

...а класа `Program` овако:

```cs
using System;

class Program
{
    static void Main()
    {
        Console.Write("Unesi poruku: ");
        string poruka = Console.ReadLine();
        Console.Write("Unesi kljuc: ");
        int kljuc = int.Parse(Console.ReadLine());
        CaesarCipher cc = new CaesarCipher(kljuc);
        Console.Write("Unesi s za sifrovanje ili d za desifrovanje: ");
        char opcija = (char)Console.Read();
        if (opcija == 's')
        {
            string sifrovana = cc.Sifruj(poruka);
            Console.WriteLine($"Sifrovana poruka: {sifrovana}");
        }
        else if (opcija == 'd')
        {
            string desifrovana = cc.Desifruj(poruka);
            Console.WriteLine($"Desifrovana poruka: {desifrovana}");
        }
        else
        {
            Console.WriteLine($"Pogresan unos.");
        }
    }
}
```

**Тест пример 1**:

```text
Unesi poruku: Hello, World! 123
Unesi kljuc: 3
Unesi s za sifrovanje ili d za desifrovanje: s
Sifrovana poruka: Khoor, Zruog! 123
```

**Тест пример 2:**:

```text
Unesi poruku: Khoor, Zruog! 123
Unesi kljuc: 3
Unesi s za sifrovanje ili d za desifrovanje: d
Desifrovana poruka: Hello, World! 123
```

## Задатак 2

Искористи класу `CaesarCipher` из претходног задатка и креирај конзолну
апликацију за шифровање и дешифровање порука Цезаровом шифром, која прима
аргументе из конзоле:

* Први аргумент:
    * `-s` за операцију шифровања поруке или
    * `-d` за операцију дешифровања поруке.
* Други аргумент:
    * `poruka` за навођење поруке.
* Трећи аргумет:
    * `kljuc` за навођење кључа.

**Решење задатка**:

```cs
using System;

class Program
{
    static void Uputstvo()
    {
        Console.WriteLine("Upotreba:");
        Console.WriteLine("  -s \"poruka\" kljuc    Operacija sifrovanja poruke.");
        Console.WriteLine("  -d \"poruka\" kljuc    Operacija desifrovanja poruke.");
        Console.WriteLine();
        Console.WriteLine("Primer sifrovanja poruke:");
        Console.WriteLine("  cezar.exe -s \"Hello, World!\" 3");
        Console.WriteLine();
        Console.WriteLine("Primer desifrovanja poruke:");
        Console.WriteLine("  cezar.exe -d \"Khoor, Zruog!\" 3");
        Console.WriteLine();
    }

    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.WriteLine("GRESKA: Pogresan broj argumenata!");
            Uputstvo();
            return;
        }
        string operacija = args[0];
        string poruka;
        if (args[0] == "-s" || args[0] == "-d")
        {
            poruka = args[1];
        }
        else
        {
            Console.WriteLine("GRESKA: Neispravna operacija!");
            Uputstvo();
            return;
        }
        int kljuc = 0;
        if (int.TryParse(args[2], out kljuc))
        {
            CaesarCipher cc = new CaesarCipher(kljuc);
            if (operacija == "-s")
            {
                string sifrovana = cc.Sifruj(poruka);
                Console.WriteLine($"Sifrovana poruka: {sifrovana}");
            }
            else
            {
                string desifrovana = cc.Desifruj(poruka);
                Console.WriteLine($"Desifrovana poruka: {desifrovana}");
            }
        }
        else
        {
            Console.WriteLine("GRESKA: Neispravan kljuc!");
            Uputstvo();
        }
    }
}
```

**Тест пример 1**:

```text
program.exe -s "Hello, World!" 3
Sifrovana poruka: Khoor, Zruog!
```

**Тест пример 2**:

```text
program.exe -s "Khoor, Zruog!" 3
Desifrovana poruka: Hello, World!
```

## Задатак 3

Искористи класу `CaesarCipher` из првог задатка и креирај конзолну апликацију
за шифровање и дешифровање текстуалних фајлова Цезаровом шифром, која прима
аргументе из конзоле:

* Први аргумент:
    * `-s` за операцију шифровања текстуалног фајла или
    * `-d` за операцију дешифровања текстуалног фајла.
* Други аргумент:
    * `fajl` за навођење fajla.
* Трећи аргумет:
    * `kljuc` за навођење кључа.

**Решење задатка**:

```cs
using System;
using System.IO;

class Program
{
    static void Uputstvo()
    {
        Console.WriteLine("Upotreba:");
        Console.WriteLine("  -s fajl kljuc    Operacija sifrovanja poruke.");
        Console.WriteLine("  -d fajl kljuc    Operacija desifrovanja poruke.");
        Console.WriteLine();
        Console.WriteLine("Primer sifrovanja tekstualnog fajla nekifajl.txt:");
        Console.WriteLine("  cezar.exe -s nekifajl.txt 3");
        Console.WriteLine();
        Console.WriteLine("Primer desifrovanja tekstualnog fajla nekifajl.txt:");
        Console.WriteLine("  cezar.exe -d nekifajl.txt 3");
        Console.WriteLine();
    }

    static void Main(string[] args)
    {
        if (args.Length != 3)
        {
            Console.WriteLine("GRESKA: Pogresan broj argumenata!");
            Uputstvo();
            return;
        }
        string operacija = args[0];
        string fajl;
        if (args[0] == "-s" || args[0] == "-d")
        {
            fajl = args[1];
            if (!File.Exists(fajl))
            {
                Console.WriteLine("GRESKA: Fajl ne postoji!");
                return;
            }
        }
        else
        {
            Console.WriteLine("GRESKA: Neispravna operacija!");
            Uputstvo();
            return;
        }
        int kljuc = 0;
        if (int.TryParse(args[2], out kljuc))
        {
            CaesarCipher cc = new CaesarCipher(kljuc);
            try
            {
                string ulaz = File.ReadAllText(fajl);
                string izlaz = string.Empty;
                if (operacija == "-s")
                {
                    izlaz = cc.Sifruj(ulaz);
                }
                else
                {
                    izlaz = cc.Desifruj(ulaz);
                }
                File.WriteAllText(fajl, izlaz);
            }
            catch (Exception e)
            {
                Console.WriteLine($"GRESKA: Neuspela obrada fajla!\r\n{e.Message}");
            }
        }
        else
        {
            Console.WriteLine("GRESKA: Neispravan kljuc!");
            Uputstvo();
        }
    }
}
```

**Тест пример 1**:

Ако се у фајлу `otvoreni.txt` налази отворени текст...

```text
Hello, World!
Linija broj 2.
Tekst u 3. liniji.
```

...онда ће се извршавањем наредбе...

```text
program.exe -s otvoreni.txt 3
```

...отворени текст променити у шифрат:

```text
Khoor, Zruog!
Olqlmd eurm 2.
Whnvw x 3. olqlml.
```

**Тест пример 2**:

Ако се у фајлу `sifrat.txt` налази шифрат...

```text
Khoor, Zruog!
Olqlmd eurm 2.
Whnvw x 3. olqlml.
```

...онда ће се извршавањем наредбе...

```text
program.exe -d sifrat.txt 3
```

...шифрат променити у отворени текст:

```text
Hello, World!
Linija broj 2.
Tekst u 3. liniji.
```
