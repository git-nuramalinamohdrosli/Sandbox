#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["SandboxTune/Server/SandboxTune.Server.csproj", "SandboxTune/Server/"]
#COPY ["SandboxTune/Client/SandboxTune.Client.csproj", "SandboxTune/Client/"]
#COPY ["SandboxTune/Shared/SandboxTune.Shared.csproj", "SandboxTune/Shared/"]
RUN dotnet restore "SandboxTune/Server/SandboxTune.Server.csproj"
COPY . .
WORKDIR "/src/SandboxTune/Server"
RUN dotnet build "SandboxTune.Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "SandboxTune.Server.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "SandboxTune.Server.dll"]