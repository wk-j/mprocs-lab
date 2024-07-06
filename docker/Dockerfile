# Use the official .NET 8 SDK image as a build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["src/MyApp/MyApp.csproj", "src/MyApp/"]
RUN dotnet restore "src/MyApp/MyApp.csproj"
COPY . .
WORKDIR "/src/src/MyApp"
RUN dotnet build "MyApp.csproj" -c Release -o /app/build

# Publish the app
FROM build AS publish
RUN dotnet publish "MyApp.csproj" -c Release -o /app/publish

# Use the official .NET 8 runtime image as a runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Set the default port
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

# Run the application
ENTRYPOINT ["dotnet", "MyApp.dll"]