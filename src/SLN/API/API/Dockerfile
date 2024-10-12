# See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

# This stage is used when running from VS in fast mode (Default for Debug configuration)
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080


# This stage is used to build the service project
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src

# Copiando o arquivo .csproj e restaurando as dependências
COPY src/SLN/API/API.csproj API/
RUN dotnet restore "API/API.csproj"

# Copiando o restante dos arquivos da pasta API
COPY src/SLN/API/ .

RUN dotnet build "API/API.csproj" -c $BUILD_CONFIGURATION -o /app/build
RUN dotnet publish "API/API.csproj" -c $BUILD_CONFIGURATION -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "API.dll"]

