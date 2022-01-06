#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
RUN pwd
COPY ["*.sln", "./"]
COPY ["NexusWeb/NexusWeb.csproj", "./NexusWeb/"]
RUN dotnet restore "./NexusWeb/NexusWeb.csproj"
#RUN dotnet restore "NexusWeb.csproj"
COPY . .
WORKDIR /src/NexusWeb/.
RUN dotnet build "NexusWeb.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "NexusWeb.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "NexusWeb.dll"]