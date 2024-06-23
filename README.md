# Andreea's Cooking App ( Program.cs )

## Descriere

Această aplicație console gestionează rețetele de aperitive pentru "Andrea's Cooking". Oferă funcționalități pentru vizualizarea, adăugarea, modificarea și ștergerea aperitivelor din baza de date phpMyAdmin. Este facut in C#.

## Structura Codului `Program.cs`

### Importurile Necesare

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using MySql.Data.MySqlClient;

Acestea sunt importurile necesare pentru funcționalitățile de bază ale C# și pentru interacțiunea cu baza de date MySQL.

Clasa Principală și Metoda 'Main'

namespace AndreeascookingApp1
{
    internal class Program
    {
        static void Main(string[] args)
        {
            MeniuPrincipal();
        }
    }
}
Clasa Program este punctul de intrare al aplicației.
Metoda Main apelează metoda MeniuPrincipal, care afișează meniul principal al aplicației.

Metoda 'MeniuPrincipal'

static void MeniuPrincipal()
{
    while (true)
    {
        Console.WriteLine("=== Meniu Principal ===");
        Console.WriteLine("1. Vizualizare Aperitive");
        Console.WriteLine("2. Adăugare Aperitive");
        Console.WriteLine("3. Modificare Aperitive");
        Console.WriteLine("4. Stergere Aperitive");
        Console.WriteLine("5. Iesire");
        Console.Write("Alegeti o optiune: ");

        string optiune = Console.ReadLine();

        switch (optiune)
        {
            case "1":
                VizualizareAperitive();
                break;
            case "2":
                AdaugareAperitiv();
                break;
            case "3":
                ModificareAperitiv();
                break;
            case "4":
                StergereAperitiv();
                break;
            case "5":
                return;
            default:
                Console.WriteLine("Opțiune invalidă. Vă rugăm să alegeți din nou.");
                break;
        }
    }
}
Afișează un meniu principal și așteaptă input-ul utilizatorului pentru a alege una dintre opțiunile disponibile.
În funcție de opțiunea selectată, se apelează metodele corespunzătoare pentru gestionarea aperitivelor.

Metoda 'VizualizareAperitive'

static void VizualizareAperitive()
{
    Console.WriteLine("=== Vizualizare Aperitive ===");
    string connectionString = "Server=localhost;Uid=root;Database=Andreeascooking;";

    using (MySqlConnection con = new MySqlConnection(connectionString))
    {
        con.Open();
        string query = "SELECT * FROM aperitive";
        MySqlCommand cmd = new MySqlCommand(query, con);
        MySqlDataReader reader = cmd.ExecuteReader();

        while (reader.Read())
        {
            Console.WriteLine($"Nume: {reader["nume"]}, Descriere: {reader["descriere"]}, Preț: {reader["pret"]}");
        }

        reader.Close();
    }
}
Conectează aplicația la baza de date și afișează toate aperitivele existente.
Folosește un MySqlDataReader pentru a citi și afișa datele.

Metoda 'AdaugareAperitiv'

static void AdaugareAperitiv()
{
    Console.WriteLine("=== Adaugare Aperitiv ===");
    Console.Write("Introduceti numele aperitivului: ");
    string nume = Console.ReadLine();
    Console.Write("Introduceti descrierea aperitivului: ");
    string descriere = Console.ReadLine();
    Console.Write("Introduceti pretul aperitivului: ");
    decimal pret = Convert.ToDecimal(Console.ReadLine());

    string connectionString = "Server=localhost;Uid=root;Database=Andreeascooking;";

    using (MySqlConnection con = new MySqlConnection(connectionString))
    {
        con.Open();
        string query = "INSERT INTO aperitive (nume, descriere, pret) VALUES (@nume, @descriere, @pret)";
        MySqlCommand cmd = new MySqlCommand(query, con);
        cmd.Parameters.AddWithValue("@nume", nume);
        cmd.Parameters.AddWithValue("@descriere", descriere);
        cmd.Parameters.AddWithValue("@pret", pret);
        cmd.ExecuteNonQuery();

        Console.WriteLine("Aperitivul a fost adaugat cu succes.");
    }
}
Permite utilizatorului să introducă detalii despre un nou aperitiv și să-l adauge în baza de date.
Folosește parametrii pentru a preveni SQL injection.

Metoda 'ModificareAperitiv'

static void ModificareAperitiv()
{
    Console.WriteLine("=== Modificare Aperitiv ===");
    Console.Write("Introduceti ID-ul aperitivului pe care doriți să-l modificati: ");
    int id = Convert.ToInt32(Console.ReadLine());

    Console.Write("Introduceti noul nume pentru aperitiv: ");
    string nume = Console.ReadLine();
    Console.Write("Introduceti noua descriere pentru aperitiv: ");
    string descriere = Console.ReadLine();
    Console.Write("Introduceti noul pret pentru aperitiv: ");
    decimal pret = Convert.ToDecimal(Console.ReadLine());

    string connectionString = "Server=localhost;Uid=root;Database=Andreeascooking;";

    using (MySqlConnection con = new MySqlConnection(connectionString))
    {
        con.Open();
        string query = "UPDATE aperitive SET nume = @nume, descriere = @descriere, pret = @pret WHERE id = @id";
        MySqlCommand cmd = new MySqlCommand(query, con);
        cmd.Parameters.AddWithValue("@nume", nume);
        cmd.Parameters.AddWithValue("@descriere", descriere);
        cmd.Parameters.AddWithValue("@pret", pret);
        cmd.Parameters.AddWithValue("@id", id);
        cmd.ExecuteNonQuery();

        Console.WriteLine("Aperitivul a fost modificat cu succes.");
    }
}

Permite utilizatorului să modifice detaliile unui aperitiv existent identificat prin ID.
Actualizează înregistrarea în baza de date folosind parametrii pentru a preveni SQL injection.

Metoda 'StergereAperitiv'

static void StergereAperitiv()
{
    Console.WriteLine("=== Stergere Aperitiv ===");
    Console.Write("Introduceti ID-ul aperitivului pe care doriti să-l stergeti: ");
    int id = Convert.ToInt32(Console.ReadLine());

    string connectionString = "Server=localhost;Uid=root;Database=Andreeascooking;";

    using (MySqlConnection con = new MySqlConnection(connectionString))
    {
        con.Open();
        string query = "DELETE FROM aperitive WHERE id = @id";
        MySqlCommand cmd = new MySqlCommand(query, con);
        cmd.Parameters.AddWithValue("@id", id);
        cmd.ExecuteNonQuery();

        Console.WriteLine("Aperitivul a fost sters cu succes.");
    }
}

Permite utilizatorului să șteargă un aperitiv din baza de date identificat prin ID.
Execută comanda DELETE în baza de date folosind parametrii pentru a preveni SQL injection.

Concluzie
Această aplicație console pentru "Andrea's Cooking" oferă funcționalități complete pentru gestionarea aperitivelor, inclusiv vizualizarea, adăugarea, modificarea și ștergerea acestora. Utilizatorii pot interacționa cu baza de date MySQL prin intermediul unei interfețe simple și intuitive.
