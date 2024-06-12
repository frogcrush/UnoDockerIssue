#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Directory.Packages.props", "."]
COPY ["Directory.Build.props", "."]
COPY ["Directory.Build.targets", "."]
COPY ["nuget.config", "."]
COPY ["UnoApp1.Server/UnoApp1.Server.csproj", "UnoApp1.Server/"]
COPY ["UnoApp1/UnoApp1.csproj", "UnoApp1/"]
COPY ["UnoApp1.DataContracts/UnoApp1.DataContracts.csproj", "UnoApp1.DataContracts/"]
RUN dotnet restore "./UnoApp1.Server/UnoApp1.Server.csproj"
COPY . .
WORKDIR "/src/UnoApp1.Server"
RUN dotnet build "./UnoApp1.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./UnoApp1.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "UnoApp1.Server.dll"]