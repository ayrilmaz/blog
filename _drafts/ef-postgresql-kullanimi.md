---
layout: post
title:  "EF - Postgresql kullanımı - 1"
tags: ef postgresql npgsql c#
categories: general
---
# Projey Oluşturma ve Kurulum
.Net 8 kullanıldığını varsayıyorum. Kapsamı geniş tutmak için webapi template üzerinden ilerleyeceğim.

``` shell
dotnet new webapi --use-controllers -n EfPostgresql -o ef-postgre-sql
```

Gerekli paketleri kuralım;

``` shell
dotnet add package Microsoft.EntityFrameworkCore --version 8.0.8
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL --version 8.0.4
```

> Migration kullanacanız Tools vs Design paketlerini de kurmalısınız.

Örnek varlık oluşturma:
Proje içinde Data adında bir klasör oluşturalım ve Person.cs dosyasını oluşturalım.

``` csharp
public class Person
{
    public int Id { get; set; }
    public string Fullname { get; set; }
    public string Email { get; set; }
    public DateTime Birthdate { get; set; }
    public decimal Salary { get; set; }
    public bool IsActive { get; set; }
}
```

Person sınıfına ait özelliklerin veritabında kolona karşılık geldiğini biliyoruz. Şimdi onları konfigure edelim.

PersonConfigure.cs dosyasını aynı klaöserde oluşturalım.

``` csharp
public class PersonConfiguration : IEntityTypeConfiguration<Person>
{
    public void Configure(EntityTypeBuilder<Person> builder)
    {
        //bu projede bu özellikle gerekli değil. Tablo adı farklı olsaydı gerekli olacaktı
        builder.ToTable("Person");

        //primary key
        builder.HasKey(t => t.Id);

        //otomatik artan değer
        builder.Property(t => t.Id).ValueGeneratedOnAdd().UseIdentityAlwaysColumn();
        //not null ve maks karakter sayısı
        builder.Property(t => t.Fullname).IsRequired().HasMaxLength(150);
        builder.Property(t => t.Email).IsRequired().HasMaxLength(250);
        //burada da sayısal değeri nasıl saklayacağını belirttik
        builder.Property(t => t.Salary).IsRequired().HasPrecision(8, 2);
    }
}
```
Daha fazla detay için [npgsql](https://www.npgsql.org/efcore/index.html)

> Konfigürasyon özelliklerinde önemli olan string ifadeler. Bunları ayarlamaz iseniz veritabının maks değeri atanır. Diğer alanlar zorunlu olduğu için yazmadık. Yapı tiplerinin sonuna "?" koyarak nullable yapılabilir.

DbContext sınıfımızı oluşturalım. Bu sınıf yardımı ile veritabanı özelliklerini belirleyeceğiz. Yine aynı klaösere EfDbContext.cs dosyasını ekleyelim.

``` csharp
public class EfDbContext(DbContextOptions<EfDbContext> options) : DbContext(options)
{
    //Person tablomuz. Genelde çoğul isim vermeyi tercih ediyorum
    public DbSet<Person> People { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.EnableSensitiveDataLogging();
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        //konfiigürasyın sınıflarını otomatik uygular
        modelBuilder.ApplyConfigurationsFromAssembly(
            Assembly.GetExecutingAssembly(),
            p => p.GetInterfaces().Any(
                c => c.IsGenericType && c.GetGenericTypeDefinition() == typeof(IEntityTypeConfiguration<>)));
    }
}
```
