[dev-cert](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-dev-certs)
```bash
# remove existing certs
dotnet dev-certs https --clean

# create new cert for localhost
dotnet dev-certs https --trust

# import existing cert for localhost
dotnet dev-certs https --clean -i /path/to/file.pfx -p password
```

serve
```bash
dotnet run --project /path/to/project.csproj
dotnet watch -lp profile
```
