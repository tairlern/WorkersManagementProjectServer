#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Server.APi/Server.APi.csproj", "Server.APi/"]
COPY ["Server.Core/Server.Core.csproj", "Server.Core/"]
COPY ["Server.Data/Server.Data.csproj", "Server.Data/"]
COPY ["Server.Service/Server.Service.csproj", "Server.Service/"]
RUN dotnet restore "./Server.APi/Server.APi.csproj"
COPY . .
WORKDIR "/src/Server.APi"
RUN dotnet build "./Server.APi.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "./Server.APi.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Server.APi.dll"]