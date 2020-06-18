FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-bionic  AS base
WORKDIR /app

# For Rotativa SDK
RUN apt-get update && apt-get install -y --no-install-recommends fontconfig libfreetype6 libjpeg-turbo8 libpng16-16 libx11-6 libxcb1 libxext6 libxrender1 xfonts-75dpi xfonts-base libgdiplus libc6-dev

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-bionic AS build
WORKDIR /src
COPY ["rotativa-dotnet-core.csproj", ""]
RUN dotnet restore "./rotativa-dotnet-core.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "rotativa-dotnet-core.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "rotativa-dotnet-core.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "rotativa-dotnet-core.dll"]