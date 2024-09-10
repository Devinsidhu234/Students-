public class Student
{
    public int StudentId { get; set; } // Primary Key
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public DateTime EnrollmentDate { get; set; }
}
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public DbSet<Student> Students { get; set; } // Table for Students

    // Configuration of the connection string
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=StudentDb;Trusted_Connection=True;");
    }
}
using System;

class Program
{
    static void Main(string[] args)
    {
        // Create a new instance of the context
        using (var context = new AppDbContext())
        {
            // Ensure database is created
            context.Database.EnsureCreated();

            // Create a new student
            var student = new Student
            {
                FirstName = "John",
                LastName = "Doe",
                EnrollmentDate = DateTime.Now
            };

            // Add the student to the context
            context.Students.Add(student);

            // Save changes to the database
            context.SaveChanges();

            // Confirmation message
            Console.WriteLine("Student added successfully!");
        }
    }
}
