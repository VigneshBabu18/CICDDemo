# Use ASP.NET runtime image as the base for final container
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8082
EXPOSE 8083

# Use .NET SDK image to build the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy the project file and restore dependencies
COPY WebAppJenkins/WebAppJenkins.csproj WebAppJenkins/
RUN dotnet restore WebAppJenkins/WebAppJenkins.csproj

# Copy the entire source code
COPY . .

# Build the application
WORKDIR /src/WebAppJenkins
RUN dotnet build WebAppJenkins.csproj -c $BUILD_CONFIGURATION -o /app/build

# Publish the application
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish WebAppJenkins.csproj -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Final image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebAppJenkins.dll"]
