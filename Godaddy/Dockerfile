#See https://aka.ms/customizecontainer to learn how to customize your debug container and how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:8.0-preview AS base
USER app
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0-preview AS build
WORKDIR /src
COPY ["Godaddy/Godaddy.csproj", "Godaddy/"]
RUN dotnet restore "Godaddy/Godaddy.csproj"
COPY . .
WORKDIR "/src/Godaddy"
RUN dotnet build "Godaddy.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "Godaddy.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Godaddy.dll"]