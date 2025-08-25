# Stage 1: Base image for runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8082
EXPOSE 8083

# Stage 2: Build the application
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copy project file and restore dependencies
COPY WebAppJenkins.csproj ./
RUN dotnet restore WebAppJenkins.csproj

# Copy all source files
COPY . .

# Build the application
RUN dotnet build WebAppJenkins.csproj -c $BUILD_CONFIGURATION -o /app/build

# Stage 3: Publish the application
FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish WebAppJenkins.csproj -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# Stage 4: Final runtime image
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "WebAppJenkins.dll"]
