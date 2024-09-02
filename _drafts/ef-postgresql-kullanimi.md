---
layout: post
title:  "EF - Postgresql kullanımı - 1"
tags: ef postgresql npgsql c#
categories: general
---
Bu rehberde, kapsamı geniş tutmak adına webapi şablonunu kullanarak ilerleyeceğiz.

# Proje Oluşturma ve Kurulum
cli ile yeni bir proje oluşturalım.
``` shell
dotnet new webapi --use-controllers -n EfPostgresql -o ef-postgre-sql
```

Gerekli paketleri yükleyelim:

``` shell
dotnet add package Microsoft.EntityFrameworkCore --version 8.0.8
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL --version 8.0.4
```

> Migration kullanmayı planlıyorsanız, Tools ve Design paketlerini de yüklemelisiniz.

**Örnek varlık oluşturma**
Veri ile ilgili dosyalarımızı düzenlemek için Data adında bir klasör oluşturalım ve bu klasöre Person.cs dosyasını ekleyelim.

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

Person sınıfındaki özelliklerin veritabanında bir kolona karşılık geldiğini biliyoruz. Şimdi onları konfigure edelim.

PersonConfigure.cs dosyasını aynı klasörde oluşturalım.

``` csharp
public class PersonConfiguration : IEntityTypeConfiguration<Person>
{
    public void Configure(EntityTypeBuilder<Person> builder)
    {
        //bu projede bu özellikle gerekli değil. Tablo adı farklı olsaydı gerekli olacaktı
        builder.ToTable("Person");

        //Primary key tanımlaması
        builder.HasKey(t => t.Id);

        //otomatik artan değer
        builder.Property(t => t.Id).ValueGeneratedOnAdd().UseIdentityAlwaysColumn();
        //not null ve maks karakter sayısı
        builder.Property(t => t.Fullname).IsRequired().HasMaxLength(150);
        builder.Property(t => t.Email).IsRequired().HasMaxLength(250);
        //Sayısal değerin nasıl saklanacağını belirtiyoruz
        builder.Property(t => t.Salary).IsRequired().HasPrecision(8, 2);
    }
}
```
> Daha fazla detay için [npgsql dökümantasyonuna](https://www.npgsql.org/efcore/index.html) göz atabilirsiniz.

> Konfigürasyon özelliklerinde, özellikle string ifadelerin ayarlanması önemlidir. Aksi takdirde, veritabanının varsayılan maksimum değeri atanır. Diğer alanlar zorunlu olduğu için burada belirtilmemiştir. Yapı tiplerinin sonuna ? koyarak nullable yapılabilir.

**DbContext Sınıfını Oluşturma**
Veritabanı özelliklerini belirlemek için DbContext sınıfımızı oluşturalım. Yine aynı klasöre EfDbContext.cs dosyasını ekleyelim.

``` csharp
public class EfDbContext(DbContextOptions<EfDbContext> options) : DbContext(options)
{
    //Person tablomuz. Genelde çoğul isim vermeyi tercih ediyorum
    public DbSet<Person> People { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //sadece geliştirme aşamasında etkinleştirmekte fayda var
        optionsBuilder.EnableSensitiveDataLogging();
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        //Konfigürasyon sınıflarını otomatik olarak uygular
        modelBuilder.ApplyConfigurationsFromAssembly(
            Assembly.GetExecutingAssembly(),
            p => p.GetInterfaces().Any(
                c => c.IsGenericType && c.GetGenericTypeDefinition() == typeof(IEntityTypeConfiguration<>)));
    }
}
```
