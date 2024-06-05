# JSON

deserialize JSON
```cs
// .Net framework < 4.7.2
Newtonsoft.Json.JsonConvert.SerializeObject(new {foo = "bar"})
// .Net framework >= 4.72
System.Text.Json.JsonSerializer.Serialize(new {foo = "bar"})
```

# Validator 

add validation attributes
```cs
using System.ComponentModel.DataAnnotations;

public class Movie
{
	[Required]
	public string Title { get; set; }

	[Range(2000, 2022)]
	public int Year { get; set; }
}

```

basic validation
```cs
using System.ComponentModel.DataAnnotations;

var movieJson = "{\"Year\":0}";
var movie = JsonSerializer.Deserialize<Movie>(movieJson);

// throws error on first validation error
// by default only check for [Required] attribute
// to check for all validations use validateAllProperties: true
Validator.ValidateObject(movie, new ValidationContext(movie), validateAllProperties: true);
```

verbose validation
```cs
using System.ComponentModel.DataAnnotations;

var movieJson = "{\"Year\":0}";
var movie = JsonSerializer.Deserialize<Movie>(movieJson);

var validationErrors = new List<ValidationResult>();
if (!Validator.TryValidateObject(movie, new ValidationContext(movie), validationErrors, validateAllProperties: true))
{
    // Look at all of the validation errors
    foreach (var error in validationErrors)
    {
        Console.WriteLine(error.ErrorMessage);
    }
}
```