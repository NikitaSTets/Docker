#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app

EXPOSE 4500

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["testDockerLinux.csproj", "testDockerLinux/"]
COPY ["Program.cs", "testDockerLinux/"]
COPY ["Startup.cs", "testDockerLinux/"]
COPY ["appsettings.Development.json", "testDockerLinux/"]
COPY ["appsettings.json", "testDockerLinux/"]

RUN dotnet restore "testDockerLinux/testDockerLinux.csproj"
COPY . .
WORKDIR "/src/testDockerLinux"
RUN dotnet build "testDockerLinux.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "testDockerLinux.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "testDockerLinux.dll"]