# CazaTesoroV2
Versión 2 del juego caza tesoro.

using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        Console.WriteLine("BIENVENIDO A LA GRAN CAZA DEL TESORO - VERSIÓN 2");
        Console.WriteLine("Tu misión: encontrar el cofre escondido. Puedes abandonar en cualquier momento escribiendo 'S'.\n");

        while (true)
        {
            int intentosPermitidos = ElegirNivelDificultad();
            if (intentosPermitidos == -1) return;

            int limite = PedirNumeroMaximo();
            if (limite == -1) return;

            Random random = new Random();
            int tesoro = random.Next(0, limite + 1);

            Console.WriteLine($"\nEl tesoro está escondido entre 0 y {limite}. Tienes {intentosPermitidos} intentos.");
            List<int> intentos = new List<int>();

            int intentoActual = 0;
            bool encontrado = false;

            while (intentoActual < intentosPermitidos)
            {
                Console.Write($"\nIntento {intentoActual + 1}/{intentosPermitidos}: Ingresa tu número: ");
                string entrada = Console.ReadLine()!;

                if (entrada.Equals("S", StringComparison.OrdinalIgnoreCase))
                {
                    Console.WriteLine("Has decidido abandonar la búsqueda. Hasta la próxima.");
                    return;
                }

                if (!int.TryParse(entrada, out int intento) || intento <= 0)
                {
                    Console.WriteLine("Número inválido. La expedición termina aquí.");
                    return;
                }

                intentos.Add(intento);

                if (intento == tesoro)
                {
                    Console.WriteLine("¡Increíble! Has encontrado el tesoro escondido.");
                    encontrado = true;
                    break;
                }
                else
                {
                    Console.WriteLine("Ese no es el lugar correcto...");
                    Console.Write("¿Quieres intentarlo de nuevo? (S para salir / cualquier tecla para continuar): ");
                    string decision = Console.ReadLine()!;
                    if (decision.Equals("S", StringComparison.OrdinalIgnoreCase))
                    {
                        Console.WriteLine("Has abandonado la búsqueda. Hasta pronto.");
                        return;
                    }
                }

                intentoActual++;
            }

            if (!encontrado)
            {
                Console.WriteLine($"\nTe has quedado sin intentos. El tesoro estaba en el lugar: {tesoro}.");
            }

            Console.WriteLine("\nResumen de tus intentos:");
            foreach (int i in intentos)
            {
                Console.WriteLine($"Intento: {i}");
            }

            Console.Write("\n¿Quieres jugar de nuevo? (S para salir / cualquier tecla para reiniciar): ");
            string jugarOtraVez = Console.ReadLine()!;
            if (jugarOtraVez.Equals("S", StringComparison.OrdinalIgnoreCase))
            {
                Console.WriteLine("Gracias por jugar. Hasta la próxima caza del tesoro.");
                break;
            }
        }
    }

    static int ElegirNivelDificultad()
    {
        while (true)
        {
            Console.WriteLine("Elige un nivel de dificultad:");
            Console.WriteLine("1 - Nivel 1 (10 intentos)");
            Console.WriteLine("2 - Nivel 2 (5 intentos)");
            Console.WriteLine("3 - Nivel 3 (3 intentos)");
            Console.Write("Escribe el número del nivel (o 'S' para salir): ");
            string entrada = Console.ReadLine()!;

            if (entrada.Equals("S", StringComparison.OrdinalIgnoreCase)) return -1;

            if (int.TryParse(entrada, out int nivel))
            {
                switch (nivel)
                {
                    case 1: return 10;
                    case 2: return 5;
                    case 3: return 3;
                    default:
                        Console.WriteLine("Ese nivel no existe. Por favor, elige 1, 2 o 3.\n");
                        continue;
                }
            }
            else
            {
                Console.WriteLine("Opción inválida. Intenta de nuevo.\n");
            }
        }
    }

    static int PedirNumeroMaximo()
    {
        while (true)
        {
            Console.Write("\nIngresa el número máximo del mapa del tesoro (o 'S' para salir): ");
            string entrada = Console.ReadLine()!;

            if (entrada.Equals("S", StringComparison.OrdinalIgnoreCase)) return -1;

            if (int.TryParse(entrada, out int limite) && limite > 0)
            {
                return limite;
            }
            Console.WriteLine("El número debe ser un entero mayor que cero. Intenta de nuevo.\n");
        }
    }
}
